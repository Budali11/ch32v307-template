cmake_minimum_required(VERSION 3.10)

project(test)

# link with linkerscripts
set(LINKER_SCRIPT ${PROJECT_SOURCE_DIR}/Link.ld)
set(CMAKE_EXE_LINKER_FLAGS ${CMAKE_EXE_LINKER_FLAGS} -T${LINKER_SCRIPT})

# add peripheral lib
include_directories(${PROJECT_SOURCE_DIR}/Peripheral/inc ${PROJECT_SOURCE_DIR}/Core ${PROJECT_SOURCE_DIR}/Debug)
set(PERIPH_LIB_DIR ${PROJECT_SOURCE_DIR}/Peripheral)
file(GLOB PERIPH_LIB ${PERIPH_LIB_DIR}/src/ch32v30x_*.c)

# add C sources
set(C_SOURCES ${PROJECT_SOURCE_DIR}/Core/core_riscv.c ${PROJECT_SOURCE_DIR}/Debug/debug.c)

# enable ASM
enable_language(ASM)
# add assemble sources
set(AS_SOURCES ${PROJECT_SOURCE_DIR}/Startup/startup_ch32v30x_D8C.S)

# specify riscv gnu compiler
set(CMAKE_C_COMPILER /usr/local/riscv/riscv32-newlib/bin/riscv32-unknown-elf-gcc)
set(CMAKE_CXX_COMPILER /usr/local/riscv/riscv32-newlib/bin/riscv32-unknown-elf-g++)
set(CMAKE_ASM_COMPILER /usr/local/riscv/riscv32-newlib/bin/riscv32-unknown-elf-as)

add_subdirectory(User)


