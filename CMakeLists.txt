cmake_minimum_required(VERSION 3.13)
include(CMakeDependentOption)


project(ArmaPboLib CXX C)

# Options that control generation of various targets.
option(ARMA_PBO_LIB_WRITE "Enable PBO Writing support" ON)

cmake_dependent_option(ARMA_PBO_LIB_SHA1 "Enable SHA1 Hashing on PboWrite" ON
                       "ARMA_PBO_LIB_WRITE" OFF)


set (APL_HEADRES src/pbo.hpp)
						
if (ARMA_PBO_LIB_SHA1)
  set(APL_SOURCES src/pbo.cpp lib/sha1.c)
else()
  set(APL_SOURCES src/pbo.cpp)
endif ()


add_library(ArmaPboLib ${APL_SOURCES} ${APL_HEADRES})
add_library(ArmaPboLib::ArmaPboLib ALIAS ArmaPboLib)

target_compile_features(ArmaPboLib INTERFACE cxx_std_17)
target_include_directories(ArmaPboLib PUBLIC "${PROJECT_SOURCE_DIR}/src")


add_library(ArmaPboLibStatic ${APL_SOURCES} ${APL_HEADRES})
add_library(ArmaPboLib::ArmaPboLibStatic ALIAS ArmaPboLibStatic)
target_compile_features(ArmaPboLibStatic INTERFACE cxx_std_17)
target_include_directories(ArmaPboLibStatic PUBLIC "${PROJECT_SOURCE_DIR}/src")

set_property(TARGET ArmaPboLibStatic PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")


if (ARMA_PBO_LIB_WRITE)
  target_compile_definitions(ArmaPboLib PUBLIC ARMA_PBO_LIB_WRITE)
  target_compile_definitions(ArmaPboLibStatic PUBLIC ARMA_PBO_LIB_WRITE)
endif()

if (ARMA_PBO_LIB_SHA1)
  target_compile_definitions(ArmaPboLib PUBLIC ARMA_PBO_LIB_SHA1)
  target_compile_definitions(ArmaPboLibStatic PUBLIC ARMA_PBO_LIB_SHA1)
endif()





