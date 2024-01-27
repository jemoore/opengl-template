cmake_minimum_required(VERSION 3.17)
project(opengl-example)

# Generate compile_commands.json
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Disable useless warnings

# Avoid warning about DOWNLOAD_EXTRACT_TIMESTAMP in CMake 3.24
if (CMAKE_VERSION VERSION_GREATER_EQUAL "3.24.0")
	cmake_policy(SET CMP0135 NEW)
endif()
if (CMAKE_VERSION VERSION_GREATER_EQUAL "3.13.0")
	cmake_policy(SET CMP0077 NEW)
endif()

# Disable useless compiler warnings
if (MSVC)
	add_compile_options(/wd4068) # Disable unknown pragma warning.
	add_compile_options(/wd4996) # Disable unsecure function warnings. (e.g. sprintf())
endif()

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(src)
