cmake_minimum_required(VERSION 3.14)
if(POLICY CMP0153)
    cmake_policy(SET CMP0153 NEW)
endif()
set(PROJECT_NAME "flutter_tts")
project(${PROJECT_NAME} LANGUAGES CXX)

# This value is used when generating builds using this plugin, so it must
# not be changed
set(PLUGIN_NAME "flutter_tts_plugin")

add_library(${PLUGIN_NAME} SHARED
        "flutter_tts_plugin.cpp"
)

apply_standard_settings(${PLUGIN_NAME})
set_target_properties(${PLUGIN_NAME} PROPERTIES
        CXX_VISIBILITY_PRESET hidden
)
target_compile_features(${PLUGIN_NAME} PUBLIC cxx_std_20)
target_compile_definitions(${PLUGIN_NAME} PRIVATE FLUTTER_PLUGIN_IMPL)
target_include_directories(${PLUGIN_NAME} INTERFACE
        "${CMAKE_CURRENT_SOURCE_DIR}/include"
)

# Link trực tiếp với runtimeobject.lib thay vì dùng NuGet
target_link_libraries(${PLUGIN_NAME} PRIVATE
        flutter
        flutter_wrapper_plugin
        runtimeobject
)

if(MSVC)
    target_compile_options(${PLUGIN_NAME} PRIVATE "/await")
endif()

# List of absolute paths to libraries that should be bundled with the plugin
set(flutter_tts_bundled_libraries
        ""
        PARENT_SCOPE
)
