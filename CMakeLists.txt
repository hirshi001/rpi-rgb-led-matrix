
# Gather all source files in lib/
file(GLOB RGBMATRIX_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/lib/*.cc")

# Create a static library from the sources
add_library(rpi-rgb-led-matrix STATIC ${RGBMATRIX_SOURCES})

# Set the include directories
target_include_directories(rpi-rgb-led-matrix PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        ${CMAKE_CURRENT_SOURCE_DIR}/lib
)