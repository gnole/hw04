## Homework III
Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeLists.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

Создание CMakeLists.txt для автоматизированной сборки библиотеки в каталоге `formatter_lib`
```sh
% cd formatter_lib
# Устанавливаем минимальную версию CMake и название проекта
% cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(formatter)
EOF
```
Устанавливаем стандарт языка и делаем его обязательным
```sh
% cat >> CMakeLists.txt <<EOF

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```
Сборка статической библиотеки с именем *formatter* и исходным файлом `formatter.cpp`
```sh
% cat >> CMakeLists.txt <<EOF

add_library(formatter STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
EOF
```
Добавление директории с хэдерами и указание, где искать заголовочные файлы
``` sh
% cat >> CMakeLists.txt <<EOF

include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
EOF
```
Создание директории, которую CMake будет использовать в качестве корневого каталога сборки и указание каталога, где искать исходные файлы для проекта и последующая сборка
```sh
% cmake -H. -B_build
...
-- Build files have been written to: /Users/evgengrmit/Evgengrmit/workspace/projects/hw03/formatter_lib/_build
% cmake --build _build
Scanning dependencies of target formatter
[ 50%] Building CXX object CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter
```
Убеждаемся, что статическая библиотека *formatter* действительно создана.
```sh
% ls _build/libformatter.a
_build/libformatter.a
```
Первое задание выполнено.

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeLists.txt` для статической библиотеки *formatter*, ваш
руководитель поручает заняться созданием `CMakeLists.txt` для библиотеки
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

Создание CMakeLists.txt для автоматизированной сборки библиотеки в каталоге `formatter_ex_lib `
```sh
% cd formatter_ex_lib
# Устанавливаем минимальную версию CMake и название проекта
% cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(formatter_ex)
EOF
```
Устанавливаем стандарт языка и делаем его обязательным, а так же для удобства изменим переменную `CMAKE_CURRENT_SOURCE_DIR`
```sh
% cat >> CMakeLists.txt <<EOF

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Users/evgengrmit/Evgengrmit/workspace/projects/hw03)
EOF
```
Сборка статической библиотеки с именем *formatter_ex_lib* и исходным файлом `formatter.cpp`
```sh
% cat >> CMakeLists.txt <<EOF

add_library(formatter_ex STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
EOF
```
Добавление директории с хэдерами и указание, где искать заголовочные файлы
``` sh
% cat >> CMakeLists.txt <<EOF

include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
EOF
```
Указание директории, где находятся хедэры библиотеки *formatter* и компоновка текущей библиотеки с библиотекой *formatter*
``` sh
% cat >> CMakeLists.txt <<EOF

include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
target_link_libraries(formatter_ex formatter)
EOF
```
Создание директории, которую CMake будет использовать в качестве корневого каталога сборки и указание каталога, где искать исходные файлы для проекта и последущая сборка
```sh
% cmake -H. -B_build
...
-- Build files have been written to: /Users/evgengrmit/Evgengrmit/workspace/projects/hw03/formatter_ex_lib/_build
% cmake --build _build
Scanning dependencies of target formatter_ex
[ 50%] Building CXX object CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex.a
[100%] Built target formatter_ex
```
Убеждаемся, что статическая библиотека *formatter* действительно создана.
```sh
% ls _build/libformatter_ex.a
_build/libformatter_ex.a
```
Второе задание выполнено.
### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeLists.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
Создание CMakeLists.txt для автоматизированной сборки библиотеки в каталоге `formatter_ex_lib `
```sh
% cd hello_world_application
# Устанавливаем минимальную версию CMake и название проекта
% cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(hello_world)
EOF
```
Устанавливаем стандарт языка и делаем его обязательным, а так же для удобства изменим переменную `CMAKE_CURRENT_SOURCE_DIR`
```sh
% cat >> CMakeLists.txt <<EOF

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Users/evgengrmit/Evgengrmit/workspace/projects/hw03)
EOF
```
Добавление исполняемого файла в соответствующий проект
```sh
% cat >> CMakeLists.txt <<EOF

add_executable(hello_world \${CMAKE_CURRENT_SOURCE_DIR}/hello_world_application/hello_world.cpp)
EOF
```
Указание директорий, где находятся хедэры библиотек *formatter* и *formatter_ex* и поиск библиотек в данных директориях.
```sh
% cat >> CMakeLists.txt <<EOF

include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)

find_library(formatter NAMES libformatter.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
find_library(formatter_ex NAMES libformatter_ex.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
EOF
```
Линковка проекта с найденными библиотеками
```sh
% cat >> CMakeLists.txt <<EOF

target_link_libraries(hello_world \${formatter} \${formatter_ex})
EOF
```
Сборка проекта с помощью CMake в директории `_build`
```sh
# Создание каталога `_build` со сборочными файлами проекта
% cmake -H. -B_build
...
-- Build files have been written to: /Users/evgengrmit/Evgengrmit/workspace/projects/hw03/hello_world_application/_build
% cmake --build _build
Scanning dependencies of target hello_world
[ 50%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
```
Запускаем проект
```sh
% _build/hello_world &&
-------------------------
hello, world!
-------------------------

```

* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.
```sh
% cd solver_application
# Устанавливаем минимальную версию CMake и название проекта
% cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(solver)
EOF
```
Устанавливаем стандарт языка и делаем его обязательным, а так же для удобства изменим переменную `CMAKE_CURRENT_SOURCE_DIR`
```sh
% cat >> CMakeLists.txt <<EOF

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Users/evgengrmit/Evgengrmit/workspace/projects/hw03)
EOF
```
Сборка статической библиотеки с именем *solver_lib* и исходным файлом `solver.cpp`
```sh
% cat >> CMakeLists.txt <<EOF

add_library(solver_lib STATIC \${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/solver.cpp)
EOF
```
Добавление директории с хэдерами и указание, где искать заголовочные файлы
``` sh
% cat >> CMakeLists.txt <<EOF

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/solver_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
EOF
```
Создание директории, которую CMake будет использовать в качестве корневого каталога сборки и указание каталога, где искать исходные файлы для библиотеки и последующая сборка
```sh
% cmake -H. -B_build
...
-- Build files have been written to: /Users/evgengrmit/Evgengrmit/workspace/projects/hw03/solver_application/_build
% cmake --build _build
Scanning dependencies of target solver_lib
[ 50%] Building CXX object CMakeFiles/solver_lib.dir/Users/evgengrmit/Evgengrmit/workspace/projects/hw03/solver_lib/solver.cpp.o
[100%] Linking CXX static library libsolver_lib.a
[100%] Built target solver_lib
```
Добавление исполняемого файла в соответствующий проект
```sh
% cat >> CMakeLists.txt <<EOF

add_executable(solver \${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)
EOF
```
Указание директорий, где находятся хедэры библиотек *formatter* и *formatter_ex* и поиск библиотек в данных директориях.
```sh
% cat >> CMakeLists.txt <<EOF

find_library(formatter NAMES libformatter.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
find_library(formatter_ex NAMES libformatter_ex.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
find_library(solver_lib NAMES libsolver_lib.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)
EOF
```
Линковка проекта с найденными библиотеками
```sh
% cat >> CMakeLists.txt <<EOF

target_link_libraries(solver \${formatter} \${formatter_ex} \${solver_lib})
EOF
```
Сборка проекта с помощью CMake в директории `_build`
```sh
# Создание каталога `_build` со сборочными файлами проекта
% cmake -H. -B_build
...
-- Build files have been written to: /Users/evgengrmit/Evgengrmit/workspace/projects/hw03/hello_world_application/_build
% cmake --build _build
Scanning dependencies of target hello_world
[ 50%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
```
Сборка проекта с помощью CMake в директории `_build`
```sh
$ cmake --build _build # Сборка всего проекта
Scanning dependencies of target solver
[ 25%] Building CXX object CMakeFiles/solver.dir/equation.cpp.o
[ 50%] Linking CXX executable solver
[ 50%] Built target solver
Scanning dependencies of target solver_lib
[ 75%] Building CXX object CMakeFiles/solver_lib.dir/Users/evgengrmit/Evgengrmit/workspace/projects/hw03/solver_lib/solver.cpp.o
[100%] Linking CXX static library libsolver_lib.a
[100%] Built target solver_lib
$ cmake --build _build --target solver_lib # Сборка только статической библиотеки
[100%] Built target solver_lib
$ cmake --build _build --target solver # Сборка solver
[100%] Built target solver
```
Запускаем проект
```sh
% _build/solver && echo
1 5 6
-------------------------
x1 = -3.000000
-------------------------
-------------------------
x2 = -2.000000
-------------------------

```
Третье задание выполнено.


**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2020 The ISC Authors
```
