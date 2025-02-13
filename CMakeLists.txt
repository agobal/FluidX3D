cmake_minimum_required(VERSION 3.11)
set(PROJECT_NAME fluidx3d)

add_library(fluidx3d STATIC)

find_package(OpenCL REQUIRED)

################################################################################
# Source groups
################################################################################
set(Headers
    "src/defines.hpp"
    "src/graphics.hpp"
    "src/info.hpp"
    "src/kernel.hpp"
    "src/lbm.hpp"
    "src/lodepng.hpp"
    "src/opencl.hpp"
    "src/setup.hpp"
    "src/shapes.hpp"
    "src/units.hpp"
    "src/utilities.hpp"
)
source_group("Headers" FILES ${Headers})

set(Sources
    "src/graphics.cpp"
    "src/info.cpp"
    "src/kernel.cpp"
    "src/lbm.cpp"
    "src/lodepng.cpp"
    "src/setup.cpp"
    "src/shapes.cpp"
)
source_group("Sources" FILES ${Sources})

set(ALL_FILES
    ${Headers}
    ${Sources}
)

target_sources(fluidx3d PRIVATE ${ALL_FILES})

################################################################################
# Target
################################################################################
#add_executable(${PROJECT_NAME} ${ALL_FILES})

#use_props(${PROJECT_NAME} "${CMAKE_CONFIGURATION_TYPES}" "${DEFAULT_CXX_PROPS}")
#set(ROOT_NAMESPACE fluidx3d)

################################################################################
# Output directory
################################################################################
#set_target_properties(${PROJECT_NAME} PROPERTIES
#    OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}/bin/"
#)
#set_target_properties(${PROJECT_NAME} PROPERTIES
#    INTERPROCEDURAL_OPTIMIZATION         "TRUE"
#)
################################################################################
# Include directories
################################################################################
target_include_directories(${PROJECT_NAME} PRIVATE
    ${OpenCL_INCLUDE_DIRS}
#"${CMAKE_CURRENT_SOURCE_DIR}/src/OpenCL/include"    
)

target_include_directories(${PROJECT_NAME} PUBLIC
    .
    src
)

################################################################################
# Compile definitions
################################################################################
target_compile_definitions(${PROJECT_NAME} PRIVATE
    CL_TARGET_OPENCL_VERSION=120
    "_MBCS"
)

################################################################################
# Compile and link options
################################################################################
if(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE
        /permissive-;
        /MP;
        /std:c++17;
        /O2;
        /Oi;
        /sdl;
        /Gy;
        /W3;
        ${DEFAULT_CXX_DEBUG_INFORMATION_FORMAT};
        /Ot;
        /fp:fast;
        /wd26451;
        /wd6386;
        ${DEFAULT_CXX_EXCEPTION_HANDLING}
    )
    target_link_options(${PROJECT_NAME} PRIVATE
        /DEBUG:FULL;
        /SUBSYSTEM:CONSOLE;
        /OPT:REF;
        /OPT:ICF
    )
endif()

################################################################################
# Dependencies
################################################################################
set(ADDITIONAL_LIBRARY_DEPENDENCIES
    "kernel32;"
    "user32;"
    "gdi32;"
    "winspool;"
    "comdlg32;"
    "advapi32;"
    "shell32;"
    "ole32;"
    "oleaut32;"
    "uuid;"
    "odbc32;"
    "odbccp32"
)
target_link_libraries(${PROJECT_NAME} PRIVATE "${ADDITIONAL_LIBRARY_DEPENDENCIES}" ${OpenCL_LIBRARIES})
# 

#target_link_directories(${PROJECT_NAME} PUBLIC
#    "${CMAKE_SOURCE_DIR}/src/OpenCL/lib"
#)

