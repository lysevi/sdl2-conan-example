cmake_minimum_required(VERSION 3.5)

project( sdl2_conan_example CXX )

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)


include(conan.cmake)

conan_cmake_run(REQUIRES sdl2/2.0.9@bincrafters/stable 
                BASIC_SETUP CMAKE_TARGETS
                BUILD missing
				OPTIONS sdl2:shared=True
				OPTIONS sdl2:iconv=False
				IMPORTS "bin, *.dll -> ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}" # Copies all dll files from packages bin folder to my "bin" folder
				IMPORTS "lib, *.dylib* -> ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}" # Copies all dylib files from packages lib folder to my "bin" folder
				IMPORTS "lib, *.so* -> ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}" # Copies all dylib files from packages lib folder to my "bin" folder
				)

find_package(OpenGL REQUIRED)

add_executable(${PROJECT_NAME} main.cpp)

target_link_libraries(${PROJECT_NAME}
	OpenGL::GL
	CONAN_PKG::sdl2
)

install(TARGETS ${PROJECT_NAME}
    DESTINATION .)

set(DLL_PATTERN "*.dll")

install(
    DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/bin/
    DESTINATION "."
    FILES_MATCHING
    PATTERN
    ${DLL_PATTERN}
)

