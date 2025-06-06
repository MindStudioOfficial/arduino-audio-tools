cmake_minimum_required(VERSION 3.20)

# set the project name
project(effects)
set (CMAKE_CXX_STANDARD 11)
set (DCMAKE_CXX_FLAGS "-Werror")

# add_compile_options(-Wstack-usage=1024)

# Add Portaduio for desktop build
add_compile_options(-DIS_DESKTOP)
FetchContent_Declare(portaudio GIT_REPOSITORY "https://github.com/PortAudio/portaudio.git" GIT_TAG v19.7.0 )
FetchContent_GetProperties(portaudio)
if(NOT portaudio_POPULATED)
    FetchContent_Populate(portaudio)
    add_subdirectory(${portaudio_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR}/portaudio)
endif()

# Build with arduino-audio-tools
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../.. ${CMAKE_CURRENT_BINARY_DIR}/arduino-audio-tools )
endif()


# build sketch as executable
add_executable (effects effects.cpp)
# set preprocessor defines
target_compile_definitions(effects PUBLIC -DARDUINO -DIS_DESKTOP)

# specify libraries
target_link_libraries(effects arduino_emulator arduino-audio-tools portaudio)

