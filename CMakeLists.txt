cmake_minimum_required(VERSION 3.16)
project(Pokemon_Pinball__U__ C CXX)

include(CheckIPOSupported)

# Set C standard
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(GBRECOMP_ENABLE_PERFORMANCE_COUNTERS "Compile generated/runtime performance counters" OFF)

set(GBRECOMP_GENERATED_COMPILE_JOBS "8" CACHE STRING "Maximum concurrent generated-source compiles under Ninja; 0 disables the pool")

# Migrate stale generated-project cache entries when the default profile changes.
set(GBRECOMP_PROFILE_CACHE_VERSION "2")
set(GBRECOMP_APPLIED_PROFILE_CACHE_VERSION "0" CACHE STRING "Internal generated-project profile version")
if(NOT GBRECOMP_APPLIED_PROFILE_CACHE_VERSION STREQUAL GBRECOMP_PROFILE_CACHE_VERSION)
    if(NOT DEFINED GBRECOMP_ENABLE_IPO)
        set(GBRECOMP_ENABLE_IPO OFF CACHE BOOL "Enable interprocedural optimization/LTO for non-Debug builds" FORCE)
    endif()
    if(NOT DEFINED GBRECOMP_ENABLE_STRIP)
        set(GBRECOMP_ENABLE_STRIP ON CACHE BOOL "Strip symbols from generated executables in non-Debug builds" FORCE)
    endif()
    if(NOT DEFINED GBRECOMP_GENERATED_OPT_LEVEL)
        set(GBRECOMP_GENERATED_OPT_LEVEL "1" CACHE STRING "Optimization level for the generated ROM source files" FORCE)
    endif()
    set(GBRECOMP_APPLIED_PROFILE_CACHE_VERSION "${GBRECOMP_PROFILE_CACHE_VERSION}" CACHE STRING "Internal generated-project profile version" FORCE)
endif()

# Default to a smaller, iteration-friendly profile for generated projects.
# Benchmark and release builds can still opt into Release/-O3/IPO explicitly.
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE MinSizeRel CACHE STRING "Build type" FORCE)
endif()
if(CMAKE_C_COMPILER_ID MATCHES "GNU|Clang")
    if(CMAKE_BUILD_TYPE STREQUAL "Debug")
        add_compile_options(-O0 -g)
    elseif(CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
        add_compile_options(-O2 -g)
    elseif(CMAKE_BUILD_TYPE STREQUAL "MinSizeRel")
        add_compile_options(-Os)
    else()
        add_compile_options(-O3)
    endif()
    add_compile_options(-ffunction-sections -fdata-sections)
endif()

# Self-contained runtime snapshot embedded by gbrecomp
set(GBRT_DIR "${CMAKE_CURRENT_SOURCE_DIR}/runtime")

# Manual SDL2 setup to avoid mingw64 header conflicts with retcomm toolchain
set(SDL2_INCLUDE_DIR "C:/msys64/mingw64/include/SDL2")
set(SDL2_LIBRARY "C:/msys64/mingw64/lib/libSDL2.dll.a")
set(SDL2MAIN_LIBRARY "C:/msys64/mingw64/lib/libSDL2main.a")
set(SDL2_FOUND TRUE)

# Create runtime library with PPU and platform support
add_library(gbrt STATIC
    ${GBRT_DIR}/src/gbrt.c
    ${GBRT_DIR}/src/gbrt_data_mod.c
    ${GBRT_DIR}/src/gbrt_hash.c
    ${GBRT_DIR}/src/gbrt_host_configuration.c
    ${GBRT_DIR}/src/gbrt_port.c
    ${GBRT_DIR}/src/gbrt_presentation.c
    ${GBRT_DIR}/src/gbrt_semantic.c
    ${GBRT_DIR}/src/differential.c
    ${GBRT_DIR}/src/ppu.c
    ${GBRT_DIR}/src/audio.c
    ${GBRT_DIR}/src/audio_stats.c
    ${GBRT_DIR}/src/interpreter.c
    ${GBRT_DIR}/src/platform_sdl.cpp
)

# Vendored Dear ImGui subset
target_sources(gbrt PRIVATE
    ${GBRT_DIR}/vendor/imgui/imgui.cpp
    ${GBRT_DIR}/vendor/imgui/imgui_draw.cpp
    ${GBRT_DIR}/vendor/imgui/imgui_tables.cpp
    ${GBRT_DIR}/vendor/imgui/imgui_widgets.cpp
    ${GBRT_DIR}/vendor/imgui/backends/imgui_impl_sdl2.cpp
    ${GBRT_DIR}/vendor/imgui/backends/imgui_impl_sdlrenderer2.cpp
)
target_include_directories(gbrt PUBLIC 
    ${GBRT_DIR}/include
    ${GBRT_DIR}/vendor/imgui
    ${SDL2_INCLUDE_DIR}
)
target_link_libraries(gbrt PUBLIC ${SDL2_LIBRARY})
target_compile_definitions(gbrt PUBLIC GB_HAS_SDL2)

if(GBRECOMP_ENABLE_PERFORMANCE_COUNTERS)
    target_compile_definitions(gbrt PUBLIC GBRT_ENABLE_PERFORMANCE_COUNTERS)
endif()

# Main executable
add_executable(Pokemon_Pinball__U__
    Pokemon_Pinball__U___main.c
    Pokemon_Pinball__U__.c
    Pokemon_Pinball__U___dispatch_chunk_0.c
    Pokemon_Pinball__U___dispatch_chunk_1.c
    Pokemon_Pinball__U___dispatch_chunk_2.c
    Pokemon_Pinball__U___dispatch_chunk_3.c
    Pokemon_Pinball__U___dispatch_chunk_4.c
    Pokemon_Pinball__U___dispatch_chunk_5.c
    Pokemon_Pinball__U___dispatch_chunk_6.c
    Pokemon_Pinball__U___dispatch_chunk_7.c
    Pokemon_Pinball__U___dispatch_chunk_8.c
    Pokemon_Pinball__U___dispatch_chunk_9.c
    Pokemon_Pinball__U___dispatch_chunk_10.c
    Pokemon_Pinball__U___dispatch_chunk_11.c
    Pokemon_Pinball__U___dispatch_chunk_12.c
    Pokemon_Pinball__U___dispatch_chunk_13.c
    Pokemon_Pinball__U___dispatch_chunk_14.c
    Pokemon_Pinball__U___dispatch_chunk_15.c
    Pokemon_Pinball__U___dispatch_chunk_16.c
    Pokemon_Pinball__U___dispatch_chunk_17.c
    Pokemon_Pinball__U___dispatch_chunk_18.c
    Pokemon_Pinball__U___dispatch_chunk_19.c
    Pokemon_Pinball__U___dispatch_chunk_20.c
    Pokemon_Pinball__U___dispatch_chunk_21.c
    Pokemon_Pinball__U___dispatch_chunk_22.c
    Pokemon_Pinball__U___dispatch_chunk_23.c
    Pokemon_Pinball__U___dispatch_chunk_24.c
    Pokemon_Pinball__U___dispatch_chunk_25.c
    Pokemon_Pinball__U___dispatch_chunk_26.c
    Pokemon_Pinball__U___dispatch_chunk_27.c
    Pokemon_Pinball__U___dispatch_chunk_28.c
    Pokemon_Pinball__U___dispatch_chunk_29.c
    Pokemon_Pinball__U___dispatch_chunk_30.c
    Pokemon_Pinball__U___dispatch_chunk_31.c
    Pokemon_Pinball__U___dispatch_chunk_32.c
    Pokemon_Pinball__U___dispatch_chunk_33.c
    Pokemon_Pinball__U___dispatch_chunk_34.c
    Pokemon_Pinball__U___dispatch_chunk_35.c
    Pokemon_Pinball__U___dispatch_chunk_36.c
    Pokemon_Pinball__U___dispatch_chunk_37.c
    Pokemon_Pinball__U___dispatch_chunk_38.c
    Pokemon_Pinball__U___dispatch_chunk_39.c
    Pokemon_Pinball__U___dispatch_chunk_40.c
    Pokemon_Pinball__U___dispatch_chunk_41.c
    Pokemon_Pinball__U___funcs_0.c
    Pokemon_Pinball__U___funcs_1.c
    Pokemon_Pinball__U___funcs_2.c
    Pokemon_Pinball__U___funcs_3.c
    Pokemon_Pinball__U___funcs_4.c
    Pokemon_Pinball__U___funcs_5.c
    Pokemon_Pinball__U___funcs_6.c
    Pokemon_Pinball__U___funcs_7.c
    Pokemon_Pinball__U___funcs_8.c
    Pokemon_Pinball__U___funcs_9.c
    Pokemon_Pinball__U___funcs_10.c
    Pokemon_Pinball__U___funcs_11.c
    Pokemon_Pinball__U___funcs_12.c
    Pokemon_Pinball__U___funcs_13.c
    Pokemon_Pinball__U___funcs_14.c
    Pokemon_Pinball__U___funcs_15.c
    Pokemon_Pinball__U___funcs_16.c
    Pokemon_Pinball__U___funcs_17.c
    Pokemon_Pinball__U___funcs_18.c
    Pokemon_Pinball__U___funcs_19.c
    Pokemon_Pinball__U___funcs_20.c
    Pokemon_Pinball__U___funcs_21.c
    Pokemon_Pinball__U___funcs_22.c
    Pokemon_Pinball__U___funcs_23.c
    Pokemon_Pinball__U___funcs_24.c
    Pokemon_Pinball__U___funcs_25.c
    Pokemon_Pinball__U___funcs_26.c
    Pokemon_Pinball__U___funcs_27.c
    Pokemon_Pinball__U___funcs_28.c
    Pokemon_Pinball__U___funcs_29.c
    Pokemon_Pinball__U___funcs_30.c
    Pokemon_Pinball__U___funcs_31.c
    Pokemon_Pinball__U___funcs_32.c
    Pokemon_Pinball__U___funcs_33.c
    Pokemon_Pinball__U___funcs_34.c
    Pokemon_Pinball__U___funcs_35.c
    Pokemon_Pinball__U___funcs_36.c
    Pokemon_Pinball__U___funcs_37.c
    Pokemon_Pinball__U___funcs_38.c
    Pokemon_Pinball__U___funcs_39.c
    Pokemon_Pinball__U___funcs_40.c
    Pokemon_Pinball__U___funcs_41.c
    Pokemon_Pinball__U___funcs_42.c
    Pokemon_Pinball__U___funcs_43.c
    Pokemon_Pinball__U___funcs_44.c
    Pokemon_Pinball__U___funcs_45.c
    Pokemon_Pinball__U___funcs_46.c
    Pokemon_Pinball__U___funcs_47.c
    Pokemon_Pinball__U___funcs_48.c
    Pokemon_Pinball__U___funcs_49.c
    Pokemon_Pinball__U___funcs_50.c
    Pokemon_Pinball__U___funcs_51.c
    Pokemon_Pinball__U___funcs_52.c
    Pokemon_Pinball__U___funcs_53.c
    Pokemon_Pinball__U___funcs_54.c
    Pokemon_Pinball__U___funcs_55.c
    Pokemon_Pinball__U___funcs_56.c
    Pokemon_Pinball__U___funcs_57.c
    Pokemon_Pinball__U___funcs_58.c
    Pokemon_Pinball__U___funcs_59.c
    Pokemon_Pinball__U___funcs_60.c
    Pokemon_Pinball__U___funcs_61.c
    Pokemon_Pinball__U___funcs_62.c
    Pokemon_Pinball__U___funcs_63.c
    Pokemon_Pinball__U___funcs_64.c
    Pokemon_Pinball__U___funcs_65.c
    Pokemon_Pinball__U___funcs_66.c
    Pokemon_Pinball__U___funcs_67.c
    Pokemon_Pinball__U___funcs_68.c
    Pokemon_Pinball__U___funcs_69.c
    Pokemon_Pinball__U___funcs_70.c
    Pokemon_Pinball__U___funcs_71.c
    Pokemon_Pinball__U___funcs_72.c
    Pokemon_Pinball__U___funcs_73.c
    Pokemon_Pinball__U___funcs_74.c
    Pokemon_Pinball__U___funcs_75.c
    Pokemon_Pinball__U___funcs_76.c
    Pokemon_Pinball__U___funcs_77.c
    Pokemon_Pinball__U___funcs_78.c
    Pokemon_Pinball__U___funcs_79.c
    Pokemon_Pinball__U___funcs_80.c
    Pokemon_Pinball__U___funcs_81.c
    Pokemon_Pinball__U___funcs_82.c
    Pokemon_Pinball__U___funcs_83.c
    Pokemon_Pinball__U___funcs_84.c
    Pokemon_Pinball__U___funcs_85.c
    Pokemon_Pinball__U___funcs_86.c
    Pokemon_Pinball__U___funcs_87.c
    Pokemon_Pinball__U___funcs_88.c
    Pokemon_Pinball__U___funcs_89.c
    Pokemon_Pinball__U___funcs_90.c
    Pokemon_Pinball__U___funcs_91.c
    Pokemon_Pinball__U___funcs_92.c
    Pokemon_Pinball__U___funcs_93.c
    Pokemon_Pinball__U___funcs_94.c
    Pokemon_Pinball__U___funcs_95.c
    Pokemon_Pinball__U___funcs_96.c
    Pokemon_Pinball__U___funcs_97.c
    Pokemon_Pinball__U___funcs_98.c
    Pokemon_Pinball__U___funcs_99.c
    Pokemon_Pinball__U___funcs_100.c
    Pokemon_Pinball__U___funcs_101.c
    Pokemon_Pinball__U___funcs_102.c
    Pokemon_Pinball__U___funcs_103.c
    Pokemon_Pinball__U___funcs_104.c
    Pokemon_Pinball__U___funcs_105.c
    Pokemon_Pinball__U___funcs_106.c
    Pokemon_Pinball__U___funcs_107.c
    Pokemon_Pinball__U___funcs_108.c
    Pokemon_Pinball__U___funcs_109.c
    Pokemon_Pinball__U___funcs_110.c
    Pokemon_Pinball__U___funcs_111.c
    Pokemon_Pinball__U___funcs_112.c
    Pokemon_Pinball__U___funcs_113.c
    Pokemon_Pinball__U___funcs_114.c
    Pokemon_Pinball__U___funcs_115.c
    Pokemon_Pinball__U___funcs_116.c
    Pokemon_Pinball__U___funcs_117.c
    Pokemon_Pinball__U___funcs_118.c
    Pokemon_Pinball__U___funcs_119.c
    Pokemon_Pinball__U___funcs_120.c
    Pokemon_Pinball__U___funcs_121.c
    Pokemon_Pinball__U___funcs_122.c
    Pokemon_Pinball__U___funcs_123.c
    Pokemon_Pinball__U___funcs_124.c
    Pokemon_Pinball__U___funcs_125.c
    Pokemon_Pinball__U___funcs_126.c
    Pokemon_Pinball__U___funcs_127.c
    Pokemon_Pinball__U___funcs_128.c
    Pokemon_Pinball__U___funcs_129.c
    Pokemon_Pinball__U___funcs_130.c
    Pokemon_Pinball__U___funcs_131.c
    Pokemon_Pinball__U___funcs_132.c
    Pokemon_Pinball__U___funcs_133.c
    Pokemon_Pinball__U___funcs_134.c
    Pokemon_Pinball__U___funcs_135.c
    Pokemon_Pinball__U___funcs_136.c
    Pokemon_Pinball__U___funcs_137.c
    Pokemon_Pinball__U___funcs_138.c
    Pokemon_Pinball__U___funcs_139.c
    Pokemon_Pinball__U___funcs_140.c
    Pokemon_Pinball__U___funcs_141.c
    Pokemon_Pinball__U___funcs_142.c
    Pokemon_Pinball__U___funcs_143.c
    Pokemon_Pinball__U___funcs_144.c
    Pokemon_Pinball__U___rom.c
)

# Large generated translation units are memory-intensive. Keep runtime/UI
# compilation parallel while bounding only the generated executable target.
if(CMAKE_GENERATOR MATCHES "Ninja" AND GBRECOMP_GENERATED_COMPILE_JOBS MATCHES "^[1-9][0-9]*$")
    set_property(GLOBAL APPEND PROPERTY JOB_POOLS "gbrecomp_generated_compile_pool=${GBRECOMP_GENERATED_COMPILE_JOBS}")
    set_property(TARGET Pokemon_Pinball__U__ PROPERTY JOB_POOL_COMPILE gbrecomp_generated_compile_pool)
endif()

# Generated ROM code defaults to a smaller manual-testing profile.
# Raise the optimization level or enable IPO/LTO explicitly for benchmark/release builds.
set(GBRECOMP_GENERATED_OPT_LEVEL "1" CACHE STRING "Optimization level for the generated ROM source files")
set_property(CACHE GBRECOMP_GENERATED_OPT_LEVEL PROPERTY STRINGS 0 1 2 3)
option(GBRECOMP_ENABLE_IPO "Enable interprocedural optimization/LTO for non-Debug builds" OFF)
option(GBRECOMP_ENABLE_STRIP "Strip symbols from generated executables in non-Debug builds" ON)
set(GBRECOMP_GENERATED_SOURCES
    Pokemon_Pinball__U__.c
    Pokemon_Pinball__U___dispatch_chunk_0.c
    Pokemon_Pinball__U___dispatch_chunk_1.c
    Pokemon_Pinball__U___dispatch_chunk_2.c
    Pokemon_Pinball__U___dispatch_chunk_3.c
    Pokemon_Pinball__U___dispatch_chunk_4.c
    Pokemon_Pinball__U___dispatch_chunk_5.c
    Pokemon_Pinball__U___dispatch_chunk_6.c
    Pokemon_Pinball__U___dispatch_chunk_7.c
    Pokemon_Pinball__U___dispatch_chunk_8.c
    Pokemon_Pinball__U___dispatch_chunk_9.c
    Pokemon_Pinball__U___dispatch_chunk_10.c
    Pokemon_Pinball__U___dispatch_chunk_11.c
    Pokemon_Pinball__U___dispatch_chunk_12.c
    Pokemon_Pinball__U___dispatch_chunk_13.c
    Pokemon_Pinball__U___dispatch_chunk_14.c
    Pokemon_Pinball__U___dispatch_chunk_15.c
    Pokemon_Pinball__U___dispatch_chunk_16.c
    Pokemon_Pinball__U___dispatch_chunk_17.c
    Pokemon_Pinball__U___dispatch_chunk_18.c
    Pokemon_Pinball__U___dispatch_chunk_19.c
    Pokemon_Pinball__U___dispatch_chunk_20.c
    Pokemon_Pinball__U___dispatch_chunk_21.c
    Pokemon_Pinball__U___dispatch_chunk_22.c
    Pokemon_Pinball__U___dispatch_chunk_23.c
    Pokemon_Pinball__U___dispatch_chunk_24.c
    Pokemon_Pinball__U___dispatch_chunk_25.c
    Pokemon_Pinball__U___dispatch_chunk_26.c
    Pokemon_Pinball__U___dispatch_chunk_27.c
    Pokemon_Pinball__U___dispatch_chunk_28.c
    Pokemon_Pinball__U___dispatch_chunk_29.c
    Pokemon_Pinball__U___dispatch_chunk_30.c
    Pokemon_Pinball__U___dispatch_chunk_31.c
    Pokemon_Pinball__U___dispatch_chunk_32.c
    Pokemon_Pinball__U___dispatch_chunk_33.c
    Pokemon_Pinball__U___dispatch_chunk_34.c
    Pokemon_Pinball__U___dispatch_chunk_35.c
    Pokemon_Pinball__U___dispatch_chunk_36.c
    Pokemon_Pinball__U___dispatch_chunk_37.c
    Pokemon_Pinball__U___dispatch_chunk_38.c
    Pokemon_Pinball__U___dispatch_chunk_39.c
    Pokemon_Pinball__U___dispatch_chunk_40.c
    Pokemon_Pinball__U___dispatch_chunk_41.c
    Pokemon_Pinball__U___funcs_0.c
    Pokemon_Pinball__U___funcs_1.c
    Pokemon_Pinball__U___funcs_2.c
    Pokemon_Pinball__U___funcs_3.c
    Pokemon_Pinball__U___funcs_4.c
    Pokemon_Pinball__U___funcs_5.c
    Pokemon_Pinball__U___funcs_6.c
    Pokemon_Pinball__U___funcs_7.c
    Pokemon_Pinball__U___funcs_8.c
    Pokemon_Pinball__U___funcs_9.c
    Pokemon_Pinball__U___funcs_10.c
    Pokemon_Pinball__U___funcs_11.c
    Pokemon_Pinball__U___funcs_12.c
    Pokemon_Pinball__U___funcs_13.c
    Pokemon_Pinball__U___funcs_14.c
    Pokemon_Pinball__U___funcs_15.c
    Pokemon_Pinball__U___funcs_16.c
    Pokemon_Pinball__U___funcs_17.c
    Pokemon_Pinball__U___funcs_18.c
    Pokemon_Pinball__U___funcs_19.c
    Pokemon_Pinball__U___funcs_20.c
    Pokemon_Pinball__U___funcs_21.c
    Pokemon_Pinball__U___funcs_22.c
    Pokemon_Pinball__U___funcs_23.c
    Pokemon_Pinball__U___funcs_24.c
    Pokemon_Pinball__U___funcs_25.c
    Pokemon_Pinball__U___funcs_26.c
    Pokemon_Pinball__U___funcs_27.c
    Pokemon_Pinball__U___funcs_28.c
    Pokemon_Pinball__U___funcs_29.c
    Pokemon_Pinball__U___funcs_30.c
    Pokemon_Pinball__U___funcs_31.c
    Pokemon_Pinball__U___funcs_32.c
    Pokemon_Pinball__U___funcs_33.c
    Pokemon_Pinball__U___funcs_34.c
    Pokemon_Pinball__U___funcs_35.c
    Pokemon_Pinball__U___funcs_36.c
    Pokemon_Pinball__U___funcs_37.c
    Pokemon_Pinball__U___funcs_38.c
    Pokemon_Pinball__U___funcs_39.c
    Pokemon_Pinball__U___funcs_40.c
    Pokemon_Pinball__U___funcs_41.c
    Pokemon_Pinball__U___funcs_42.c
    Pokemon_Pinball__U___funcs_43.c
    Pokemon_Pinball__U___funcs_44.c
    Pokemon_Pinball__U___funcs_45.c
    Pokemon_Pinball__U___funcs_46.c
    Pokemon_Pinball__U___funcs_47.c
    Pokemon_Pinball__U___funcs_48.c
    Pokemon_Pinball__U___funcs_49.c
    Pokemon_Pinball__U___funcs_50.c
    Pokemon_Pinball__U___funcs_51.c
    Pokemon_Pinball__U___funcs_52.c
    Pokemon_Pinball__U___funcs_53.c
    Pokemon_Pinball__U___funcs_54.c
    Pokemon_Pinball__U___funcs_55.c
    Pokemon_Pinball__U___funcs_56.c
    Pokemon_Pinball__U___funcs_57.c
    Pokemon_Pinball__U___funcs_58.c
    Pokemon_Pinball__U___funcs_59.c
    Pokemon_Pinball__U___funcs_60.c
    Pokemon_Pinball__U___funcs_61.c
    Pokemon_Pinball__U___funcs_62.c
    Pokemon_Pinball__U___funcs_63.c
    Pokemon_Pinball__U___funcs_64.c
    Pokemon_Pinball__U___funcs_65.c
    Pokemon_Pinball__U___funcs_66.c
    Pokemon_Pinball__U___funcs_67.c
    Pokemon_Pinball__U___funcs_68.c
    Pokemon_Pinball__U___funcs_69.c
    Pokemon_Pinball__U___funcs_70.c
    Pokemon_Pinball__U___funcs_71.c
    Pokemon_Pinball__U___funcs_72.c
    Pokemon_Pinball__U___funcs_73.c
    Pokemon_Pinball__U___funcs_74.c
    Pokemon_Pinball__U___funcs_75.c
    Pokemon_Pinball__U___funcs_76.c
    Pokemon_Pinball__U___funcs_77.c
    Pokemon_Pinball__U___funcs_78.c
    Pokemon_Pinball__U___funcs_79.c
    Pokemon_Pinball__U___funcs_80.c
    Pokemon_Pinball__U___funcs_81.c
    Pokemon_Pinball__U___funcs_82.c
    Pokemon_Pinball__U___funcs_83.c
    Pokemon_Pinball__U___funcs_84.c
    Pokemon_Pinball__U___funcs_85.c
    Pokemon_Pinball__U___funcs_86.c
    Pokemon_Pinball__U___funcs_87.c
    Pokemon_Pinball__U___funcs_88.c
    Pokemon_Pinball__U___funcs_89.c
    Pokemon_Pinball__U___funcs_90.c
    Pokemon_Pinball__U___funcs_91.c
    Pokemon_Pinball__U___funcs_92.c
    Pokemon_Pinball__U___funcs_93.c
    Pokemon_Pinball__U___funcs_94.c
    Pokemon_Pinball__U___funcs_95.c
    Pokemon_Pinball__U___funcs_96.c
    Pokemon_Pinball__U___funcs_97.c
    Pokemon_Pinball__U___funcs_98.c
    Pokemon_Pinball__U___funcs_99.c
    Pokemon_Pinball__U___funcs_100.c
    Pokemon_Pinball__U___funcs_101.c
    Pokemon_Pinball__U___funcs_102.c
    Pokemon_Pinball__U___funcs_103.c
    Pokemon_Pinball__U___funcs_104.c
    Pokemon_Pinball__U___funcs_105.c
    Pokemon_Pinball__U___funcs_106.c
    Pokemon_Pinball__U___funcs_107.c
    Pokemon_Pinball__U___funcs_108.c
    Pokemon_Pinball__U___funcs_109.c
    Pokemon_Pinball__U___funcs_110.c
    Pokemon_Pinball__U___funcs_111.c
    Pokemon_Pinball__U___funcs_112.c
    Pokemon_Pinball__U___funcs_113.c
    Pokemon_Pinball__U___funcs_114.c
    Pokemon_Pinball__U___funcs_115.c
    Pokemon_Pinball__U___funcs_116.c
    Pokemon_Pinball__U___funcs_117.c
    Pokemon_Pinball__U___funcs_118.c
    Pokemon_Pinball__U___funcs_119.c
    Pokemon_Pinball__U___funcs_120.c
    Pokemon_Pinball__U___funcs_121.c
    Pokemon_Pinball__U___funcs_122.c
    Pokemon_Pinball__U___funcs_123.c
    Pokemon_Pinball__U___funcs_124.c
    Pokemon_Pinball__U___funcs_125.c
    Pokemon_Pinball__U___funcs_126.c
    Pokemon_Pinball__U___funcs_127.c
    Pokemon_Pinball__U___funcs_128.c
    Pokemon_Pinball__U___funcs_129.c
    Pokemon_Pinball__U___funcs_130.c
    Pokemon_Pinball__U___funcs_131.c
    Pokemon_Pinball__U___funcs_132.c
    Pokemon_Pinball__U___funcs_133.c
    Pokemon_Pinball__U___funcs_134.c
    Pokemon_Pinball__U___funcs_135.c
    Pokemon_Pinball__U___funcs_136.c
    Pokemon_Pinball__U___funcs_137.c
    Pokemon_Pinball__U___funcs_138.c
    Pokemon_Pinball__U___funcs_139.c
    Pokemon_Pinball__U___funcs_140.c
    Pokemon_Pinball__U___funcs_141.c
    Pokemon_Pinball__U___funcs_142.c
    Pokemon_Pinball__U___funcs_143.c
    Pokemon_Pinball__U___funcs_144.c
)
if(NOT CMAKE_BUILD_TYPE STREQUAL "Debug")
    if(MSVC)
        if(GBRECOMP_GENERATED_OPT_LEVEL STREQUAL "0")
            set(GBRECOMP_GENERATED_OPT_FLAG "/Od")
        elseif(GBRECOMP_GENERATED_OPT_LEVEL STREQUAL "1")
            set(GBRECOMP_GENERATED_OPT_FLAG "/O1")
        else()
            set(GBRECOMP_GENERATED_OPT_FLAG "/O2")
        endif()
        set_source_files_properties(${GBRECOMP_GENERATED_SOURCES} PROPERTIES COMPILE_OPTIONS "${GBRECOMP_GENERATED_OPT_FLAG}")
    else()
        set_source_files_properties(${GBRECOMP_GENERATED_SOURCES} PROPERTIES COMPILE_OPTIONS "-O${GBRECOMP_GENERATED_OPT_LEVEL}")
    endif()
endif()

if(NOT MSVC AND NOT CMAKE_BUILD_TYPE STREQUAL "Debug")
    if(APPLE)
        target_link_options(gbrt PRIVATE -Wl,-dead_strip)
        target_link_options(Pokemon_Pinball__U__ PRIVATE -Wl,-dead_strip)
    else()
        target_link_options(gbrt PRIVATE -Wl,--gc-sections)
        target_link_options(Pokemon_Pinball__U__ PRIVATE -Wl,--gc-sections)
    endif()
endif()

if(GBRECOMP_ENABLE_IPO AND NOT CMAKE_BUILD_TYPE STREQUAL "Debug")
    check_ipo_supported(RESULT GBRECOMP_IPO_SUPPORTED OUTPUT GBRECOMP_IPO_ERROR)
    if(GBRECOMP_IPO_SUPPORTED)
        set_property(TARGET gbrt PROPERTY INTERPROCEDURAL_OPTIMIZATION TRUE)
        set_property(TARGET Pokemon_Pinball__U__ PROPERTY INTERPROCEDURAL_OPTIMIZATION TRUE)
    else()
        message(STATUS "GB Recompiled: IPO/LTO not enabled (${GBRECOMP_IPO_ERROR})")
    endif()
endif()

target_link_libraries(Pokemon_Pinball__U__ gbrt ${SDL2MAIN_LIBRARY})
if(GBRECOMP_ENABLE_STRIP AND NOT MSVC AND NOT CMAKE_BUILD_TYPE STREQUAL "Debug")
    if(APPLE)
        add_custom_command(TARGET Pokemon_Pinball__U__ POST_BUILD
            COMMAND ${CMAKE_STRIP} -Sx $<TARGET_FILE:Pokemon_Pinball__U__>
            VERBATIM)
    elseif(CMAKE_STRIP)
        add_custom_command(TARGET Pokemon_Pinball__U__ POST_BUILD
            COMMAND ${CMAKE_STRIP} --strip-debug $<TARGET_FILE:Pokemon_Pinball__U__>
            VERBATIM)
    endif()
endif()
