cmake_minimum_required(VERSION 2.8.11)

macro(compileAsC99)
  if (CMAKE_VERSION VERSION_LESS "3.1")
    if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
      set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
      set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
    endif()
  else()
    set (CMAKE_C_STANDARD 99)
    set (CMAKE_CXX_STANDARD 11)
  endif()
endmacro(compileAsC99)

compileAsC99()

add_library(my_parson
    parson.c
    parson.h
)

set(AZUREIOT_INC_FOLDER ".." "/usr/include/azureiot" "/usr/include/azureiot/inc")

include_directories(${AZUREIOT_INC_FOLDER})

set(iothub_client_sample_module_sender_c_files
    ./iothub_client_sample_module_filter.byTemp.c
)

set(iothub_client_sample_module_sender_h_files
    ./iothub_client_sample_module_filter.h
)

add_executable(iothub_client_sample_module_sender ${iothub_client_sample_module_sender_c_files} ${iothub_client_sample_module_sender_h_files})

target_link_libraries(iothub_client_sample_module_sender
    my_parson
    iothub_client_mqtt_transport
    iothub_client
    parson
    aziotsharedutil
    umqtt
    pthread
    curl
    ssl
    crypto
    m
)