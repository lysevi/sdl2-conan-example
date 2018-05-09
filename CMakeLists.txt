cmake_minimum_required(VERSION 3.5)

project( sdl2_conan_example CXX )

include(conan.cmake)

conan_cmake_run(REQUIRES sdl2/2.0.8@bincrafters/stable
                BASIC_SETUP CMAKE_TARGETS
                BUILD missing
				OPTIONS sdl2:shared=True
				IMPORTS "bin, *.dll -> ./bin" # Copies all dll files from packages bin folder to my "bin" folder
				IMPORTS "lib, *.dylib* -> ./bin" # Copies all dylib files from packages lib folder to my "bin" folder
				IMPORTS "lib, *.so* -> ./bin" # Copies all dylib files from packages lib folder to my "bin" folder
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
#if( ${CMAKE_BUILD_TYPE}=="Debug" )
#	set(DLL_PATTERN "*d.dll")
#else()
#	set(DLL_PATTERN "*[^d].dll")
#endif()

install(
    DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/bin/
    DESTINATION "."
    FILES_MATCHING
    PATTERN
    ${DLL_PATTERN}
)

