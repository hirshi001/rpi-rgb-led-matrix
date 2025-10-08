function(add_example example_name)
  add_executable(${example_name} ${example_name})
  target_link_libraries(${example_name} PRIVATE rpi-rgb-led-matrix)
endfunction()

find_package(PkgConfig REQUIRED)
pkg_check_modules(ImageMagick Magick++-7.Q16HDRI)

if(ImageMagick_FOUND)
  message(STATUS "Found ImageMagick 7 (Magick++): ${ImageMagick_VERSION}")

  # Create an imported target for ImageMagick
  add_library(ImageMagick::Magick++ INTERFACE IMPORTED)

  target_include_directories(ImageMagick::Magick++ INTERFACE ${ImageMagick_INCLUDE_DIRS})
  target_link_libraries(ImageMagick::Magick++ INTERFACE ${ImageMagick_LIBRARIES})
  target_compile_options(ImageMagick::Magick++ INTERFACE ${ImageMagick_CFLAGS_OTHER})

  # Add definitions (only if not already included)
  target_compile_definitions(ImageMagick::Magick++ INTERFACE
          MAGICKCORE_HDRI_ENABLE=1
          MAGICKCORE_QUANTUM_DEPTH=16
  )

  # Your example can now link cleanly
else()
  message(WARNING "ImageMagick++ (Magick++-7.Q16HDRI) not found, skipping ImageMagick example.")
endif()


add_example(c-example.c)
add_example(clock.cc)
add_example(demo-main.cc)

find_package(ImageMagick COMPONENTS Magick++)
if(ImageMagick_FOUND)
  add_example(image-example.cc)
  target_link_libraries(image-example.cc PRIVATE ImageMagick::Magick++)
else()
  message(WARNING "ImageMagick++ not found, skipping ImageMagick example.")
endif()

add_example(input-example.cc)
add_example(ledcat.cc)
add_example(minimal-example.cc)
add_example(pixel-mover.cc)
add_example(scrolling-text-example.cc)
add_example(text-example.cc)