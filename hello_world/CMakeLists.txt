project(helloworld LANGUAGES C CXX)
cmake_minimum_required(VERSION 3.5)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(CycloneDDS-CXX REQUIRED)

include_directories(${CycloneDDS_INCLUDE_DIRS})

# Convenience function, provided by the idlc backend for CXX that generates a CMake
# target for the given IDL file. The function calls idlc to generate
# source files and compiles them into a library.
idlcxx_generate(TARGET ddscxxHelloWorldData_lib FILES HelloWorldData.idl WARNINGS no-implicit-extensibility)

add_executable(ddscxxHelloworldPublisher publisher.cpp)
add_executable(ddscxxHelloworldSubscriber subscriber.cpp)

# Link both executables to IDL data type library and ddscxx.
target_link_libraries(ddscxxHelloworldPublisher ddscxxHelloWorldData_lib CycloneDDS-CXX::ddscxx)
target_link_libraries(ddscxxHelloworldSubscriber ddscxxHelloWorldData_lib CycloneDDS-CXX::ddscxx)
