# Ubuntu 20.04 installation

## Install Ubuntu packages
Update Ubuntu

    sudo apt update && sudo apt upgrade

Install gcc compiler, cairo grapics, GraphicsMagick, libpng, cmake, git

    sudo apt install build-essential
    sudo apt install libcairo2-dev
    sudo apt install libgraphicsmagick1-dev
    sudo apt install libpng-dev
    sudo apt install cmake
    sudo apt install git-all
  
## Download Udacity and third party code 
1. Fork the [Udacity repository](https://github.com/udacity/CppND-Route-Planning-Project) (see [Fork a repository](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo))

2. Download the forked repository: 
    
        git clone https://github.com/YOUR-USERNAME/CppND-Route-Planning-Project

3. The provided googletest submodule does not compile, remove it and commit change:
```
        cd CppND-Route-Planning-Project
        git rm thirdparty/googletest && git commit -m "removed googletest"
```
4. Add current version of googletest:

        git submodule add https://github.com/google/googletest.git thirdparty/googletest

5. Add IO2D library to git submodules
        
        git submodule add https://github.com/cpp-io2d/P0267_RefImpl ./thirdparty/P0267_RefImpl

    Commit changes and push to remote
    
        git commit -m "Added googletest and P0267_RefImpl to the project"
        git push

4. Initialize and download all thirdparty submodules -> googletest, P0267_RefImpl, pugixml (see [Git Tools - Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules))

        git submodule init
        git submodule update


## Build IO2D library P0267_RefImpl
1. The library only compiles on my machine when commenting out the following sections in the `CMakeLists.txt` file.

    ```
    # COMMENT OUT: 
    #if( NOT DEFINED IO2D_WITHOUT_SAMPLES )
    # 	add_subdirectory(P0267_RefImpl/Samples)
    # endif()


    # if( NOT DEFINED IO2D_WITHOUT_TESTS )
    # 	enable_testing()
    # 	add_subdirectory(P0267_RefImpl/Tests)
    # endif()
    ```

2. Now build the library. In the `CppND-Route-Planning-Project/thirdparty/P0267_RefImpl` directory enter

        mkdir Debug && cd Debug
        cmake "-DCMAKE_BUILD_TYPE=Debug" ..
        cmake --build .


## Compile project code
Now the project code be compiled:

cd to `CppND-Route-Planning-Project` and enter

    mkdir build && cd build
    cmake ..
    make


## Running
In `build` run:

    ./OSM_A_star_search 

Output:
```
To specify a map file use the following format: 
Usage: [executable] [-f filename.osm]
Reading OpenStreetMap data from the following file: ../map.osm
Distance: 0 meters.
```
and shows a window with a map.

## Testing
The testing executable is also placed in the build directory. Run:

    ./test

Output:
```Running main() from /home/dluchten/tmp/CppND-Route-Planning-Project/thirdparty/googletest/googletest/src/gtest_main.cc
[==========] Running 4 tests from 1 test suite.
[----------] Global test environment set-up.
[----------] 4 tests from RoutePlannerTest
[ RUN      ] RoutePlannerTest.TestCalculateHValue
Illegal instruction (core dumped)
```



