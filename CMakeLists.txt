cmake_minimum_required (VERSION 3.25)

project ( RastaConverter )

set( CMAKE_CXX_STANDARD 14 )

add_subdirectory( ${CMAKE_CURRENT_SOURCE_DIR}/libextern/FreeImage )

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
  #src/main2.cpp
  src/TargetPicture.cpp
)

target_include_directories( RastaConverter PRIVATE "c:\\local\\SDL2-2.0.22\\include" )
#target_include_directories( RastaConverter PRIVATE ${CMAKE_SOURCE_DIR}/libextern/ )
target_link_libraries( RastaConverter PRIVATE FreeImage )
target_link_libraries( RastaConverter PRIVATE c:/local/SDL2-2.0.22/lib/x64/SDL2.lib)

if (WIN32)
  target_compile_definitions( RastaConverter PRIVATE -D_CRT_SECURE_NO_DEPRECATE )
  target_compile_definitions( RastaConverter PRIVATE -D_SCL_SECURE_NO_DEPRECATE )
endif()
