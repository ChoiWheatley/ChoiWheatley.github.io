---
aliases: 
tags: 
description:
title: Library {C}
created: 2023-09-02T13:44:48
updated: 2023-09-10T11:02:04
---
- parent: [[0017 C 🍎]]
- 설명: shared library를 만들고 링크하는 법을 터득한다.
- 체크: Done

CMake를 사용하면서 미래에 사용하게 될 여러 라이브러리들을 함께 컴파일하고 링크하기 위해 add_subdirectory(), add_library(), [target_]link_library() 등등 함수를 사용하면서 발생하는 문제들을 해결하기 위해 새로운 페이지를 팠다.

## 목표

- GCC로 정적 라이브러리 컴파일과 링크를 한다.
- GCC로 동적 라이브러리 컴파일과 링크를 한다.

## static library

현재 파일구조는 아래와 같다. 두 개의 sample_lib 파일을 컴파일 하여 라이브러리로 만들고, lib_test_main에서 라이브러리의 인터페이스를 가져다 쓸 것이다.

```c
.
├── CMakeLists.txt
├── lib_test_main.cpp
├── sample_lib1.cpp
└── sample_lib2.cpp
```

```c
// sample_lib1.cpp
int add(const int &a, const int &b)
{
  return a + b;
}

// sample_lib2.cpp
int subtract(const int &a, const int &b)
{
  return a - b;
}

// lib_test_main.cpp
#include <iostream>
int add(const int &a, const int &b);
int main(void)
{
  std::cout << add(1, 2) << "\n";
  return 0;
}
```

먼저 두 lib 파일을 컴파일한다. 정확히 말하자면 전처리와 컴파일 과정까지만 수행하여 목적파일인 .o 파일을 만드는 것을 목표로 한다.

```
➜  lib git:(master) ✗ g++ -c sample_lib1.cpp -std=gnu++17 -o sample_lib1.cpp.o
➜  lib git:(master) ✗ g++ -c sample_lib2.cpp -std=gnu++17 -o sample_lib2.cpp.o
➜  lib git:(master) ✗ tree
.
├── CMakeLists.txt
├── lib_test_main.cpp
├── sample_lib1.cpp
├── sample_lib1.cpp.o
├── sample_lib2.cpp
└── sample_lib2.cpp.o

0 directories, 6 files
```

이제 단순히 두 목적파일을 아카이브 한다.

```
➜  lib git:(master) ✗ ar -rc MyLib.a *.o
➜  lib git:(master) ✗ tree
.
├── CMakeLists.txt
├── MyLib.a
├── lib_test_main.cpp
├── sample_lib1.cpp
├── sample_lib1.cpp.o
├── sample_lib2.cpp
└── sample_lib2.cpp.o

0 directories, 7 files
```

사실 이렇게 단순한 정도의 라이브러리는 굳이 아카이브 하지 않고 한 번에 GCC 인자로 넣어버려도 상관이 없지만 정적 라이브러리 파일 하나만 들고다니는 것에 의미가 있기 때문에 ar 커맨드로 아카이브 한 것이다.

이제 메인 프로그램을 컴파일하자. 정상적으로 잘 작동한다.

```
➜  lib git:(master) ✗ g++ -c lib_test_main.cpp -std=gnu++17 -o lib_test_main.cpp.o
➜  lib git:(master) ✗ g++ -o TestLib.out lib_test_main.cpp.o MyLib.a 
➜  lib git:(master) ✗ ./TestLib.out 
3
➜  lib git:(master) ✗
```

## CMake with static library

CMake는 Make와는 다르게 중간에 발생하는 모든 부산물 (*.o)들을 추상화하였기 때문에 재료들과 만들고 싶은 라이브러리 이름만 있으면 알아서 만들어준다.

```makefile
cmake_minimum_required(VERSION 3.16)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BUILD_TYPE DEBUG)

# 여기에는 라이브러리를 만드는 코드만 생성할 것임.
project("MyLib")
set(CMAKE_VERBOSE_MAKEFILE TRUE) # project() 명령은 이 변수를 항상 FALSE로 초기화하기에...
set(PROJECT_VERSION_MAJOR 1)
set(PROJECT_VERSION_MINOR 0)
set(
  LIB_NAME
  "${CMAKE_PROJECT_NAME}.${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}"
)

set(
  LIB_SOURCES
  sample_lib1.cpp
  sample_lib2.cpp
)

# 라이브러리 만들기 https://matgomes.com/add-library-cmake-create-libraries/
add_library(${LIB_NAME} STATIC ${LIB_SOURCES})
```

cmake를 수행하기 위해 build 디렉토리를 하나 만들고 거기 안에서 cmake 명령어를 친다. 

```
**➜  lib git:(master) ✗ mkdir build
➜  lib git:(master) ✗ cd build
➜  build git:(master) ✗ cmake ..**
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build
```

또는 그냥 build 디렉토리를 만들지 않고 cmake 파라메터로 빌드할 디렉토리를 명시해도 되긴 한다.

```
**➜  lib git:(master) ✗ cmake -S . -B build**
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build
**➜  lib git:(master) ✗ tree**
.
├── CMakeLists.txt
├── build
│   ├── CMakeCache.txt
│   ├── CMakeFiles
│   │   ├── 3.16.3
│   │   │   ├── CMakeCCompiler.cmake
│   │   │   ├── CMakeCXXCompiler.cmake
│   │   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   │   ├── CMakeSystem.cmake
│   │   │   ├── CompilerIdC
│   │   │   │   ├── CMakeCCompilerId.c
│   │   │   │   ├── a.out
│   │   │   │   └── tmp
│   │   │   └── CompilerIdCXX
│   │   │       ├── CMakeCXXCompilerId.cpp
│   │   │       ├── a.out
│   │   │       └── tmp
│   │   ├── CMakeDirectoryInformation.cmake
│   │   ├── CMakeOutput.log
│   │   ├── CMakeTmp
│   │   ├── Makefile.cmake
│   │   ├── Makefile2
│   │   ├── MyLib.1.0.dir
│   │   │   ├── DependInfo.cmake
│   │   │   ├── build.make
│   │   │   ├── cmake_clean.cmake
│   │   │   ├── cmake_clean_target.cmake
│   │   │   ├── depend.make
│   │   │   ├── flags.make
│   │   │   ├── link.txt
│   │   │   └── progress.make
│   │   ├── TargetDirectories.txt
│   │   ├── cmake.check_cache
│   │   └── progress.marks
│   ├── Makefile
│   └── cmake_install.cmake
├── lib_test_main.cpp
├── sample_lib1.cpp
└── sample_lib2.cpp

9 directories, 31 files
```

이제 우리 프로젝트를 위한 메이크파일을 생성해 주었으니 make 명령어만 치면 모든 빌드가 끝난다. 일부로 `CMAKE_VERBOSE_MAKEFILE`을 TRUE로 설정해 주었기 때문에 엄청 많은 로그가 생겨났지만 이것을 분석하면 CMake가 어떻게 작동하는지 볼 수 있다.

```
**➜  build git:(master) ✗ make**
/usr/bin/cmake -S/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib -B/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build/CMakeFiles /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
make -f CMakeFiles/MyLib.1.0.dir/build.make CMakeFiles/MyLib.1.0.dir/depend
make[2]: Entering directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
cd /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build/CMakeFiles/MyLib.1.0.dir/DependInfo.cmake --color=
Scanning dependencies of target MyLib.1.0
make[2]: Leaving directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
make -f CMakeFiles/MyLib.1.0.dir/build.make CMakeFiles/MyLib.1.0.dir/build
make[2]: Entering directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
[ 33%] Building CXX object CMakeFiles/MyLib.1.0.dir/sample_lib1.cpp.o
**/usr/bin/c++    -g   -std=gnu++17 -o CMakeFiles/MyLib.1.0.dir/sample_lib1.cpp.o -c /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/sample_lib1.cpp**
[ 66%] Building CXX object CMakeFiles/MyLib.1.0.dir/sample_lib2.cpp.o
**/usr/bin/c++    -g   -std=gnu++17 -o CMakeFiles/MyLib.1.0.dir/sample_lib2.cpp.o -c /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/sample_lib2.cpp**
[100%] Linking CXX static library libMyLib.1.0.a
/usr/bin/cmake -P CMakeFiles/MyLib.1.0.dir/cmake_clean_target.cmake
/usr/bin/cmake -E cmake_link_script CMakeFiles/MyLib.1.0.dir/link.txt --verbose=1
**/usr/bin/ar qc libMyLib.1.0.a  CMakeFiles/MyLib.1.0.dir/sample_lib1.cpp.o CMakeFiles/MyLib.1.0.dir/sample_lib2.cpp.o
/usr/bin/ranlib libMyLib.1.0.a**
make[2]: Leaving directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
[100%] Built target MyLib.1.0
make[1]: Leaving directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
/usr/bin/cmake -E cmake_progress_start /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build/CMakeFiles 0
```

요약해보면…

```
c++ -g -std=gnu++17 -o CMakeFiles/MyLib.1.0.dir/sample_lib1.cpp.o -c sample_lib1.cpp

c++ -g -std=gnu++17 -o CMakeFiles/MyLib.1.0.dir/sample_lib2.cpp.o -c sample_lib2.cpp

ar qc libMyLib.1.0.a  CMakeFiles/MyLib.1.0.dir/sample_lib1.cpp.o CMakeFiles/MyLib.1.0.dir/sample_lib2.cpp.o

/usr/bin/ranlib libMyLib.1.0.a
```

이정도로 요약해 볼 수 있겠다. 여기서 `c++` 명령어는 `g++` 명령어와 동일하다 (우분투20.04기준) 손수 빌드한 것과 비교해본다면 `ar -rc` 와 `ar qc` 의 차이인 듯 하다. 

- `r`
    
    Insert the files member... into archive (with replacement). This operation differs from q in that any previously existing members are deleted if their names match those  
    being added.
    
    파일들을 아카이브한다. q와의 다른 점은 이전에 만들어진 모든 멤버들과 이름이 겹치면 삭제된다는 것이다. 즉, rc나 qc나 똑같이 아카이브 하는 명령어이다.
    
- `ranlib`
    
    아카이브에 대한 인덱스를 만드는 명령어. 인덱싱 속도를 높일 수 있다고 한다. 없어도 무관한 것 같다.
    

결론적으로 파일이 엄청나게 많이 생겨나긴 했지만 우리는 build/{타겟이름}만 보면 된다. 우리는 `libMyLib.1.0.a` 파일이 최종 타겟이다. 만약 이 아카이브를 메인함수가 달린 프로그램에 링크시키려면 위에서 한 것과 똑같이 하면 된다.

```
➜  lib git:(master) ✗ g++ lib_test_main.cpp build/libMyLib.1.0.a 
➜  lib git:(master) ✗ ./a.out
3
```

# shared library

이번엔 동적 라이브러리를 만들어 빌드과정에 어떻게 포함되는지 알아볼 것이다. 먼저 손수 작업하는 방법을 먼저 해보자. [https://medium.com/meatandmachines/shared-dynamic-libraries-in-the-c-programming-language-8c2c03311756](https://medium.com/meatandmachines/shared-dynamic-libraries-in-the-c-programming-language-8c2c03311756) 를 참고했다.

```
➜  lib git:(master) ✗ g++ -g -fPIC -Wall -Werror -Wextra -pedantic sample_lib*.cpp -shared -o libMyLib.so
➜  lib git:(master) ✗ ls
CMakeLists.txt  libMyLib.so  lib_test_main.cpp  sample_lib1.cpp  sample_lib2.cpp
➜  lib git:(master) ✗ g++ -g -Wall lib_test_main.cpp libMyLib.so
➜  lib git:(master) ✗ l
total 84
drwxr-xr-x 2 chltm chltm  4096 Jul 18 18:13 ./
drwxr-xr-x 6 chltm chltm  4096 Jul 18 17:15 ../
-rw-r--r-- 1 chltm chltm   631 Jul 18 17:14 CMakeLists.txt
-rwxr-xr-x 1 chltm chltm 39584 Jul 18 18:13 a.out*
-rwxr-xr-x 1 chltm chltm 18800 Jul 18 18:07 libMyLib.so*
-rw-r--r-- 1 chltm chltm   142 Jul 18 16:05 lib_test_main.cpp
-rw-r--r-- 1 chltm chltm    75 Jul 18 16:06 sample_lib1.cpp
-rw-r--r-- 1 chltm chltm    60 Jul 18 17:08 sample_lib2.cpp
➜  lib git:(master) ✗ ./a.out
./a.out: error while loading shared libraries: libMyLib.so: cannot open shared object file: No such file or directory
```

[https://www.lesstif.com/lpt/shared-library-dependencies-linux-ldd-95880421.html](https://www.lesstif.com/lpt/shared-library-dependencies-linux-ldd-95880421.html)

ldd 명령어를 사용하면 필요한 공유 라이브러리의 의존성을 출력해 준다고 한다. 현재 a.out 의 의존성을 확인해보자.

```
**➜  lib git:(master) ✗ ldd ./a.out**
        linux-vdso.so.1 (0x00007fffb64d5000)
        libMyLib.so => not found
        libstdc++.so.6 => /lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007f8a7b103000)
        libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007f8a7b0e8000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f8a7aef6000)
        libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f8a7ada7000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f8a7b2f7000)
```

파일 위치를 확인할 수 없다고? 바로 옆에 두고도 못 찾는다고?

[https://stackoverflow.com/a/8538605/6428901](https://stackoverflow.com/a/8538605/6428901) 에 따르면, 스탠다드 시스템 라이브러리 (나의 경우는 `/lib/x86_64-linux-gnu/` )가 아닌 경우는 환경변수에 해당 공유 라이브러리의 위치를 추가하는 것으로 문제를 해결할 수 있다고 한다.

```
➜  lib git:(master) ✗ export LD_LIBRARY_PATH=.
➜  lib git:(master) ✗ ./a.out
3
```

와 된다! 한가지 걱정은, 이 세상에 얼마나 많은 동적 라이브러리들이 부유하고 또 서로 얽히고 섥히는데, 프로그램 안에 동적 라이브러리를 직접 위치할 수 없다면 매번 이렇게 환경변수 설정을 해주고 의존성 관리를 해주어야 하는 걸까? 

## CMake with shared library

[CMake with static library](https://www.notion.so/CMake-with-static-library-94bd0571d7654226b8ead5014f2effb1?pvs=21) 에 적었던 코드의 마지막 줄에 `STATIC` 키워드를 `SHARED` 키워드로 바꾸기만 하면 된다. 

```makefile
# static library
add_library(${LIB_NAME} STATIC ${LIB_SOURCES})
# shared library
add_library(${LIB_NAME} SHARED ${LIB_SOURCES})
```

```
**➜  lib git:(master) ✗ cmake -S . -B build**
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build
**➜  lib git:(master) ✗ make --directory=build**
make: Entering directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
/usr/bin/cmake -S/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib -B/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build --check-build-system CMakeFiles/Makefile.cmake 0
/usr/bin/cmake -E cmake_progress_start /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build/CMakeFiles /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build/CMakeFiles/progress.marks
make -f CMakeFiles/Makefile2 all
make[1]: Entering directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
make -f CMakeFiles/MyLib.1.0.dir/build.make CMakeFiles/MyLib.1.0.dir/depend
make[2]: Entering directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
cd /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build && /usr/bin/cmake -E cmake_depends "Unix Makefiles" /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build/CMakeFiles/MyLib.1.0.dir/DependInfo.cmake --color=
Scanning dependencies of target MyLib.1.0
make[2]: Leaving directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
make -f CMakeFiles/MyLib.1.0.dir/build.make CMakeFiles/MyLib.1.0.dir/build
make[2]: Entering directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
[ 33%] Building CXX object CMakeFiles/MyLib.1.0.dir/sample_lib1.cpp.o
**/usr/bin/c++  -DMyLib_1_0_EXPORTS  -g -fPIC   -std=gnu++17 -o CMakeFiles/MyLib.1.0.dir/sample_lib1.cpp.o -c /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/sample_lib1.cpp**
[ 66%] Building CXX object CMakeFiles/MyLib.1.0.dir/sample_lib2.cpp.o
**/usr/bin/c++  -DMyLib_1_0_EXPORTS  -g -fPIC   -std=gnu++17 -o CMakeFiles/MyLib.1.0.dir/sample_lib2.cpp.o -c /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/sample_lib2.cpp**
[100%] Linking CXX shared library libMyLib.1.0.so
/usr/bin/cmake -E cmake_link_script CMakeFiles/MyLib.1.0.dir/link.txt --verbose=1
**/usr/bin/c++ -fPIC -g  -shared -Wl,-soname,libMyLib.1.0.so -o libMyLib.1.0.so CMakeFiles/MyLib.1.0.dir/sample_lib1.cpp.o CMakeFiles/MyLib.1.0.dir/sample_lib2.cpp.o** 
make[2]: Leaving directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
[100%] Built target MyLib.1.0
make[1]: Leaving directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
/usr/bin/cmake -E cmake_progress_start /home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build/CMakeFiles 0
make: Leaving directory '/home/chltm/workspace/choi-workspace/c++/learning_cmake/lib/build'
```

여기에서 눈여겨 보아야 할 부분만 요악하자면…

```
c++ -DMyLib_1_0_EXPORTS -g -fPIC -std=gnu++17 -o sample_lib1.cpp.o -c sample_lib1.cpp

c++ -DMyLib_1_0_EXPORTS -g -fPIC -std=gnu++17 -o sample_lib2.cpp.o -c sample_lib2.cpp

c++ -fPIC -g -shared -Wl,-soname,libMyLib.1.0.so -o libMyLib.1.0.so sample_lib1.cpp.o sample_lib2.cpp.o 
```

첫번째, 두 번째 줄의 경우 `-D` 옵션을 통해서 Define 구문을 추가한 것 말곤 특별히 다른 점은 없는 것 같다. 문제는 세 번째 줄에서 두 개의 목적파일을 통해 동적 라이브러리 `libMyLib.1.0.so` 를 만드는 데 들어간 파라메터인데, 모르는 것들이 많다. 

- `-Wl,` [https://gcc.gnu.org/onlinedocs/gcc-11.2.0/gcc/Link-Options.html](https://gcc.gnu.org/onlinedocs/gcc-11.2.0/gcc/Link-Options.html)
    
    Pass option as an option to the linker.  If option contains commas, it is split into multiple options at the commas.  You can use this syntax to pass an argument to the option. For example, -Wl,-Map,output.map passes -Map output.map to the linker.  When using the GNU linker, you can also get the same effect with -Wl,-Map=output.map.
    
    comma 뒤에 붙은 모든 명령어들을 `ld` 링커에게 넘기겠다는 의미이다.
    
- `-soname,` [https://www.man7.org/linux/man-pages/man1/ld.1.html](https://www.man7.org/linux/man-pages/man1/ld.1.html)
    
    When creating an ELF shared object, set the internal DT_SONAME field to the specified name. When an executable is linked with a shared object which has a DT_SONAME field, then  
    when the executable is run the dynamic linker will attempt to load the shared object specified by the DT_SONAME field rather than the using the file name given to the linker.
    
    `ld` 명령어의 파라메터로, 동적 라이브러리의 파일이름보다 우선하는 이름을 지정할 수 있다. 없으면 파일 이름을 따라가고 있으면 반드시 soname과 일치하는 동적 라이브러리를 로드.
    
- `-shared` [https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html](https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html)
    
    Produce a shared object which can then be linked with other objects to form an executable.  Not all systems support this option.  For predictable results, you must also specify the same set of options used for compilation (-fpic, -fPIC, or model suboptions) when you specify this linker option.[1](https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html#FOOT1)
    
    동적 라이브러리를 만들어주는 옵션. 모든 시스템이 이를 지원하지는 않기 때문에 사실 더 확실한 `-fPIC` 옵션을 사용하자. 
    
    그러면 CMake는 굳이 필요도 없는 옵션을 왜 추가했을까?
    

동적 라이브러리를 링크하여 메인 프로그램을 빌드해보자.

```
➜  lib git:(master) ✗ g++ lib_test_main.cpp build/libMyLib.1.0.so 
➜  lib git:(master) ✗ ./a.out
./a.out: error while loading shared libraries: libMyLib.1.0.so: cannot open shared object file: No such file or directory
```

아까와 똑같은 에러가 나왔다. 이젠 해결할 수 있는 간단한 문제.

```
➜  lib git:(master) ✗ echo $LD_LIBRARY_PATH 
.
➜  lib git:(master) ✗ export LD_LIBRARY_PATH=.:./build:
➜  lib git:(master) ✗ ./a.out
3
```
