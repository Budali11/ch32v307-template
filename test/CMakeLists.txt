cmake_minimum_required(VERSION 3.10)

set(CMAKE_SYSTEM_PROCESSOR risc-v)
set(TARGET      test)

project(${TARGET})

# risc-v cpu
add_compile_options(-march=rv32imac -mabi=ilp32 -g)

# link with linkerscripts
set(LINKER_SCRIPT ${PROJECT_SOURCE_DIR}/Link.ld)
add_link_options(-Wl,-Map=${TARGET}.map -T${LINKER_SCRIPT})

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
set(CMAKE_C_COMPILER /usr/local/riscv/riscv-wch/bin/riscv-none-embed-gcc)
set(CMAKE_CXX_COMPILER /usr/local/riscv/riscv-wch/bin/riscv-none-embed-g++)
set(CMAKE_ASM_COMPILER /usr/local/riscv/riscv-wch/bin/riscv-none-embed-as)

# add phony targets
set(ERASE -f /usr/local/share/openocd/scripts/interface/wch-riscv.cfg -c init -c halt -c "flash erase_sector wch_riscv 0 last" -c exit)
set(DOWNLOAD -f wch-riscv.cfg  -c init -c halt  -c "program ${TARGET}.elf"  -c exit)
set(VERIFY -f wch-riscv.cfg  -c init -c halt  -c "verify_image ${TARGET}.elf"  -c exit)
add_custom_target(erase 
    COMMAND ${PROJECT_SOURCE_DIR}/openocd ${ERASE})
add_custom_target(download 
    COMMAND ${PROJECT_SOURCE_DIR}/openocd ${DOWNLOAD})
add_custom_target(verify 
    COMMAND ${PROJECT_SOURCE_DIR}/openocd ${VERIFY})

add_subdirectory(User)
