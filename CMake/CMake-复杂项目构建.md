## 项目结构

```powershell
cmake_planning_demo/
│  CMakeLists.txt
│  
├─bin
│      libpnc_map.dll
│      libprocess.dll
│      planning_main.exe
│   
├─build
├─docs
├─src
│  │  CMakeLists.txt
│  │  planning_main.cpp
│  │
│  ├─pnc_map
│  │      CMakeLists.txt
│  │      pnc_map.cpp
│  │      pnc_map.h
│  │
│  └─process
│          CMakeLists.txt
│          process.cpp
│          process.h
│
├─test
└─third_party
```

## CMakeLists.txt

### 根目录

```cmake
cmake_minimum_required(VERSION 3.29.0)
project(planning
    VERSION 0.0.1
    DESCRIPTION "a demo of cmake planning"
    HOMEPAGE_URL "https://github.com/platinum-livehand/cmake_planning_demo"
    LANGUAGES CXX
)

set(CMAKE_CXX_STANDARD 17)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

# 目录变量
set(PROCESS_DIR ${CMAKE_SOURCE_DIR}/src/process)
set(PNC_MAP_DIR ${CMAKE_SOURCE_DIR}/src/pnc_map)

# 添加子目录
add_subdirectory(src)
```

### src 目录

```cmake
project(planning_main)

# 添加子目录
add_subdirectory(pnc_map)
add_subdirectory(process)

# 生成可执行程序
add_executable(${PROJECT_NAME} planning_main.cpp)

# 添加包含
target_include_directories(${PROJECT_NAME}
PUBLIC
${PROCESS_DIR}
)

# 链接库
target_link_libraries(${PROJECT_NAME}
PUBLIC
process
)
```

### pnc_map 目录

```cmake
project(pnc_map)

# 生成动态库
add_library(${PROJECT_NAME}
SHARED
pnc_map.cpp
)
```

### process 目录

```cmake
project(process)

# 生成动态库
add_library(${PROJECT_NAME}
SHARED
process.cpp
)

# 添加包含
target_include_directories(${PROJECT_NAME}
PUBLIC
${PNC_MAP_DIR}
)

# 链接库
target_link_libraries(${PROJECT_NAME}
PUBLIC
pnc_map
)
```

## 源码

### pnc_map.h

```c++
#pragma once

class PNC_Map
{
public:
    void mapInfo();
};
```

### pnc_map.cpp

```c++
#include <iostream>
#include "pnc_map.h"

void PNC_Map::mapInfo()
{
    std::cout << "this is pnc_map" << std::endl;
}
```

### process.h

```c++
#pragma once
#include "pnc_map.h"

class Process
{
public:
    void planProcess();
public:
    PNC_Map my_map;
};
```

### process.cpp

```c++
#include <iostream>
#include "process.h"

void Process::planProcess()
{
    std::cout << "this is planning process" << std::endl;
    my_map.mapInfo();
    std::cout << "planning success!" << std::endl;
}
```

### planning_main.cpp

```c++
#include <iostream>
#include "process.h"
#include "show_result.h"

int main()
{
    std::cout << "planning start" << std::endl;
    Process pro;
    pro.planProcess();
    std::cout << "planing end" << std::endl;

    std::cout << "show result: " << std::endl;

    ShowResult show;
    show.drawResult();

    return 0;
}
```

