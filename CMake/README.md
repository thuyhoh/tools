# CMake 
- CMake tlà một build system generator — công cụ mô tả cách build project (compile, link, include thư viện…), rồi nó sinh ra file build cho tool khác như Make, Ninja, hoặc IDE.
## Workflow của CMake
- CMake không trực tiếp compile code
```
CMakeLists.txt
      ↓
   cmake
      ↓
 Makefile / Ninja
      ↓
 make / ninja
      ↓
 binary
```
## Cấu trúc project
```
my_project/
├── CMakeLists.txt      # File CMake chính (root)
├── src/                # Source code (.c/.cpp)
│   ├── main.c
│   ├── gpio.c
│   └── uart.c
├── include/            # Header files (.h)
│   ├── gpio.h
│   └── uart.h
├── lib/                # Libraries (optional)
├── build/              # Generated build files (không commit git)
└── docs/               # Tài liệu (optional)
```

## Cấu trúc CMakeLists.txt 
### Version CMake tối thiểu
```
cmake_minimum_required(VERSION 3.10)
```
### Project infor

# 3. Variables / options
### Khởi tạo project

## Build project
### Build project từ folder ./build
```
thuy@thuy-ASUS:~/embedded/tools/CMake/hello_project/build$ cmake ..
-- The C compiler identification is GNU 13.3.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/gcc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Configuring done (0.1s)
-- Generating done (0.0s)
-- Build files have been written to: /home/thuy/embedded/tools/CMake/hello_project/build

thuy@thuy-ASUS:~/embedded/tools/CMake/hello_project/build$ ls
CMakeCache.txt  CMakeFiles  cmake_install.cmake  Makefile

thuy@thuy-ASUS:~/embedded/tools/CMake/hello_project/build$ cmake --build .
[ 50%] Building C object CMakeFiles/hello.dir/main.c.obj
[100%] Linking C executable 
```

```
thuy@thuy-ASUS:~/embedded/tools/CMake/hello_project$ cmake -B ./build/
-- The C compiler identification is GNU 13.3.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/gcc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Configuring done (0.1s)
-- Generating done (0.0s)
-- Build files have been written to: /home/thuy/embedded/tools/CMake/hello_project/build
```


## Variable
- Trong CMake có 3 loại biến:
    - Normal variable
    - Cache variable
    - enviroment variable
``` cmake
# Gán giá trị cho biến
set(PROJECT_NAME hello) # PROJECT_NAME → "hello"
set(SRC main.c utils.c) # SRC → "main.c utils.c"

# Sử dụng biến 
${VARIABLE_NAME}
```
### Normal variable
- Chỉsống trong scope hiện tại.
### Cache variable
- Lưu trong CMakeCache.txt, tồn tại qua nhiều lần build.
```
set(BOARD stm32 CACHE STRING "Target board")
```
=> kiểm tra 
### Environment variable
- Lấy từ shell/Linux environment.
``` sh
# linux shell
export TOOLCHAIN=/opt/gcc-arm

# cmake 
$ENV{TOOLCHAIN}
```
### Một số biến thường sử dụng 
- CMAKE_SOURCE_DIR
- CMAKE_CURRENT_SOURCE_DIR



## Hiển thị biến 
- Sử dụng message
``` CMake
message("Hello")
message(STATUS "Hello STATUS")
message(DEBUG "Hello DEBUG")
message(WARNING "Hello WARNING")
message(FATAL ERROR "Hello ERROR")
```
- sử dụng thư viện CMakePrintHelpers
```
include (CMakePrintHelpers)
cmake_print_variables(variable)
```


## GLOB và GLOB_RECURSE
- GLOB dùng để lấy danh sách file theo pattern.
```
file(GLOB SRC_FILES src/*.c) # gán toàn bộ file .c trong folder src/ cho biến SRC_FILES

# => kết quả
SRC_FILES =
    src/main.c
    src/uart.c
    src/spi.c 
```
- GLOB_RECURSE giống với GLOB nhưng quét toàn bộ đệ quy tất cả subfolder

## target_include_directories
- Dùng để nói với compiler Header files của target nằm ở đâu.
``` cmake
target_include_directories(target scope dir1 dir2 ...)
```
### scope: PRIVATE / PUBLIC / INTERFACE



