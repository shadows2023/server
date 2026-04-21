# server

基于 Boost.Asio 的文件传输服务器

## 功能


## 依赖
- CMake >= 3.20
- C++ 编译器，支持 C++20（示例使用 g++ 13）
- Boost.System / Boost.Asio（系统安装或通过包管理器安装）

## 构建（在项目根目录）
```sh
mkdir -p build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build . --config Release