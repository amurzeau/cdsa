cmake_minimum_required(VERSION 3.1)
project(cdsai LANGUAGES C)

option(BUILD_TESTING "Build the testing tree" ${BUILD_TESTING})

function(cdsa_library arg_name)
	# Library object
	add_library(cdsa_${arg_name} OBJECT src/${arg_name}.c src/${arg_name}.h)
	target_include_directories(cdsa_${arg_name} PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>)

	# Tests
	if(BUILD_TESTING)
		add_executable(test_${arg_name} tests/test_${arg_name}.c)
		target_link_libraries(test_${arg_name} PRIVATE cdsa_${arg_name})
		add_test(NAME test_${arg_name} COMMAND test_${arg_name} cmake)
	endif()
endfunction()

cdsa_library(hash_string)
cdsa_library(hashtable)
cdsa_library(list)
cdsa_library(queue)
cdsa_library(rbtree)
cdsa_library(stack)

if(BUILD_TESTING)
	target_link_libraries(test_hash_string PRIVATE cdsa_hashtable)
endif()

