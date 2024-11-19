---
aliases: 
tags: 
description:
title: aws ECR 도커 이미지 레지스트리
created: 2024-11-19T16:18:17
updated: 2024-11-19T16:36:33
---

## Pulling from ECR

- [docs / Pulling from ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-pull-ecr-image.html)
- [docs / Private Registry Authentication](https://docs.aws.amazon.com/AmazonECR/latest/userguide/registry_auth.html) ^private-registry-auth

[ECR Credential Helper](https://github.com/awslabs/amazon-ecr-credential-helper) 를 사용하는 방법과 [Authorization Token](https://docs.aws.amazon.com/AmazonECR/latest/userguide/registry_auth.html#registry-auth-token)을 사용하여 직접 `docker login`을 하는 방법이 있다.  
도커 로그인을 하는 방법은 다음과 같은 단점이 있다:

1. 토큰의 유효기간이 **1시간**으로 제한되기 때문에 자주 갱신해야 한다.
2. 이를 자동화하려면 별도의 **스크립트**나 **CI/CD 설정**이 필요하다.

반면, Credential Helper는 다음의 이점이 있다:

- 인증을 자동으로 처리하므로 토큰 갱신 부담이 없다.
- 사용이 간편하며, 설치 후 기본 Docker 명령어로 바로 사용할 수 있다.  

하지만, Credential Helper는 다음과 같은 주의점이 있다:

- **추가적인 프로그램 설치**가 필요하므로, 의존성에 민감하거나 보안 정책상 제약이 있는 환경에서는 사용하지 않을 수 있다.

**유스케이스로 살펴보자**:

- **개발 환경 (로컬 컴퓨터)**: 
  - 이미지를 간편하게 `push`, `pull` 하고 싶다면 Credential Helper를 사용하는 것이 효율적이다.
- **프로덕션 환경 (EC2 등)**:
  - EC2에서 새 이미지를 자동으로 풀링하고 배포하는 경우, Authorization Token 방식이 적합하다.  
    - `AWS CLI`를 사용하여 토큰을 주기적으로 갱신하는 스크립트를 설정하거나, **Instance Role**과 함께 사용하면 보안성을 높일 수 있다.  

**예시**:

```bash
# Authorization Token 방식 (수동 로그인)
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com
```

```bash
# Credential Helper 설정
cat <<EOF >> ~/.docker/config.json
{
  "credHelpers": {
    "<account_id>.dkr.ecr.<region>.amazonaws.com": "ecr-login"
  }
}
EOF
```

---

## Pushing to ECR

- [docs / Pushing docker image](https://docs.aws.amazon.com/AmazonECR/latest/userguide/docker-push-ecr-image.html)

### Steps for Pushing

1. **Authenticate to ECR**:
   - Credential Helper 또는 Authorization Token을 사용하여 인증.

2. **Tag the Docker Image**:

   ```bash
   docker tag <image>:<tag> <account_id>.dkr.ecr.<region>.amazonaws.com/<repository_name>:<tag>
   ```

3. **Push the Docker Image**:

   ```bash
   docker push <account_id>.dkr.ecr.<region>.amazonaws.com/<repository_name>:<tag>
   ```

### Best Practices

- **Use Lifecycle Policies**:
  - ECR 저장소에 불필요한 이미지가 누적되지 않도록 [Lifecycle Policies](https://docs.aws.amazon.com/AmazonECR/latest/userguide/LifecyclePolicies.html)를 설정하여 오래된 이미지를 자동으로 삭제.
  
- **Automate Pushing with CI/CD**:
  - GitHub Actions, Jenkins, AWS CodePipeline 등과 통합하여 이미지를 빌드하고 ECR로 푸시하는 파이프라인을 구성.
  - 예를 들어, GitHub Actions에서 AWS CLI를 사용한 예시는 다음과 같다:

    ```yaml
    - name: Log in to Amazon ECR
      run: |
        aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com
    - name: Build, Tag, and Push
      run: |
        docker build -t <repository_name>:<tag> .
        docker tag <repository_name>:<tag> <account_id>.dkr.ecr.<region>.amazonaws.com/<repository_name>:<tag>
        docker push <account_id>.dkr.ecr.<region>.amazonaws.com/<repository_name>:<tag>
    ```
