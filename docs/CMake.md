---
description:
aliases: 
tags: 
created: 2023-04-12T22:48:18
updated: 2023-09-02T22:39:05
title: CMake
---
- [CMake Packages](https://cmake.org/cmake/help/latest/manual/cmake-packages.7.html#creating-relocatable-packages)
- <https://github.com/methylDragon/coding-notes/blob/master/CMake/01%20CMake%20-%20Basics%20and%20Scripting.md>
- [Tutorial](https://preshing.com/20170522/learn-cmakes-scripting-language-in-15-minutes/)
- [한글번역 - CMake 도움되는 글](https://gist.github.com/yuntaek/589a16002ccaf07cdcabf8bdd132f71f)
- <https://cgold.readthedocs.io/en/latest/overview/cmake-can.html>

## CMake Crash Course

[CMake 할때 쪼오오금 도움이 되는 문서 · GitHub](https://www.notion.so/CMake-GitHub-fc4ecb3301b84b1fbe666464d4b43b69?pvs=21)

[3. Tutorials - CGold 0.1 documentation](https://cgold.readthedocs.io/en/latest/tutorials.html)

[Learn CMake's Scripting Language in 15 Minutes](https://preshing.com/20170522/learn-cmakes-scripting-language-in-15-minutes/)

[coding-notes/01 CMake - Basics and Scripting.md at master · methylDragon/coding-notes](https://github.com/methylDragon/coding-notes/blob/master/CMake/01%20CMake%20-%20Basics%20and%20Scripting.md)

[CMake Tutorial - CMake 3.24.0-rc4 Documentation](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)

## 목표

VSCode의 C/C++ 플러그인을 사용하면 .vscode/tasks.json 파일과 .vscode/launch.json 파일을 통해 내 컴퓨터에 들어있는 gcc나 clang과 같은 빌드/디버그 프로그램을 실행시켜 IDE처럼 쓸 수 있다. 아직까진 직접 gcc와 gdb에 연결이 되어있는 반면에, CMake를 사용하여 원하는 폴더마다 CMakeLists.txt 파일을 만들어 각 폴더 별 빌드규칙을 설정하고 F5만 눌러도 CMake configure → build → make → exec이 연달아 일어나도록 구성할 것이다.

덩달아 CMakeLists.txt 안에는 GoogleTest를 활용한 CTest 관련 코드도 들어있어 여러가지 configuration에 따라 빌드를 할 건지, 테스트를 할 건지 선택할 수 있도록 구성할 것이다.

## 다른 CMakeLists.txt 실행시키기 가능할까?

루트에서부터 재귀적으로 CMakeLists.txt 파일이 하위 파일을 컴파일 해서 각각 테스트용 바이너리, 라이브러리 생성, 실행파일 생성을 따로따로 담당하게 만들 수 없을까?

[https://stackoverflow.com/questions/16725657/cmake-depending-on-another-cmake-project](https://stackoverflow.com/questions/16725657/cmake-depending-on-another-cmake-project)

## 자동으로 파일 fetch하기

[How to add source files in another folder](https://stackoverflow.com/a/25610377)

```cpp
Source
    |
    |_ CMakeLists.txt   
    |    > project(MyProgram)
    |    > cmake_minimum_required(VERSION 3.8)
    |    > add_subdirectory("Dir1")
    |    > add_subdirectory("Dir2")
    |
    |_ Dir1   
    |     |_ CMakeLists.txt   
    |         > file(GLOB Sources "*.cpp")
    |         > add_library(Dir1 STATIC ${Sources})
    |_ Dir2   
          |_ CMakeLists.txt   
              > file(GLOB Sources "*.cpp")
              > add_executable(MyProgram ${Sources})
              > target_link_libraries(MyProgram Dir1)
```

## 최하위 디렉토리 이름만 꺼내기

[CMake - How to get the SECOND LAST in the directory name?](https://stackoverflow.com/questions/34296269/cmake-how-to-get-the-second-last-in-the-directory-name#34311368)

```cpp
get_filename_component(a_dir "${some_file}" PATH)
get_filename_component(a_last_dir "${a_dir}" NAME)
```

## CMake 정리

[https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html](https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html) 다른 블로그 글들을 읽으면서 부정확한 정보들로 인해 머리 아프지 말고 차라리 공식 문서를 읽자. 아래는 내가 읽으면서 정리한 것 뿐이다.

타겟을 정의하기 위해선 `add_executable()` 과 `add_library()` 커맨드를 사용해야 한다. 타겟에 대한 의존성은 `target_link_libraries()` 커맨드에서 관리한다.

타겟에 대한 빌드 세부사항과 사용 요구사항 (Build specifications and the usage requirements of binary targets)을 정의하는 커맨드는 다음과 같다.

- `target_include_directories()` : 주어진 타겟을 컴파일할 때 포함할 디렉토리를 명시한다. `INCLUDE_DIRECTORIES` 프로퍼티 값을 설정하며, `-I` 옵션에 사용된다.
- `target_compile_definitions()` : 컴파일 시에 선언할 `#define` 문을 정의할 수 있다. 타겟은 반드시 `add_executable()` 또는 `add_library()` 에 의해 선언되어있어야 한다. `COMPILE_DEFINITIONS` 프로퍼티 값을 설정하며, `-D` 옵션에 사용된다.
- `target_compile_options()` : 컴파일 시에 파라메터로 올 옵션들을 정의할 수 있다. 마찬가지로 타겟이 선언된 뒤에 사용이 가능하다.

**Build Configurations**

[https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html#build-configurations](https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html#build-configurations)

Release, Debug모드가 곧 Build Configuration이다. 이것 말고도 컴파일 시에 이것저것 조정할 것들에 대한 선택권한을 줄 수 있다. 예를 들어 Makefile generator를 Ninja로 할 건지, Visual Studio로 할 건지 등에 대한 선택이 가능해진다. Build Configuration을 generator expression으로 설정하게 되면 `CMAKE_BUILD_TYPE`은 무시된다. 

CMake GUI를 열어 외부 라이브러리나 프로그램을 컴파일 할 일이 있다면 꼭 Configuration을 먼저하고 그 다음에 Generate한 기억이 있을 것이다. 

문서는 multi-config 타입의 경우에 if 문으로 빌드타입을 처리하지 말라고 강조한다. config time에서야 겨우 가능한 configuration들을 알고 빌드단계 전까진 configuration들의 값을 알 수 없기 때문이라는데 무슨 소리인지 잘 모르겠다.

```bash
# 안 좋은 예시. This is wrong for multi-config generators because they don't use
# and typically don't even set CMAKE_BUILD_TYPE
string(TOLOWER ${CMAKE_BUILD_TYPE} build_type)
if (build_type STREQUAL debut)
	target_compile_definitions(exe1 PRIVATE DEBUG_BUILD)
endif()

# 좋은 예시. Works correctly for both single and multi-config generators
target_compile_definitions(exe1 PRIVATE
	$<$<CONFIG:Debug>:DEBUG_BUILD>
)
```

## Generator Expression

[https://cmake.org/cmake/help/latest/manual/cmake-generator-expressions.7.html](https://cmake.org/cmake/help/latest/manual/cmake-generator-expressions.7.html)

Generator Expression은 타겟 프로퍼티를 정의할 때 사용하는데, 이때 타겟 프로퍼티로 `LINK_LIBRARIES`, `INCLUDE_DIRECTORIES`, `COMPILE_DEFINITIONS` 등이 있다. 이 프로퍼티를 전파(populate)하는 데 사용될 수도 있다. populate 키워드는 아마도 PRIVATE, PUBLIC, INTERFACE와 연관이 있는 것 같은데 그건 나중에 보자. 

Generator Expression은 조건부 링킹, 조건부 definition, 조건부 include 등에 사용된다. 해당 ‘조건’들은 사전에 정의되어있고, 빌드속성이나 타겟 프로퍼티, 플랫폼 등에 의해 결정되고 쿼리될 수 있다고 한다. 메이크파일에서의 특수변수 `$>` , `$@`라던가 정규표현식 같은 것으로 이해된다. 아님말고.

`$<...>` 형태의 모든 표현식은 generator expression으로 평가된다. 이때 안에는 조건문이 들어갈 수도 있고 단순 문자열, 타겟이 들어갈 수 있다.

조건문이 너무 많으니까 필요할 때마다 문서 봐 가면서 작성할 것.

**string-valued generator expressions**

```makefile
include_directories(/usr/include/$<CXX_COMPILER_ID>/)
```

여기에서 기본으로 설정된 컴파일러의 이름에 따라 `/usr/include/GNU/` 또는 `/usr/include/Clang` 등으로 치환될 수 있다. 아니, 그냥 저 프로퍼티를 그냥 정의한 거 아닌가?

**Path Decompositions**

CMake에는 함수란 개념이 조금 모호해서 변수 자리에 아무렇게나 함수를 박아넣을 수는 없다. 이 점을 보완한 게 Generator Expression인 것 같다. 이렇게라도 함수적으로 표현한다면 가독성이 좋아질 테니깐. 

[https://cmake.org/cmake/help/latest/command/cmake_path.html](https://cmake.org/cmake/help/latest/command/cmake_path.html#command:cmake_path) `cmake_path()` 에서 주어지는 다양한 Decomposition과 Query, Modification 등등의 기능을 원래는 별도의 라인과 별도의 변수명으로 적어야 했지만 Generator Expression의 도움으로 Path를 분해하고 쿼리하고 조작하는 등의 행위가 가능해졌다.

```
$<PATH:GET_ROOT_NAME,path>
$<PATH:GET_ROOT_DIRECTORY,path>
$<PATH:GET_ROOT_PATH,path>
$<PATH:GET_FILENAME,path>
$<PATH:GET_EXTENSION[,LAST_ONLY],path>
$<PATH:GET_STEM[,LAST_ONLY],path>
$<PATH:GET_RELATIVE_PART,path>
$<PATH:GET_PARENT_PATH,path>
```

**Variable Queries**

[https://cmake.org/cmake/help/latest/manual/cmake-generator-expressions.7.html#id2](https://cmake.org/cmake/help/latest/manual/cmake-generator-expressions.7.html#id2)

대표적으로 C++ 컴파일러 ID를 쿼리하는 `$<CXX_COMPILER_ID>` 는 다음 리스트 중에서 현재 CMake가 사용하는 컴파일러 아이디를 리턴한다.

```
Absoft = Absoft Fortran (absoft.com)
ADSP = Analog VisualDSP++ (analog.com)
AppleClang = Apple Clang (apple.com)
ARMCC = ARM Compiler (arm.com)
ARMClang = ARM Compiler based on Clang (arm.com)
Bruce = Bruce C Compiler
CCur = Concurrent Fortran (ccur.com)
Clang = LLVM Clang (clang.llvm.org)
Cray = Cray Compiler (cray.com)
Embarcadero, Borland = Embarcadero (embarcadero.com)
Flang = Classic Flang Fortran Compiler (https://github.com/flang-compiler/flang)
LLVMFlang = LLVM Flang Fortran Compiler (https://github.com/llvm/llvm-project/tree/main/flang)
Fujitsu = Fujitsu HPC compiler (Trad mode)
FujitsuClang = Fujitsu HPC compiler (Clang mode)
G95 = G95 Fortran (g95.org)
GNU = GNU Compiler Collection (gcc.gnu.org)
GHS = Green Hills Software (www.ghs.com)
HP = Hewlett-Packard Compiler (hp.com)
IAR = IAR Systems (iar.com)
Intel = Intel Compiler (intel.com)
IntelLLVM = Intel LLVM-Based Compiler (intel.com)
LCC = MCST Elbrus C/C++/Fortran Compiler (mcst.ru)
MSVC = Microsoft Visual Studio (microsoft.com)
NVHPC = NVIDIA HPC SDK Compiler (nvidia.com)
NVIDIA = NVIDIA CUDA Compiler (nvidia.com)
OpenWatcom = Open Watcom (openwatcom.org)
PGI = The Portland Group (pgroup.com)
PathScale = PathScale (pathscale.com)
SDCC = Small Device C Compiler (sdcc.sourceforge.net)
SunPro = Oracle Solaris Studio (oracle.com)
TI = Texas Instruments (ti.com)
TinyCC = Tiny C Compiler (tinycc.org)
XL, VisualAge, zOS = IBM XL (ibm.com)
XLClang = IBM Clang-based XL (ibm.com)
IBMClang = IBM LLVM-based Compiler (ibm.com)
```

## Packages

패키지, 인스톨은 시기상조다. 온디맨드 식으로 필요할 때 그때 공부하는 쪽으로 진행하자. 이러다간 끝도 없겠다.

[https://cmake.org/cmake/help/latest/manual/cmake-packages.7.html](https://cmake.org/cmake/help/latest/manual/cmake-packages.7.html#creating-relocatable-packages)

**find_package & 의존성 관리 가이드**

[https://cmake.org/cmake/help/latest/command/find_package.html](https://cmake.org/cmake/help/latest/command/find_package.html#command:find_package)

[https://cmake.org/cmake/help/latest/guide/using-dependencies/index.html](https://cmake.org/cmake/help/latest/guide/using-dependencies/index.html)

## Project

[https://cmake.org/cmake/help/latest/command/project.html](https://cmake.org/cmake/help/latest/command/project.html)

프로젝트는 `PROJECT_NAME` 변수에 설정이 된다. 버전을 Major, Minor, Patch, Tweak 순으로 설정할 수 있다. 여러개의 프로젝트를 만들 수 있으며, 이때 가장 높은 레벨 프로젝트명이 `CMAKE_PROJECT_NAME` 변소에 설정이 된다.
