# 1 设置CMAKE 支持的软件版本版本号
cmake_minimum_required(VERSION 3.5)

# 2 指定工程名称
project(hello) 

# 3 配置CMake的编译器
set(CMAKE_C_COMPILER "gcc") # CMAKE_C_COMPILER是关键词变量：存放编译器的类型，不同平台选择不同的编译器，比如：arm-linux-gnueabihf-gcc 

# 4 配置CMake编译选项
set(CMAKE_CXX_FLAGS "-g -Wall") # CMAKE_CXX_FLAGS是关键词变量：存放编译选项，-g 编译中生成调试信息， -Wall显示警告

# 5 配置生成的文件内容，hex、bin

# 6 获取当前工程路径：定义变量Project_Source_Path，把路径存进去, 后面直接使用此变量
set(Project_Source_Path "${PROJECT_SOURCE_DIR}") # PROJECT_SOURCE_DIR是CMake关键词：获取CMake文件所在路径
message("This Project Path in" ${Project_Source_Path}) # 打印信息

# 7 获取当前控制所有头文件：*.c, 存档Project_Src_Head变量中
file(GLOB_RECURSE Project_Src_Head "${Project_Source_Path}/LearnCmake/*.h") # GLOB_RECURSE 是关键词变量：递归搜寻所有子目录的文件
message("This Project Head " ${Project_Src_Head}) # 打印信息
# 8 添加头文件到环境变量
include_directories(${Project_Src_Head})

# 9 获取当前路径所有源文件：*.c 存到Project_Src_C变量中
file(GLOB_RECURSE Project_Src_C "${Project_Source_Path}/LearnCmake/*.c")
message("This Project C " ${Project_Src_C}) # 打印信息

# 10 生成执行文件
add_executable(hello ${Project_Src_C}) # 编译生成 hello可执行文件

# 11 Generate hex file using objcopy
# 自定义对hello文件进行处理，生成hex文件
add_custom_command(TARGET hello POST_BUILD
    COMMAND ${CMAKE_OBJCOPY} -O ihex hello hello.hex
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    COMMENT "Generating hex file"
)