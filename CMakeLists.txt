cmake_minimum_required (VERSION 3.25)

set( CMAKE_PREFIX_PATH "" CACHE PATH "prefix path for SDL2 and SDL_ttf" )

project ( RastaConverter )

add_subdirectory( ${CMAKE_CURRENT_SOURCE_DIR}/libextern/FreeImage )

find_package( SDL2 QUIET REQUIRED CONFIG )
find_package( SDL2_ttf QUIET REQUIRED CONFIG )

set( CMAKE_CXX_STANDARD 14 )

add_executable( RastaConverter
  src/CommandLineParser.cpp
  src/config.cpp
  src/Distance.cpp
  src/Evaluator.cpp
  src/mt19937int.cpp
  src/Program.cpp
  src/rasta.cpp
  src/RastaConsole.cpp
  src/RastaSDL.cpp
  src/rgb.cpp
  src/main.cpp
  src/TargetPicture.cpp
)

#target_include_directories( RastaConverter PRIVATE SDL2::SDL2 )
target_link_libraries( RastaConverter PRIVATE SDL2::SDL2 SDL2_ttf::SDL2_ttf FreeImage )

if (WIN32)

  target_compile_definitions( RastaConverter PRIVATE -D_CRT_SECURE_NO_DEPRECATE -D_SCL_SECURE_NO_DEPRECATE )

  get_target_property( SDL2_TTF_BIN SDL2_ttf::SDL2_ttf IMPORTED_LOCATION )
  get_target_property( SDL2_BIN SDL2::SDL2 IMPORTED_LOCATION )

  add_custom_command( TARGET RastaConverter POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different ${SDL2_BIN} $<TARGET_FILE_DIR:RastaConverter>
    COMMAND ${CMAKE_COMMAND} -E copy_if_different ${SDL2_TTF_BIN} $<TARGET_FILE_DIR:RastaConverter>
  )

endif()

add_custom_command( TARGET RastaConverter POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/src/clacon2.ttf $<TARGET_FILE_DIR:RastaConverter>
  COMMAND ${CMAKE_COMMAND} -E copy_directory_if_different ${CMAKE_CURRENT_SOURCE_DIR}/src/Palettes $<TARGET_FILE_DIR:RastaConverter>/Palettes
)
