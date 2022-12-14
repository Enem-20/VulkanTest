project(VulkanTest)

add_executable(${PROJECT_NAME} src/main.cpp)

set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 20)

if(WIN32 AND MSVC)
	set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT VulkanTest)
endif(WIN32 AND MSVC)

add_Subdirectory(external/glfw)
add_subdirectory(external/VulkanHpp)

set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(GLFW_INSTALL OFF CACHE BOOL "" FORCE)

if(WIN32)
	target_link_libraries(${PROJECT_NAME} PRIVATE 
		"$ENV{VULKAN_SDK}/Lib/vulkan-1.lib"
		)
endif(WIN32)

if(UNIX)
	target_link_libraries(${PROJECT_NAME} PRIVATE 
		"$ENV{VULKAN_SDK}/Lib/vulkan-1.a"
		)
endif(UNIX)

target_link_libraries(${PROJECT_NAME} PRIVATE 
	glfw
	)

target_include_directories(${PROJECT_NAME} PRIVATE 
	external/VulkanHpp/Vulkan-Headers/include
	external/glm
	external/stb
)

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
					COMMAND ${CMAKE_COMMAND} -E copy_directory
					${CMAKE_SOURCE_DIR}/res $<TARGET_FILE_DIR:${PROJECT_NAME}>/res)
					
