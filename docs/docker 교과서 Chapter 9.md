---
aliases: 
tags: 
description:
title: docker 교과서 Chapter 9
created: 2024-11-02T16:54:14
updated: 2024-11-05T00:06:58
---

## What is Prometheus?

프로메테우스는 도커 컨테이너의 투명성을 확보하기 위해 컨테이너의 상태를 수집 및 수치화 하여 공통 API를 통해 통신하도록 만들어진 모니터링 툴이다. 따라서 어느 컨테이너건 프로메테우스가 정의한 API 규칙만 준수하면 개별 컨테이너는 물론 분산환경에서의 상태 추적 및 관리가 용이해진다.

## Monitoring Docker Engine

[docs.docker.com # Collect Docker metrics with Prometheus](https://docs.docker.com/engine/daemon/prometheus/) 이 문서는 도커 컨테이너가 아닌, 도커 엔진 자체를 프로메테우스를 통하여 모니터링하는 방법에 대해 설명한다. 아래는 `127.0.0.1:9323/metrics`에 접속했을때 나타나는 프로메테우스 포맷 출력결과 일부를 복사한 것이다

```
# HELP builder_builds_failed_total Number of failed image builds
# TYPE builder_builds_failed_total counter
builder_builds_failed_total{reason="build_canceled"} 0
builder_builds_failed_total{reason="build_target_not_reachable_error"} 0
builder_builds_failed_total{reason="command_not_supported_error"} 0
builder_builds_failed_total{reason="dockerfile_empty_error"} 0
builder_builds_failed_total{reason="dockerfile_syntax_error"} 0
builder_builds_failed_total{reason="error_processing_commands_error"} 0
builder_builds_failed_total{reason="missing_onbuild_arguments_error"} 0
builder_builds_failed_total{reason="unknown_instruction_error"} 0
...
```

개중엔 현재 컨테이너 정보들도 볼 수 있다:

```
# HELP engine_daemon_container_states_containers The count of containers in various states
# TYPE engine_daemon_container_states_containers gauge
engine_daemon_container_states_containers{state="paused"} 0
engine_daemon_container_states_containers{state="running"} 1
engine_daemon_container_states_containers{state="stopped"} 0
```

## Monitoring Docker Containers

도커 컨테이너를 모니터링하려면 애플리케이션에 프로메테우스 메트릭 API를 제공해야한다. 다행히도 다양한 언어별로 프로메테우스 라이브러리가 존재하며, 각 라이브러리별로 기본으로 제공해주는 메트릭이 존재할 뿐만 아니라 사용자가 원하는 수치를 다양한 타입 (게이지, 카운터 등)으로 추가하는것이 가능하다.

> [!question] 그렇다면 어떤 수치를 제공해주는 것이 좋을까?

- 도메인에 따라 다르겠지만, 대체로 **응답시간**을 수치로 제공해주는 것이 좋다. 사용자와 애플리케이션 간의 응답시간도 좋고 다른 서비스와의 응답시간도 좋다. 이 측정값으로 애플리케이션과 커뮤니케이션 하는 액터들간의 상태나 속도 등을 파악할 수 있을 뿐더러, 이상징후를 빠르게 알아차릴 수 있다.
- **로깅**도 훌륭한 방법이 될 수 있다. 복잡한 정보를 담아야 하는 로그는 데이터베이스에 저장해야겠지만, 500에러가 발생한 횟수같이 단순한 로그의 경우 CPU 타임이나 디스크 용량, 메모리를 확연히 아끼면서 원하는 정보는 시각화 할 수 있는 중요한 요소이다.

**golang**

```go
import (
	"github.com/prometheus/client_golang/prometheus/promhttp"
	"github.com/prometheus/client_golang/prometheus"
	...
)

func main() {
	//create Prometheus metrics
	inFlightGauge := prometheus.NewGauge(prometheus.GaugeOpts{
		Name: "image_gallery_in_flight_requests",
		Help: "Image Gallery - in-flight requests",
	})
	requestCounter := prometheus.NewCounterVec(
		prometheus.CounterOpts{
			Name: "image_gallery_requests_total",
			Help: "Image Gallery - total requests",
		},
		[]string{"code", "method"},
	)

	prometheus.MustRegister(inFlightGauge, requestCounter)
	...
	wrappedIndexHandler := promhttp.InstrumentHandlerInFlight(inFlightGauge,
							promhttp.InstrumentHandlerCounter(requestCounter, indexHandler))

	http.Handle("/metrics", promhttp.Handler())
}
```

**nodejs**

```js
const restify = require("restify");
const prom = require("prom-client");
const log = require("./log");
const os = require("os");

const accessCounter = new prom.Counter({
  name: "access_log_total",
  help: "Access Log - total log requests"
});

const clientIpGauge = new prom.Gauge({
  name: "access_client_ip_current",
  help: "Access Log - current unique IP addresses"
});

//setup Prometheus with hostname label:
const defaultLabels = { hostname: os.hostname() };
prom.register.setDefaultLabels(defaultLabels);
prom.collectDefaultMetrics();

function respond(req, res, next) {
  //metrics:
  accessCounter.inc();
  ipAddresses.push(req.body.clientIp);
  let uniqueIps = Array.from(new Set(ipAddresses));
  clientIpGauge.set(uniqueIps.length);

  res.send(201, "Created");
  next();
}

var ipAddresses = new Array();

server.get("/metrics", function(req, res, next) {
  res.end(prom.register.metrics());
});
```

### Prometheus Graph

프로메테우스에 들어가 PromQL 이라는 쿼리를 작성하게되면 원하는 결과를 시간순 또는 객체순으로 관리해서 볼 수 있다. 아래는 `accesslog`라는 도커 컨테이너를 3개로 스케일아웃한 상태에서 10번 연속으로 요청을 보냈을때 출력결과이다. 

![[ksnip_20241104-225109.png]]

`sum` 쿼리를 사용하고 `without`으로 원치 않는 속성을 배제할 수 있다. 아래는 `accesslog`의 모든 로그의 개수를 그래프로 그리도록 만들었다.

![[ksnip_20241104-230001.png]]

이때 `prometheus.yml` 파일에서 `accesslog`를 스크래핑하는 세팅을 `dns_sd_configs`라고 주었기 때문에 하나의 도메인 네임으로 여러 IP주소를 얻어와 스크래핑 할 수 있었다.

```yml
scrape_configs:
  - job_name: "access-log"
    metrics_path: /metrics
    scrape_interval: 3s
    dns_sd_configs:
      - names:
          - accesslog
        type: A
        port: 80
  ...
```

## What is Grafana?

그라파나는 애플리케이션 투명성을 보장하기 위한 대시보드 도구로, 프로메테우스에 promQL 요청을 보내 이를 시각화 할 수 있는 능력을 가지고 있다. 어떤 정보를 어떻게 표현해야할지에 대한 지혜가 필요하다면 구글의 [[Site Reliability Engineering]] 문서를 확인해보자. 

>  **서비스 수준 지표(SLI)**: SLI는 서비스의 성능을 나타내는 지표로, 지연 시간, 가용성, 오류율과 같은 항목이 포함됩니다.

**grafana dashboard**

![[ksnip_20241104-231935.png]]

**graphana panel editting**

![[ksnip_20241104-232434.png]]
