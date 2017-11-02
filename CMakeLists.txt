cmake_minimum_required(VERSION 2.7)

project(ecdsa++)

set(CMAKE_MACOSX_RPATH 1)

find_package(OpenSSL REQUIRED)
find_library(SECP256K1 secp256k1)

include_directories(
  "./include/ecdsa"
  "./googletest/googletest"
  "./googletest/googletest/include"
  ${OPENSSL_INCLUDE_DIR}
  )

set(CMAKE_CXX_FLAGS "-std=c++11")

file(GLOB src "./src/*.cc")

add_library(ecdsa++ SHARED ${src})

target_link_libraries(ecdsa++ ${OPENSSL_LIBRARIES} ${SECP256K1})

file(GLOB gtest
  "./googletest/googletest/src/gtest-all.cc"
  "./googletest/googletest/src/gtest_main.cc"
  )
file(GLOB ecdsa_test "./test/test.cc")

add_executable(ecdsa_test ${src} ${gtest} ${ecdsa_test})

target_link_libraries(ecdsa_test ${OPENSSL_LIBRARIES} ${SECP256K1})