# CMakeList.txt : CMake project for minichlink, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.8)

# Enable Hot Reload for MSVC compilers if supported.
if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

project ("minichlink")

# Add source to this project's executable.
add_executable (minichlink "minichlink.c" "minichlink.h" "pgm-wch-linke.c" "pgm-esp32s2-ch32xx.c" "nhc-link042.c" "ardulink.c" "serial_dev.c" "pgm-b003fun.c" ) # "minichgdb.c")

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET minichlink PROPERTY CXX_STANDARD 20)
endif()

add_library(libusb STATIC IMPORTED)
#set(LIB_USB_PATH "libusb-1.0.lib")
# Tell CMake where to find the library.
set_target_properties(libusb PROPERTIES IMPORTED_LOCATION "${PROJECT_SOURCE_DIR}/libusb-1.0.lib")
#link_directories("${PROJECT_SOURCE_DIR}")

#add_library("libusb-1.0.lib")

target_compile_definitions(minichlink PRIVATE CH32V003)
#target_compile_definitions(minichlink PRIVATE __ASSEMBLER__)
target_link_libraries(minichlink libusb setupapi wsock32 ws2_32)

# TODO: Add tests and install targets if needed.


#C_S:=minichlink.c pgm-wch-linke.c pgm-esp32s2-ch32xx.c nhc-link042.c ardulink.c serial_dev.c pgm-b003fun.c minichgdb.c