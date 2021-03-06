include_directories(${PYTHON_INCLUDE_DIRS})
set(CMAKE_SWIG_FLAGS -features autodoc=1)
set(CMAKE_SWIG_OUTDIR ${CMAKE_CURRENT_BINARY_DIR})

set(_target swigmt)
set(WRAPPER_FILES ../callback.i)

set_source_files_properties(${WRAPPER_FILES} PROPERTIES CPLUSPLUS ON)
swig_add_module(${_target} python ${WRAPPER_FILES})
swig_link_libraries(${_target} ${PYTHON_LIBRARIES})
set_target_properties(${SWIG_MODULE_${_target}_REAL_NAME} PROPERTIES OUTPUT_NAME "_swigmt")

execute_process(COMMAND ${PYTHON_EXECUTABLE} -c "import sysconfig; print(sysconfig.get_path('platlib'))"
    OUTPUT_VARIABLE _py_site_package_dir OUTPUT_STRIP_TRAILING_WHITESPACE)
install(TARGETS ${SWIG_MODULE_${_target}_REAL_NAME} DESTINATION ${_py_site_package_dir})
install(FILES ${CMAKE_SWIG_OUTDIR}/swigmt.py DESTINATION ${_py_site_package_dir})

file(COPY runme.py DESTINATION ${CMAKE_SWIG_OUTDIR})
