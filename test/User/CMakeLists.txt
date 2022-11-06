# include paths
include_directories(.)

set(C_SOURCES ${C_SOURCES} main.c ch32v30x_it.c system_ch32v30x.c)

# add executable
add_executable(test ${C_SOURCES} ${PERIPH_LIB} ${AS_SOURCES})
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
