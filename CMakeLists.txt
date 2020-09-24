cmake_minimum_required(VERSION 2.8 FATAL_ERROR) # we need at least 3.11 because we use the FetchContent feature to manage dependencies and at least 3.14 because we use FetchContent_MakeAvailable

project(local_exp)


# include(FetchContent)

###   VARIABLES   ##############################################################
set(CMAKE_BUILD_TYPE RelWithDebInfo)
set(CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH}" "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -O3 -Wno-reorder -fopenmp") #we need c++17 because this solves alignment issues with eigen http://eigen.tuxfamily.org/bz/show_bug.cgi?id=1409
set(CMAKE_CXX_STANDARD 17) #we need c++17 because this solves alignment issues with eigen http://eigen.tuxfamily.org/bz/show_bug.cgi?id=1409
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)



####   GLOBAL OPTIONS   ###https://stackoverflow.com/questions/15201064/cmake-conditional-preprocessor-define-on-code
# option(CORE_WITH_VIEWER "Use a GL viewer, or if we don't have it available we will still publish to RViz" ON)
# option(CORE_WITH_HALIDE "Compile Halide" OFF)
# option(CORE_WITH_TORCH "Compile Torch" OFF)
# option(CORE_WITH_CUDA "Compile CUDA" OFF)
# option(CORE_WITH_GLM "With GLM for some quality of life functions in EasyGL" OFF)
# option(CORE_WITH_DIR_WATCHER "Compile with the dir_watcher dependency from emildb" OFF)





######   PACKAGES   ############################################################
find_package(OpenCV REQUIRED)
find_package(Boost REQUIRED COMPONENTS system thread filesystem)



###   INCLUDES   #########################################################
set(PROJECT_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/LocalExpansionStereo
                        ${PROJECT_SOURCE_DIR}/maxflow
                        ) # Header folder
include_directories(${PROJECT_INCLUDE_DIR} )
include_directories(${OpenCV_INCLUDE_DIRS})
include_directories(${Boost_INCLUDE_DIR})




###   SOURCES   #################################################################
set(MY_SRC  
    ${PROJECT_SOURCE_DIR}/LocalExpansionStereo/main.cpp
    ${PROJECT_SOURCE_DIR}/maxflow/graph.cpp
    ${PROJECT_SOURCE_DIR}/maxflow/maxflow.cpp
    
)

###   SET ALL THE GLOBAL OPTIONS #########################################
# if(CORE_WITH_VIEWER)
#     message("USING GL Viewer")
#     add_definitions(-DWITH_VIEWER)
# else()
#     message("USING OFFSCREEN")
# endif()


###   MAIN LIB  ####################


###   EXECUTABLE   #######################################
add_executable(local_exp ${MY_SRC}  )


###   LIBS   ###############################################
set(LIBS -lpthread -ldl ) #because loguru needs them
# if(CORE_WITH_DIR_WATCHER)
#     set(LIBS  ${LIBS} ${KQUEUE_LIBRARIES})
# endif()
# if(${TORCH_FOUND})
#     set(LIBS ${LIBS} ${TORCH_LIBRARIES} )
# endif()
set(LIBS ${LIBS}  ${OpenCV_LIBS}  ${Boost_LIBRARIES}  -fopenmp )


target_link_libraries(local_exp ${LIBS} )
# target_link_libraries(easypbr PRIVATE easypbr_cpp)
# target_link_libraries(run_easypbr PRIVATE easypbr_cpp )


#definitions for cmake variables that are necesarry during runtime
# target_compile_definitions(easypbr_cpp PRIVATE PROJECT_SOURCE_DIR="${PROJECT_SOURCE_DIR}") #point to the cmakelist folder of the easy_pbr
# target_compile_definitions(easypbr_cpp PRIVATE CMAKE_SOURCE_DIR="${CMAKE_SOURCE_DIR}") # points to the CMakeList folder of whichever project included easy_pbr
# target_compile_definitions(easypbr_cpp PRIVATE AWESOMEFONT_DIR="${PROJECT_SOURCE_DIR}/extern/imgui_ext/fontawesome-webfont.ttf")
# target_compile_definitions(easypbr_cpp PRIVATE PROGGY_DIR="${imgui_SOURCE_DIR}/misc/fonts/ProggyClean.ttf")

#  target_compile_definitions(easypbr_cpp PRIVATE DATA_DIR="${DATA_DIR}")
# target_compile_definitions(easypbr_cpp PRIVATE EASYPBR_SHADERS_PATH="${EASYPBR_SHADERS_PATH}")

# target_compile_definitions(easypbr_cpp PRIVATE EASYPBR_DATA_DIR="${PROJECT_SOURCE_DIR}/data")
# target_compile_definitions(easypbr_cpp PRIVATE EASYPBR_SHADERS_PATH="${PROJECT_SOURCE_DIR}/shaders")
# # target_compile_definitions(easypbr_cpp PRIVATE DEFAULT_CONFIG="${PROJECT_SOURCE_DIR}/config/default_params.cfg")
# target_compile_definitions(easypbr_cpp PRIVATE PROJECT_SOURCE_DIR="${PROJECT_SOURCE_DIR}") #point to the cmakelist folder of the easy_pbr
# add_definitions( -DDEFAULT_CONFIG="${PROJECT_SOURCE_DIR}/config/default_params.cfg" )

# target_compile_definitions(easypbr_cpp PRIVATE -DTRILIBRARY -DANSI_DECLARATORS)


#install the libraries in a default search path
# message("INSTALLING IN " ${CMAKE_INSTALL_LIBDIR})
# install(TARGETS easypbr DESTINATION ${CMAKE_INSTALL_LIBDIR})
# install(TARGETS easypbr_cpp DESTINATION ${CMAKE_INSTALL_LIBDIR})
# install(TARGETS run_easypbr DESTINATION bin)


# install(TARGETS easypbr
#     LIBRARY DESTINATION lib
# )
# install(TARGETS easypbr_cpp
#     LIBRARY DESTINATION lib
# )
# install(DIRECTORY
#     include/easy_pbr/
#     DESTINATION include/easy_pbr
# )
