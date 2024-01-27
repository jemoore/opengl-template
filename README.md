# Purpose
This template can be used to create a C++ project using glad and glfw for opengl.

# Conan
Why use conan?
- easier dependency management
- repeatability
- - conanfile.txt has dependencies labeled with version and file can be placed in source control
- crossplatform
- often used in 'professional' environments

## Install conan 2 or later

```
# On Windows
winget conan
# or
winget upgrade conan
```

## Create profile if it does not exist

```
conan profile detect
```

Creates ~/.conan2/profiles/default
Will create a Release build profile.  Copy the file to create a debug profile named 'debug'. Inside the file change the build type from Release to Debug.

## Get dependencies with conan
Third party dependencies are listed in the file conanfile.txt
To download the dependencies:
```
conan install . -sbuild_type=Debug -of="conan/debug" --build=missing
```

# Configure with CMake

From the 'Developer Command Prompt' command line:

```
mkdir build
cd build

cmake .. -G "Visual Studio 17 2022" -DCMAKE_BUILD_TYPE=Debug -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake
# or on Linux
cmake .. -DCMAKE_BUILD_TYPE=Debug -DCMAKE_TOOLCHAIN_FILE=../conan/debug/build/Debug/generators/conan_toolchain.cmake

cmake --build .

# or stay in current dir
cmake -B build -S src -DCMAKE_BUILD_TYPE=Debug -DCMAKE_TOOLCHAIN_FILE=../conan/debug/build/Debug/generators/conan_toolchain.cmake
cmake --build build

# On Windows the above cmake command doesn't always seem to work.  When running the CMake configure and build from within VSCode, it works with no problem.  It uses the following commnd:
cmake -DCMAKE_INSTALL_PREFIX=W:/cpp/opengl/build/vs2022-debug/install "-DCMAKE_CXX_FLAGS_INIT=  /W4 /WX /EHsc" -DCMAKE_BUILD_TYPE=Debug -DBUILD_TESTS=true -DCMAKE_EXPORT_COMPILE_COMMANDS=true -DCMAKE_TOOLCHAIN_FILE=W:/cpp/opengl/conan/debug/build/generators/conan_toolchain.cmake -SW:/cpp/opengl -BW:/cpp/opengl/build/vs2022-debug -G "Visual Studio 17 2022"
# then to build:
cmake --build W:/cpp/opengl/build/vs2022-debug --config Debug
```

# Change to CMakePresets.json

I am not yet sure of the cause but I have seen that on occassion the created conan directory structure is different from what is in the CMakePresets.json file.
Sometimes the created directory structure is: conan/debug/build/Debug/generators/conan_toolchain.cmake (there is an extra Debug directory)
After running 'conan install' check this path in CMakePresets.json to make sure the paths match (before running cmake).

Here is the original:

 "name": "conan-debug",
 "hidden": true,
 "cacheVariables": {
     "CMAKE_TOOLCHAIN_FILE": "${sourceDir}/conan/debug/build/generators/conan_toolchain.cmake"
 }
