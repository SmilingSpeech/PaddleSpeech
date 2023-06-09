set(srcs)

list(APPEND srcs
  recognizer_controller.cc
  recognizer_controller_impl.cc
  recognizer_instance.cc
  recognizer.cc
)

add_library(recognizer STATIC ${srcs})
target_link_libraries(recognizer PUBLIC decoder)

set(TEST_BINS 
  recognizer_batch_main
  recognizer_batch_main2
  recognizer_main
)

foreach(bin_name IN LISTS TEST_BINS)
  add_executable(${bin_name} ${CMAKE_CURRENT_SOURCE_DIR}/${bin_name}.cc)
  target_include_directories(${bin_name} PRIVATE ${SPEECHX_ROOT} ${SPEECHX_ROOT}/kaldi)
  target_link_libraries(${bin_name} recognizer nnet decoder fst utils gflags glog kaldi-base kaldi-matrix kaldi-util)
  target_compile_options(${bin_name}  PRIVATE ${PADDLE_COMPILE_FLAGS})
  target_include_directories(${bin_name}  PRIVATE ${pybind11_INCLUDE_DIRS} ${PROJECT_SOURCE_DIR})
  target_link_libraries(${bin_name}  ${PYTHON_LIBRARIES} ${PADDLE_LINK_FLAGS} -ldl)
endforeach()
