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
### Build project từ folder chính
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

## 
### Khai báo dự án
- Khai báo version CMake tối thiểu
```cmake
cmake_minimum_required(VERSION 3.10)
``` 
- khai báo thông tin dự án
``` cmake
project(myproj 
    VERSION 1.0
    DESCRIPTION "Hello World Project"
    LANGUAGES C )
```
</br>=> Sau khi khai báo, CMake tạo ra các biến
``` cmake
PROJECT_NAME
PROJECT_VERSION
PROJECT_SOURCE_DIR
PROJECT_BINARY_DIR
```

### Biến trong CMake
- Gán giá trị cho biến
```cmake
set(VAR_NAME "Hello")
```
- Truy cập biến
```cmake
${VAR_NAME}
```
- Xóa biến
```
unset(VAR_NAME)
```
### List
- danh sách được lưu dưới dạng các phần tử ngăn cách bằng dấu ``;``
```
set(SRC
    main.cpp
    foo.cpp
    bar.cpp
)
=> SRC = main.cpp;foo.cpp;bar.cpp
```
- Thêm phần tử cho list
```
list(APPEND SRC test.cpp)

=> SRC = main.cpp;foo.cpp;bar.cpp;test.cpp
```
- lấy phần tử
``` cmake
list(GET SRC 0 FIRST) # main.cpp
message(${FIRST})
```
### include file CMake
```cmake
include(cmake/Utils.cmake)
```
### Hiển thị log
```
message(STATUS "")
message(WARNING "")
message(FATAL_ERROR "")
```

### Tạo file thực thi 
```cmake
add_executable(MyApp
    main.cpp
    foo.cpp
)
```
</br>=> Sau khi build tạo ra MyApp.exe/MyApp


### Tạo thư viện
``` cmake
add_library(led STATIC
    led.c
    rgb_led.c
    ws2812.c
)
```
</br> => tạo ra file libled.a
```
libled.a
├── led.o
├── rgb_led.o
└── ws2812.o
```

### Liên kết thư viện
```
target_link_libraries(led)
```


### thêm một thư mục con vào quá trình build.
```
Project/
│
├── CMakeLists.txt
│
├── BSP/
│   └── led/
│       ├── CMakeLists.txt
│       ├── led.c
│       └── led.h
│
└── App/
    ├── CMakeLists.txt
    └── app.c
```
```cmake 
# ~/CMakeLists.txt
add_subdirectory(BSP/led)
```
<br> => Khi gặp lệnh add_subirectory()
- Đi vào thư mục BSP/led.
- Tìm file BSP/led/CMakeLists.txt.
- Thực thi toàn bộ nội dung của file đó.
- Quay lại file Project/CMakeLists.txt và tiếp tục dòng tiếp theo.





