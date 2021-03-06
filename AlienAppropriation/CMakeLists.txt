cmake_minimum_required(VERSION 2.6)

# Name of project
set(PROJ_NAME AlienAppropiation)
project(${PROJ_NAME})

# Specify project files: header files and source files
set(HDRS
    base_node.h
    camera.h
    entity_game_nodes.h
    entity_node.h
    game.h
    map_generator.h
    model_loader.h
    player_node.h
    PoissonGenerator.h
    projectile_node.h
    resource.h
    resource_manager.h
    scene_graph.h
    scene_node.h
    ui_node.h
)
 
set(SRCS
    base_node.cpp
    camera.cpp
    entity_game_nodes.cpp
    entity_node.cpp
    game.cpp
    main.cpp
    map_generator.cpp
    player_node.cpp
    projectile_node.cpp
    resource.cpp
    resource_manager.cpp
    scene_graph.cpp
    scene_node.cpp
    shaders/default_fp.glsl
    shaders/default_vp.glsl
    shaders/litTexture_fp.glsl
    shaders/litTexture_vp.glsl
    shaders/particleBeam_fp.glsl
    shaders/particleBeam_gp.glsl
    shaders/particleBeam_vp.glsl
    shaders/particleShield_fp.glsl
    shaders/particleShield_gp.glsl
    shaders/particleShield_vp.glsl
    shaders/skybox_fp.glsl
    shaders/skybox_vp.glsl
    shaders/textured_fp.glsl
    shaders/textured_vp.glsl
    shaders/three-term_shiny_blue_fp.glsl
    shaders/three-term_shiny_blue_vp.glsl
    ui_node.cpp
)

# Add path name to configuration file
configure_file(path_config.h.in path_config.h)

# Add executable based on the source files
add_executable(${PROJ_NAME} ${HDRS} ${SRCS})

# Require OpenGL library
find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIR})
target_link_libraries(${PROJ_NAME} ${OPENGL_gl_LIBRARY})

# Other libraries needed
set(LIBRARY_PATH "" CACHE PATH "Folder with GLEW, GLFW, GLM, and SOIL libraries")
include_directories(${LIBRARY_PATH}/include)
if(NOT WIN32)
    find_library(GLEW_LIBRARY GLEW)
    find_library(GLFW_LIBRARY glfw)
    find_library(SOIL_LIBRARY SOIL)
elseif(WIN32)
    find_library(GLEW_LIBRARY glew32s HINTS ${LIBRARY_PATH}/lib)
    find_library(GLFW_LIBRARY glfw3 HINTS ${LIBRARY_PATH}/lib)
    find_library(SOIL_LIBRARY SOIL HINTS ${LIBRARY_PATH}/lib)
endif(NOT WIN32)
target_link_libraries(${PROJ_NAME} ${GLEW_LIBRARY})
target_link_libraries(${PROJ_NAME} ${GLFW_LIBRARY})
target_link_libraries(${PROJ_NAME} ${SOIL_LIBRARY})

# The rules here are specific to Windows Systems
if(WIN32)
    # Avoid ZERO_CHECK target in Visual Studio
    set(CMAKE_SUPPRESS_REGENERATION TRUE)
 
    # This will use the proper libraries in debug mode in Visual Studio
    set_target_properties(${PROJ_NAME} PROPERTIES DEBUG_POSTFIX _d)
endif(WIN32)
