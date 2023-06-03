## Лабораторная работа 6

## Ход выполнения

- Клонировал репозиторий от ЛР (лабораторной работы)  №4

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

- Переключился на репозиторий для ЛР №6
```console
$ git remote remove origin
$ git remote add origin https://github.com/3Artem99/lab06
```

- Создал файл CPackConfig.cmake

```console
$ touch CPackConfig.cmake
```
- Удалил папку "hello_world_application" (она не нужна по заданию (Homework))

```console 
$ rm -rf hello_world_application/
```
[Ссылка на задание](https://github.com/tp-labs/lab06)

- Запушил недоделанную работу на гитхаб в ветку main

```console
$ git add .
$ git commit -m "added CPackConfig.cmake"
```

```console
[main c3ab3e4] added CPackConfig.cmake
 3 files changed, 24 deletions(-)
 create mode 100644 CPackConfig.cmake
 delete mode 100644 hello_world_application/CMakeLists.txt
 delete mode 100644 hello_world_application/hello_world.cpp
```

```console
$ git add .
$ git commit -m "edit readme.h"
```

```console
[main c9f30cf] edit readme.h
 2 files changed, 33 insertions(+), 428 deletions(-)
 create mode 100644 .README.h.swp
 rewrite README.md (99%)
```

```console
$ git push origin main
```

```console
Username for 'https://github.com': 3Artem99
Password for 'https://3Artem99@github.com': 
Перечисление объектов: 130, готово.
Подсчет объектов: 100% (130/130), готово.
При сжатии изменений используется до 4 потоков
Сжатие объектов: 100% (66/66), готово.
Запись объектов: 100% (130/130), 1.03 МиБ | 29.26 МиБ/с, готово.
Всего 130 (изменений 58), повторно использовано 123 (изменений 56), повторно использовано пакетов 0
remote: Resolving deltas: 100% (58/58), done.
To https://github.com/3Artem99/lab06
 * [new branch]      main -> main
```

- Изменил его (CPackConfig.cmake) Уже в гитхабе

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

- Перестроил файл CI.yml

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

- Совсем забыл про файл CMakeLists.txt

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
