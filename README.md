## ТиМП для невдуплёнышей [1]

Научимся собирать простенькие приложения с помощью утилиты **cmake**. После выполнения работы вам не понадобится IDE, чтобы компилить код, хотя компилить всё в консоли я всё равно не рекомендую).

## Теория

**CMake файл** — набор инструкций по сборке проекта. Должен называться `CMakeLists.txt`.

**Таргет** — что-то, что можно собрать, то есть библиотека или исполняемый файл.

**Собрать** — значит из исходного кода получить файл библиотеки формата `.a` или исполняемый файл.

Ещё у CMake есть свои переменные, как и у консоли, но нам пока нужна только одна:

**CMAKE_CURRENT_SOURCE_DIR** — путь к директории, где лежит `CMakeLists.txt`

**Процесс сборки** делится на два этапа:

- Подготовка файлов сборки
- Непосредственно сборка

**Команды сборки:**

**cmake -B build** — готовит файлы сборки и собирает их в папочку `build`.

**cmake --build build** — на основе файлов из папки `build` собирает библиотеку/исполняемый файл.

**Как писать эти ваши CMake файлы?**

Очень просто. Рассмотрим структуру CMake файла для сборки некоторого исполняемого файла, использующего некую библиотеку:

```cmake
cmake_minimum_required(VERSION 3.4)
```

Эта строка задаёт минимальную версию утилиты **cmake**, необходимую для сборки.

```cmake
project(app)
```

Задаём название проекта

```cmake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
```

Задаём требуемый стандарт C++.

```cmake
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../library library_dir)
```

Включаем в проект библиотеку, лежащую в директории `../library`. Предполагается, что в `../library` есть CMake файл для сборки этой библиотеки.

Здесь:

**${CMAKE_CURRENT_SOURCE_DIR}/../library** — путь к директории с CMake файлом библиотеки

**library_dir** — под каким именем директория файлов сборки библиотеки будет записана в файлах сборки нашего исполняемого файла.

```cmake
add_executable(app ${CMAKE_CURRENT_SOURCE_DIR}/app.cpp)
```

Создаём таргет `app` исполняемого файла, исходный код которого лежит в файле `app.cpp`.

```cmake
target_include_directories(app PUBLIC
${CMAKE_CURRENT_SOURCE_DIR}/../library
)
```

Открываем таргету доступ к директории `../library`. Там лежит какой-нибудь library.h — заголовочный файл библиотеки.

```cmake
target_link_libraries(app library)
```

Подключаем библиотеку `library` к таргету `app`.


## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```sh
$ open https://cmake.org/
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [ ] 2. Ознакомиться со ссылками учебного материала
- [ ] 3. Выполнить инструкцию учебного материала
- [ ] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=<имя_пользователя>
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
$ cd projects/lab03
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```

```sh
$ g++ -std=c++11 -I./include -c sources/print.cpp
$ ls print.o
$ nm print.o | grep print
$ ar rvs print.a print.o
$ file print.a
$ g++ -std=c++11 -I./include -c examples/example1.cpp
$ ls example1.o
$ g++ example1.o print.a -o example1
$ ./example1 && echo
```

```sh
$ g++ -std=c++11 -I./include -c examples/example2.cpp
$ nm example2.o
$ g++ example2.o print.a -o example2
$ ./example2
$ cat log.txt && echo
```

```sh
$ rm -rf example1.o example2.o print.o
$ rm -rf print.a
$ rm -rf example1 example2
$ rm -rf log.txt
```

```sh
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```

```sh
$ cmake -H. -B_build
$ cmake --build _build
```

```sh
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```

```sh
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

```sh
$ cmake --build _build
$ cmake --build _build --target print
$ cmake --build _build --target example1
$ cmake --build _build --target example2
```

```sh
$ ls -la _build/libprint.a
$ _build/example1 && echo
hello
$ _build/example2
$ cat log.txt && echo
hello
$ rm -rf log.txt
```

```sh
$ git clone https://github.com/tp-labs/lab03 tmp
$ mv -f tmp/CMakeLists.txt .
$ rm -rf tmp
```

```sh
$ cat CMakeLists.txt
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
$ cmake --build _build --target install
$ tree _install
```

```sh
$ git add CMakeLists.txt
$ git commit -m"added CMakeLists.txt"
$ git push origin master
```

## Report

```sh
$ popd
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

> Всё, что от нас требуется — это написать CMakeLists.txt.
> 
> Создаём **в директории** `formatter_lib` файл с названием `CMakeLists.txt`
>
> Приведу готовый файл:
> ```cmake
> cmake_minimum_required(VERSION 3.4)
> project(formatter_lib)
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> # Выше - стандартная фигня, она везде будет одинаковой, кроме имени проекта.
> add_library(formatter_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
> # Добавляем таргет-статичную библиотеку formatter_lib из файла с кодом formatter.cpp
> ```
> Проверим, что проект собирается. Для этого **в директории** `formatter_lib` введём команды:
> ```sh
> $ cmake -B build
> $ cmake --build build
> ```
> Последней строкой консоль должна выдать `[100%] Built target formatter_lib`
> 
> Проверить, что мы всё работает, получится только на третьем шаге. Терпение, друзья.

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

> Аналогично, создаём `CMakeLists.txt` в папке `formatter_ex_lib`:
> ```cmake
> cmake_minimum_required(VERSION 3.4)
> project(formatter_ex_lib)
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> 
> add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter_lib_dir)
> # Собираем то, что лежит в formatter_lib и пихаем это в папочку formatter_lib_dir
> add_library(formatter_ex_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
> # Добавляем статичную библиотеку formatter_ex_lib из файла formatter_ex.cpp
> target_include_directories(formatter_ex_lib PUBLIC
> ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib
> )
> # Открываем таргету formatter_ex_lib доступ к директории formatter_lib
> target_link_libraries(formatter_ex_lib formatter_lib)
> # Привязываем библиотеку formatter_lib к библиотеке formatter_ex_lib.
> ```
> После чего собираем командами (не забудьте сменить директорию в консоли!):
> ```
> $ cmake -B build
> $ cmake --build build
> ```

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;

> Наконец-то исполняемый файл! Ура!
> Процесс такой же. Директория `hello_world_application`, файл `CMakeLists.txt`:
> ```cmake
> cmake_minimum_required(VERSION 3.4)
> 
> project(hello_world)
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib formatter_ex_lib_dir)
> 
> add_executable(hello_world ${CMAKE_CURRENT_SOURCE_DIR}/hello_world.cpp)
> # На этот раз вместо библиотеки создаём исполняемый файл. Синтаксис тот же.
> target_include_directories(hello_world PUBLIC
> ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib
> )
> 
> target_link_libraries(hello_world formatter_ex_lib)
> # Привязываем к таргету hello_world библиотеку formatter_ex_lib
> ```
> Вы могли заметить, что подключать библиотеку `formatter_lib` не приходится, потому что она уже подключается при сборке `formatter_ex_lib`.
> 
> Сборка:
> ```sh
> $ cmake -B build
> $ cmake --build build
> ```
> Теперь можно проверить, что всё работает как надо.
> 
> В папке `build` появится исполняемый файл `hello_world`. Запустим его:
> ```sh
> $ build/hello_world
> ```


* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

> Сначала соберём библиотеку `solver_lib`. 
> 
> Есть лишь одна проблема. В коде библиотеки есть **ошибка**: не хватает библиотеки `cmath`, а `sqrtf` не лежит в `std`. Исправим это. В файле `solver_lib/solver.cpp`:
> ```cpp
> #include "solver.h"
> 
> #include <stdexcept>
> #include <cmath>
> 
> void solve(float a, float b, float c, float& x1, float& x2)
> {
>     float d = (b * b) - (4 * a * c);
> 
>     if (d < 0)
>     {
>         throw std::logic_error{"error: discriminant < 0"};
>     }
> 
>     x1 = (-b - sqrtf(d)) / (2 * a);
>     x2 = (-b + sqrtf(d)) / (2 * a);
> }
> 
> ```
> Далее соберём библиотеку.
> 
> Директория `solver_lib`, файл `CMakeLists.txt`:
> ```cmake
> cmake_minimum_required(VERSION 3.4)
> project(solver_lib)
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> add_library(solver_lib ${CMAKE_CURRENT_SOURCE_DIR}/solver.cpp)
> ```
> Директория `solver_application`, файл `CMakeLists.txt`:
> ```cmake
> cmake_minimum_required(VERSION 3.4)
> project(solver)
> set(CMAKE_CXX_STANDARD 11)
> set(CMAKE_CXX_STANDARD_REQUIRED ON)
> add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib formatter_ex_lib_dir)
> add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib solver_lib_dir)
> add_executable(solver ${CMAKE_CURRENT_SOURCE_DIR}/equation.cpp)
> target_include_directories(formatter_ex_lib PUBLIC
> ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib
> ${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib
> )
> target_link_libraries(solver formatter_ex_lib solver_lib)
> ```
> Разница лишь в том, что к таргету `solver` подключаются две библиотеки.
> 
> Сборка:
> ```sh
> $ cmake -B build
> $ cmake --build build
> ```
> Проверим, что всё работает:
> ```sh
> $ build/solver
> ```
> От нас потребуется ввести три числа — коэффициенты квадратного уравнения. На выходе получим решение уравнения.


**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2021 The ISC Authors
```
