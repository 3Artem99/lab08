## Лабораторная работа 6

## Ход выполнения

Клонировал репозиторий от ЛР (лабораторной работы)  №4

```console
$ git clone https://github.com/3Artem99/lab040 lab06
```

```console
Клонирование в «lab06»…
remote: Enumerating objects: 124, done.
remote: Counting objects: 100% (124/124), done.
remote: Compressing objects: 100% (61/61), done.
remote: Total 124 (delta 56), reused 118 (delta 55), pack-reused 0
Получение объектов: 100% (124/124), 1.03 МиБ | 4.11 МиБ/с, готово.
Определение изменений: 100% (56/56), готово.
```

Переключился на репозиторий для ЛР №6
```console
$ git remote remove origin
$ git remote add origin https://github.com/3Artem99/lab06
```

Создал файл CPackConfig.cmake

```console
$ touch CPackConfig.cmake
```
Удалил папку "hello_world_application" (она не нужна по заданию (Homework))

```console 
$ rm -rf hello_world_application/
```
[![Markdown clickable image](/images/markdown.png "Click me!")]([https://google.com](https://github.com/tp-labs/lab06))

Запушил недоделанную работу на гитхаб в ветку main

```console
$ git push origin main
```
Изменил его (CPackConfig.cmake)
Уже в гитхабе

```console
include(InstallRequiredSystemLibraries)
set(CPACK_PACKAGE_CONTACT donotwriteme@bmstuisbetterthanhse.com)
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})

set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
set(CPACK_RPM_PACKAGE_NAME "solver_lab")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "solver")
set(CPACK_RPM_PACKAGE_VERSION CPACK_PACKAGE_VERSION)
set(CPACK_DEBIAN_PACKAGE_NAME "libsolvert-lab")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_VERSION CPACK_PACKAGE_VERSION)
include(CPack)
```

Перестроил файл CI.yml

```console
include(InstallRequiredSystemLibraries)
set(CPACK_PACKAGE_CONTACT donotwriteme@bmstuisbetterthanhse.com)
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})

set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
set(CPACK_RPM_PACKAGE_NAME "solver_lab")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "solver")
set(CPACK_RPM_PACKAGE_VERSION CPACK_PACKAGE_VERSION)
set(CPACK_DEBIAN_PACKAGE_NAME "libsolvert-lab")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_VERSION CPACK_PACKAGE_VERSION)
include(CPack)
```

Совсем забыл про файл CMakeLists.txt

```console
cmake_minimum_required(VERSION 3.4)
project(lab03)

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
