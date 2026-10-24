cmake_minimum_required(VERSION 3.11)

project(int2048)

enable_testing()

include_directories(src/include)

set(DATA_PATH "${CMAKE_CURRENT_SOURCE_DIR}/data")
file(GLOB_RECURSE SOURCES "${DATA_PATH}/**.cpp")

foreach (SOURCE ${SOURCES})
  string(REPLACE "${CMAKE_CURRENT_SOURCE_DIR}/data/Integer" "" TESTCASE_NAME "${SOURCE}")
  string(REPLACE "/" "_" TESTCASE_NAME "${TESTCASE_NAME}")
  string(REPLACE ".cpp" ".in" INPUT ${SOURCE})
  string(REPLACE ".cpp" ".out" OUTPUT ${SOURCE})
  add_executable("${TESTCASE_NAME}" ${SOURCE} src/int2048.cpp)
  if (EXISTS ${INPUT})
    add_test(NAME ${TESTCASE_NAME} COMMAND bash -c "$<TARGET_FILE:${TESTCASE_NAME}> <${INPUT} | diff ${OUTPUT} -")
  else ()
    add_test(NAME ${TESTCASE_NAME} COMMAND bash -c "$<TARGET_FILE:${TESTCASE_NAME}> | diff ${OUTPUT} -")
  endif()
  set_property(TEST ${TESTCASE_NAME} PROPERTY TIMEOUT 10)
endforeach()