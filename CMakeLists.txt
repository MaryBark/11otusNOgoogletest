#cmake_minimum_required(VERSION 3.5)

#project(11otusjoin_server LANGUAGES CXX)

#set(CMAKE_CXX_STANDARD 11)
#set(CMAKE_CXX_STANDARD_REQUIRED ON)

#add_executable(11otusjoin_server main.cpp)

#cmake_minimum_required(VERSION 3.2)

#if (DEFINED ENV{TRAVIS_BUILD_NUMBER})
#   project(11otusjoin_server VERSION 0.0 $ENV{TRAVIS_BUILD_NUMBER})
#else()
#   project(11otusjoin_server VERSION 0.0.1)
#endif()

##find_package(GTest REQUIRED)
#find_package(Boost #[[COMPONENTS system]] REQUIRED)

#set(INCLUDE_DIR ${PROJECT_SOURCE_DIR})
#file(GLOB HEADER_FILES *.h *.cpp)
#add_executable(11otusjoin_server ${HEADER_FILES} main.cpp)

#set_target_properties(11otusjoin_server PROPERTIES
#        CXX_STANDARD 14
#        CXX_STANDARD_REQUIRED ON
#        COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
#        INCLUDE_DIRECTORIES "${Boost_INCLUDE_DIR};${INCLUDE_DIR}"
#)

#enable_testing()
#add_executable(11otusjoin_server_tests ${HEADER_FILES} test_main.cpp)

##set_target_properties(11otusjoin_server_tests PROPERTIES INCLUDE_DIRECTORIES ${GTEST_INCLUDE_DIRS} )

#if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
#        set_target_properties(11otusjoin_server PROPERTIES COMPILE_OPTIONS -W3)
#        target_link_libraries(11otusjoin_server ${Boost_LIBRARIES} )
#        target_link_libraries(11otusjoin_server_tests ${GTEST_LIBRARIES} ${GTEST_MAIN_LIBRARIES})
#else()
#        set_target_properties(11otusjoin_server PROPERTIES COMPILE_OPTIONS -Wpedantic -Wall -Wextra)
#        target_link_libraries(11otusjoin_server pthread ${Boost_LIBRARIES} )
#        target_link_libraries(11otusjoin_server_tests pthread ${GTEST_LIBRARIES} ${GTEST_MAIN_LIBRARIES})
#endif()


##add_test(test_symmetric_difference	11otusjoin_server_tests)
##add_test(test_intersection			11otusjoin_server_tests)
##add_test(test_output				11otusjoin_server_tests)
##add_test(test_commands				11otusjoin_server_tests)
##add_test(test_command_errors		11otusjoin_server_tests)

#install(TARGETS 11otusjoin_server RUNTIME DESTINATION bin)

#set(CPACK_GENERATOR DEB)

#set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
#set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
#set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

#set(CPACK_PACKAGE_CONTACT Alttaira@yandex.ru)

#include (CPack)


cmake_minimum_required(VERSION 3.2)

project(11otusjoin_server VERSION 0.0 ${PATCH_VERSION})

#if($ENV{TRAVIS_BUILD_NUMBER})
#    project(allocator1 VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
#else()
#    project(allocator1 VERSION 0.0.1)
#endif()

configure_file(version.h.in version.h)

add_executable(11otusjoin_server main.cpp command_handler.h server_command.h error_code.h table.h server.h)

#find_package(Boost)

#set_target_properties(11otusjoin_server PROPERTIES
#        CXX_STANDARD 14
#        CXX_STANDARD_REQUIRED ON
#        COMPILE_OPTIONS "-Wpedantic;-Wall;-Wextra"
#        )

#set_target_properties(11otusjoin_server PROPERTIES
#        CXX_STANDARD 14
#        CXX_STANDARD_REQUIRED ON
#        COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
#        INCLUDE_DIRECTORIES "${Boost_INCLUDE_DIR};${INCLUDE_DIR}"
#)


#set_target_properties(11otusjoin_server PROPERTIES COMPILE_OPTIONS -Wpedantic -Wall -Wextra)
#target_link_libraries(11otusjoin_server pthread ${Boost_LIBRARIES} )

#
if (CPP_OTUS_ENABLE_BOOST)
  if (WIN32)
    set(Boost_USE_STATIC_LIBS OFF) # в Linux не работает в разных дистрибутивах
  else()
    set(Boost_USE_STATIC_LIBS ON) # надо для распространения пакетов БЕЗ зависимости от версии boost (иначе возникает сложность с   настройкой CPACK_DEBIAN_PACKAGE_DEPENDS)
    set(Boost_USE_MULTITHREADED ON)
    set(Boost_USE_STATIC_RUNTIME OFF)
  endif()
  add_definitions(-DBOOST_ALL_NO_LIB) # disable auto-linking
  add_definitions(-DBOOST_ALL_DYN_LINK)
  set(Boost_ADDITIONAL_VERSIONS 1.74.0; 1.75.0; 1.78.0; 1.79.0)
  find_package (Boost 1.68.0 OPTIONAL COMPONENTS program_options filesystem regex) # thread
  if( Boost_FOUND )
    if (WIN32)
      add_definitions(-DBOOST_CONFIG_SUPPRESS_OUTDATED_MESSAGE) # начиная с 1.66.0 можно подавить "Boost.Config is older than your   compiler version..."
    endif()
  else()
    set(Boost_ADDITIONAL_VERSIONS 1.54.0)
    find_package (Boost 1.36.0 REQUIRED COMPONENTS) # thread
  endif()
  cpp_otus_add_prerequisites_dirs(${Boost_LIBRARY_DIRS})
endif()
if( NOT Boost_FOUND )
  if (CPP_OTUS_ENABLE_BOOST)
    message(WARNING "Not found boost!")
  endif()
elseif (BOOST_ROOT)
  message ( STATUS "Boost version: ${Boost_VERSION}. Location: ${BOOST_ROOT}")
elseif(Boost_LIBRARY_DIRS)
  message ( STATUS "Boost version: ${Boost_VERSION}. Location: ${Boost_LIBRARY_DIRS}")
else()
  message ( STATUS "Boost version: ${Boost_VERSION}")
endif()




install(TARGETS 11otusjoin_server RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")


set(CPACK_PACKAGE_CONTACT Alttaira@yandex.ru)

# ну на фиг эти гугл тесты, я с тними уже задолбалась((((
include(CPack)


