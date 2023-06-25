# lab06-popytka

# Клонируем репозиторий из 4 лабораторной работы, добавляем файлы CPackConfig.cmake, CMakeLists.txt, CI.yml 
```sh
──(kali㉿kali)-[~]
└─$ cd HECCYLLIujTbmy/workspace/projects/
```
```sh                                                                                                    
┌──(kali㉿kali)-[~/HECCYLLIujTbmy/workspace/projects]
└─$ git clone https://github.com/HECCYLLIujTbmy/lab04-pls lab06
Cloning into 'lab06'...
remote: Enumerating objects: 225, done.
remote: Counting objects: 100% (225/225), done.
remote: Compressing objects: 100% (98/98), done.
remote: Total 225 (delta 119), reused 208 (delta 113), pack-reused 0
Receiving objects: 100% (225/225), 90.33 KiB | 1.22 MiB/s, done.
Resolving deltas: 100% (119/119), done.
```
```sh                                                                                                  
kali㉿kali)-[~/HECCYLLIujTbmy/workspace/projects/lab06]
└─$ git init  
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint: 
hint:   git config --global init.defaultBranch <name>
hint: 
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint: 
hint:   git branch -m <name>
Initialized empty Git repository in /home/kali/HECCYLLIujTbmy/workspace/projects/lab06/.git/
```
```sh
┌──(kali㉿kali)-[~/HECCYLLIujTbmy/workspace/projects]
└─$ cd lab06                             
                                                                                                    
┌──(kali㉿kali)-[~/HECCYLLIujTbmy/workspace/projects/lab06]
```

<details><summary>$ nano CPackConfig.cmake</summary>

```sh
include(InstallRequiredSystemLibraries)

set(CPACK_PACKAGE_CONTACT donotdisturb@yandex.ru)
set(CPACK_PACKAGE_VERSION_MAJOR ${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK ${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION ${PRINT_VERSION})

set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static C++ library for printing")

set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)

set(CPACK_RPM_PACKAGE_NAME "solver")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print-solver")
set(CPACK_RPM_PACKAGE_VERSION CPACK_PACKAGE_VERSION)

set(CPACK_DEBIAN_PACKAGE_NAME "solver")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_VERSION CPACK_PACKAGE_VERSION)

include(CPack)
```

</details>

```sh
(kali㉿kali)-[~/HECCYLLIujTbmy/workspace/projects/lab06]
```

<details><summary>$ nano CMakeLists.txt</summary>

```sh
cmake_minimum_required(VERSION 3.4)
project(lab06)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include_directories("formatter_lib")
include_directories("formatter_ex_lib")
include_directories("solver_lib")

add_library(formatter_lib STATIC "formatter_lib/formatter.cpp")
add_library(formatter_ex_lib STATIC "formatter_ex_lib/formatter_ex.cpp")
add_library(solver_lib STATIC "solver_lib/solver.cpp")

add_executable(solver "solver_application/equation.cpp")

target_link_libraries(solver solver_lib formatter_ex_lib formatter_lib)

include(CPackConfig.cmake)
```

</details>

```sh
┌──(kali㉿kali)-[~/HECCYLLIujTbmy/workspace/projects/lab06]
└─$ cd .github/workflows
┌──(kali㉿kali)-[~/…/projects/lab06/.github/workflows]
└─$ nano CI.yml
```
<details><summary>$ nano CI.yml</summary>

```sh
name: CMake

on:
  push:
    branches: [main]
    tags: -"v*0.*"
  pull_request:
    branches: [main]

env:
  BUILD_TYPE: Release

jobs:
  build_Linux:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Configure Solver
        run: cmake -H. -B_build -DCPACK_GENERATOR="TGZ"

      - name: Build Solver
        run: cmake --build _build --target package

      - name: Make Solver Package
        run: cd _build && cpack -G "DEB" &&
             cpack -G "RPM" &&
             mkdir ../artifacts &&
             mv *.tar.gz ../artifacts/ &&
             mv *.deb ../artifacts/ &&
             mv *.rpm ../artifacts/
      - name: Publish
        uses: actions/upload-artifact@v2
        with:
          name: DebRpm
          path: artifacts/
```
</details>

<details><summary>$ git commit -m"f1rst  push"</summary>

  ```sh
[master (root-commit) da15f07] f1rst  push
 286 files changed, 26013 insertions(+)
 create mode 100644 .github/workflows/CI.yml
 create mode 100644 .github/workflows/Linux.yml
 create mode 100644 .github/workflows/Windows.yml
 create mode 100644 CMakeLists.txt
 create mode 100644 CPackConfig.cmake
 create mode 100644 formatter_ex_lib/CMakeLists.txt
 create mode 100644 formatter_ex_lib/_build/CMakeCache.txt
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/3.22.1/CMakeCCompiler.cmake
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/3.22.1/CMakeCXXCompiler.cmake
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/3.22.1/CMakeDetermineCompilerABI_C.bin
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/3.22.1/CMakeDetermineCompilerABI_CXX.bin
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/3.22.1/CMakeSystem.cmake
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/3.22.1/CompilerIdC/CMakeCCompilerId.c
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/3.22.1/CompilerIdC/a.out
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/3.22.1/CompilerIdCXX/CMakeCXXCompilerId.cpp
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/3.22.1/CompilerIdCXX/a.out
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/CMakeOutput.log
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/Makefile.cmake
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/Makefile2
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/TargetDirectories.txt
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/cmake.check_cache
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/DependInfo.cmake
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/build.make
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/cmake_clean.cmake
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/cmake_clean_target.cmake
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/compiler_depend.make
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/compiler_depend.ts
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/depend.make
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/flags.make
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o.d
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/link.txt
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/formatter_ex.dir/progress.make
 create mode 100644 formatter_ex_lib/_build/CMakeFiles/progress.marks
 create mode 100644 formatter_ex_lib/_build/Makefile
 create mode 100644 formatter_ex_lib/_build/cmake_install.cmake
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/DependInfo.cmake
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/build.make
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/cmake_clean.cmake
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/cmake_clean_target.cmake
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/compiler_depend.make
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/compiler_depend.ts
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/depend.make
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/flags.make
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/formatter.cpp.o
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/formatter.cpp.o.d
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/link.txt
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/formatter.dir/progress.make
 create mode 100644 formatter_ex_lib/_build/formatter/CMakeFiles/progress.marks
 create mode 100644 formatter_ex_lib/_build/formatter/Makefile
 create mode 100644 formatter_ex_lib/_build/formatter/cmake_install.cmake
 create mode 100644 formatter_ex_lib/_build/formatter/libformatter.a
 create mode 100644 formatter_ex_lib/_build/libformatter_ex.a
 create mode 100644 formatter_ex_lib/formatter_ex.cpp
 create mode 100644 formatter_ex_lib/formatter_ex.h
 create mode 100644 formatter_lib/CMakeLists.txt
 create mode 100644 formatter_lib/_build/CMakeCache.txt
 create mode 100644 formatter_lib/_build/CMakeFiles/3.22.1/CMakeCCompiler.cmake
 create mode 100644 formatter_lib/_build/CMakeFiles/3.22.1/CMakeCXXCompiler.cmake
 create mode 100644 formatter_lib/_build/CMakeFiles/3.22.1/CMakeDetermineCompilerABI_C.bin
 create mode 100644 formatter_lib/_build/CMakeFiles/3.22.1/CMakeDetermineCompilerABI_CXX.bin
 create mode 100644 formatter_lib/_build/CMakeFiles/3.22.1/CMakeSystem.cmake
 create mode 100644 formatter_lib/_build/CMakeFiles/3.22.1/CompilerIdC/CMakeCCompilerId.c
 create mode 100644 formatter_lib/_build/CMakeFiles/3.22.1/CompilerIdC/a.out
 create mode 100644 formatter_lib/_build/CMakeFiles/3.22.1/CompilerIdCXX/CMakeCXXCompilerId.cpp
 create mode 100644 formatter_lib/_build/CMakeFiles/3.22.1/CompilerIdCXX/a.out
 create mode 100644 formatter_lib/_build/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 formatter_lib/_build/CMakeFiles/CMakeOutput.log
 create mode 100644 formatter_lib/_build/CMakeFiles/Makefile.cmake
 create mode 100644 formatter_lib/_build/CMakeFiles/Makefile2
 create mode 100644 formatter_lib/_build/CMakeFiles/TargetDirectories.txt
 create mode 100644 formatter_lib/_build/CMakeFiles/cmake.check_cache
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/DependInfo.cmake
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/build.make
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/cmake_clean.cmake
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/cmake_clean_target.cmake
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/compiler_depend.make
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/compiler_depend.ts
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/depend.make
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/flags.make
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/formatter.cpp.o
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/formatter.cpp.o.d
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/link.txt
 create mode 100644 formatter_lib/_build/CMakeFiles/formatter.dir/progress.make
 create mode 100644 formatter_lib/_build/CMakeFiles/progress.marks
 create mode 100644 formatter_lib/_build/Makefile
 create mode 100644 formatter_lib/_build/cmake_install.cmake
 create mode 100644 formatter_lib/_build/libformatter.a
 create mode 100644 formatter_lib/formatter.cpp
 create mode 100644 formatter_lib/formatter.h
 create mode 100644 hello_world_application/CMakeLists.txt
 create mode 100644 hello_world_application/_build/CMakeCache.txt
 create mode 100644 hello_world_application/_build/CMakeFiles/3.22.1/CMakeCCompiler.cmake
 create mode 100644 hello_world_application/_build/CMakeFiles/3.22.1/CMakeCXXCompiler.cmake
 create mode 100644 hello_world_application/_build/CMakeFiles/3.22.1/CMakeDetermineCompilerABI_C.bin
 create mode 100644 hello_world_application/_build/CMakeFiles/3.22.1/CMakeDetermineCompilerABI_CXX.bin
 create mode 100644 hello_world_application/_build/CMakeFiles/3.22.1/CMakeSystem.cmake
 create mode 100644 hello_world_application/_build/CMakeFiles/3.22.1/CompilerIdC/CMakeCCompilerId.c
 create mode 100644 hello_world_application/_build/CMakeFiles/3.22.1/CompilerIdC/a.out
 create mode 100644 hello_world_application/_build/CMakeFiles/3.22.1/CompilerIdCXX/CMakeCXXCompilerId.cpp
 create mode 100644 hello_world_application/_build/CMakeFiles/3.22.1/CompilerIdCXX/a.out
 create mode 100644 hello_world_application/_build/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 hello_world_application/_build/CMakeFiles/CMakeOutput.log
 create mode 100644 hello_world_application/_build/CMakeFiles/Makefile.cmake
 create mode 100644 hello_world_application/_build/CMakeFiles/Makefile2
 create mode 100644 hello_world_application/_build/CMakeFiles/TargetDirectories.txt
 create mode 100644 hello_world_application/_build/CMakeFiles/cmake.check_cache
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/DependInfo.cmake
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/build.make
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/cmake_clean.cmake
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/compiler_depend.internal
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/compiler_depend.make
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/compiler_depend.ts
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/depend.make
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/flags.make
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/hello_world.cpp.o
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/hello_world.cpp.o.d
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/link.txt
 create mode 100644 hello_world_application/_build/CMakeFiles/example.dir/progress.make
 create mode 100644 hello_world_application/_build/CMakeFiles/progress.marks
 create mode 100644 hello_world_application/_build/Makefile
 create mode 100644 hello_world_application/_build/cmake_install.cmake
 create mode 100644 hello_world_application/_build/example
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/DependInfo.cmake
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/build.make
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/cmake_clean.cmake
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/cmake_clean_target.cmake
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/compiler_depend.internal
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/compiler_depend.make
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/compiler_depend.ts
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/depend.make
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/flags.make
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o.d
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/link.txt
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/progress.make
 create mode 100644 hello_world_application/_build/formatter_ex/CMakeFiles/progress.marks
 create mode 100644 hello_world_application/_build/formatter_ex/Makefile
 create mode 100644 hello_world_application/_build/formatter_ex/cmake_install.cmake
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/DependInfo.cmake
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/build.make
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/cmake_clean.cmake
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/cmake_clean_target.cmake
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/compiler_depend.internal
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/compiler_depend.make
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/compiler_depend.ts
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/depend.make
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/flags.make
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/formatter.cpp.o
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/formatter.cpp.o.d
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/link.txt
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/progress.make
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/CMakeFiles/progress.marks
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/Makefile
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/cmake_install.cmake
 create mode 100644 hello_world_application/_build/formatter_ex/formatter/libformatter.a
 create mode 100644 hello_world_application/_build/formatter_ex/libformatter_ex.a
 create mode 100644 hello_world_application/hello_world.cpp
 create mode 100644 solver_application/CMakeLists.txt
 create mode 100644 solver_application/_build/CMakeCache.txt
 create mode 100644 solver_application/_build/CMakeFiles/3.22.1/CMakeCCompiler.cmake
 create mode 100644 solver_application/_build/CMakeFiles/3.22.1/CMakeCXXCompiler.cmake
 create mode 100644 solver_application/_build/CMakeFiles/3.22.1/CMakeDetermineCompilerABI_C.bin
 create mode 100644 solver_application/_build/CMakeFiles/3.22.1/CMakeDetermineCompilerABI_CXX.bin
 create mode 100644 solver_application/_build/CMakeFiles/3.22.1/CMakeSystem.cmake
 create mode 100644 solver_application/_build/CMakeFiles/3.22.1/CompilerIdC/CMakeCCompilerId.c
 create mode 100644 solver_application/_build/CMakeFiles/3.22.1/CompilerIdC/a.out
 create mode 100644 solver_application/_build/CMakeFiles/3.22.1/CompilerIdCXX/CMakeCXXCompilerId.cpp
 create mode 100644 solver_application/_build/CMakeFiles/3.22.1/CompilerIdCXX/a.out
 create mode 100644 solver_application/_build/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 solver_application/_build/CMakeFiles/CMakeOutput.log
 create mode 100644 solver_application/_build/CMakeFiles/Makefile.cmake
 create mode 100644 solver_application/_build/CMakeFiles/Makefile2
 create mode 100644 solver_application/_build/CMakeFiles/TargetDirectories.txt
 create mode 100644 solver_application/_build/CMakeFiles/cmake.check_cache
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/DependInfo.cmake
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/build.make
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/cmake_clean.cmake
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/compiler_depend.internal
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/compiler_depend.make
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/compiler_depend.ts
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/depend.make
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/equation.cpp.o
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/equation.cpp.o.d
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/flags.make
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/link.txt
 create mode 100644 solver_application/_build/CMakeFiles/example.dir/progress.make
 create mode 100644 solver_application/_build/CMakeFiles/progress.marks
 create mode 100644 solver_application/_build/Makefile
 create mode 100644 solver_application/_build/cmake_install.cmake
 create mode 100644 solver_application/_build/example
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/DependInfo.cmake
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/build.make
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/cmake_clean.cmake
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/cmake_clean_target.cmake
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/compiler_depend.internal
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/compiler_depend.make
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/compiler_depend.ts
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/depend.make
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/flags.make
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o.d
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/link.txt
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/formatter_ex.dir/progress.make
 create mode 100644 solver_application/_build/formatter_ex/CMakeFiles/progress.marks
 create mode 100644 solver_application/_build/formatter_ex/Makefile
 create mode 100644 solver_application/_build/formatter_ex/cmake_install.cmake
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/DependInfo.cmake
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/build.make
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/cmake_clean.cmake
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/cmake_clean_target.cmake
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/compiler_depend.internal
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/compiler_depend.make
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/compiler_depend.ts
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/depend.make
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/flags.make
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/formatter.cpp.o
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/formatter.cpp.o.d
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/link.txt
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/formatter.dir/progress.make
 create mode 100644 solver_application/_build/formatter_ex/formatter/CMakeFiles/progress.marks
 create mode 100644 solver_application/_build/formatter_ex/formatter/Makefile
 create mode 100644 solver_application/_build/formatter_ex/formatter/cmake_install.cmake
 create mode 100644 solver_application/_build/formatter_ex/formatter/libformatter.a
 create mode 100644 solver_application/_build/formatter_ex/libformatter_ex.a
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/progress.marks
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/DependInfo.cmake
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/build.make
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/cmake_clean.cmake
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/cmake_clean_target.cmake
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/compiler_depend.internal
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/compiler_depend.make
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/compiler_depend.ts
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/depend.make
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/flags.make
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/link.txt
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/progress.make
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o
 create mode 100644 solver_application/_build/solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o.d
 create mode 100644 solver_application/_build/solver_lib/Makefile
 create mode 100644 solver_application/_build/solver_lib/cmake_install.cmake
 create mode 100644 solver_application/_build/solver_lib/libsolver_lib.a
 create mode 100644 solver_application/equation.cpp
 create mode 100644 solver_lib/CMakeLists.txt
 create mode 100644 solver_lib/_build/CMakeCache.txt
 create mode 100644 solver_lib/_build/CMakeFiles/3.22.1/CMakeCCompiler.cmake
 create mode 100644 solver_lib/_build/CMakeFiles/3.22.1/CMakeCXXCompiler.cmake
 create mode 100644 solver_lib/_build/CMakeFiles/3.22.1/CMakeDetermineCompilerABI_C.bin
 create mode 100644 solver_lib/_build/CMakeFiles/3.22.1/CMakeDetermineCompilerABI_CXX.bin
 create mode 100644 solver_lib/_build/CMakeFiles/3.22.1/CMakeSystem.cmake
 create mode 100644 solver_lib/_build/CMakeFiles/3.22.1/CompilerIdC/CMakeCCompilerId.c
 create mode 100644 solver_lib/_build/CMakeFiles/3.22.1/CompilerIdC/a.out
 create mode 100644 solver_lib/_build/CMakeFiles/3.22.1/CompilerIdCXX/CMakeCXXCompilerId.cpp
 create mode 100644 solver_lib/_build/CMakeFiles/3.22.1/CompilerIdCXX/a.out
 create mode 100644 solver_lib/_build/CMakeFiles/CMakeDirectoryInformation.cmake
 create mode 100644 solver_lib/_build/CMakeFiles/CMakeOutput.log
 create mode 100644 solver_lib/_build/CMakeFiles/Makefile.cmake
 create mode 100644 solver_lib/_build/CMakeFiles/Makefile2
 create mode 100644 solver_lib/_build/CMakeFiles/TargetDirectories.txt
 create mode 100644 solver_lib/_build/CMakeFiles/cmake.check_cache
 create mode 100644 solver_lib/_build/CMakeFiles/progress.marks
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/DependInfo.cmake
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/build.make
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/cmake_clean.cmake
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/cmake_clean_target.cmake
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/compiler_depend.internal
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/compiler_depend.make
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/compiler_depend.ts
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/depend.make
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/flags.make
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/link.txt
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/progress.make
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/solver.cpp.o
 create mode 100644 solver_lib/_build/CMakeFiles/solver_lib.dir/solver.cpp.o.d
 create mode 100644 solver_lib/_build/Makefile
 create mode 100644 solver_lib/_build/cmake_install.cmake
 create mode 100644 solver_lib/_build/libsolver_lib.a
 create mode 100644 solver_lib/solver.cpp
 create mode 100644 solver_lib/solver.h
 ```

</details>

```sh
┌──(kali㉿kali)-[~/HECCYLLIujTbmy/workspace/projects/lab06]
└─$ git push origin master    
Username for 'https://github.com': HECCYLLIujTbmy
Password for 'https://HECCYLLIujTbmy@github.com':                                                                                                                                                                                           
Enumerating objects: 216, done.
Counting objects: 100% (216/216), done.
Delta compression using up to 2 threads
Compressing objects: 100% (204/204), done.
Writing objects: 100% (216/216), 84.15 KiB | 3.66 MiB/s, done.
Total 216 (delta 114), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (114/114), done.
To https://github.com/HECCYLLIujTbmy/lab06-popytka
 * [new branch]      master -> master
```
