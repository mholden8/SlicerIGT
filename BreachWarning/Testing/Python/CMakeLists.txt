
#slicer_add_python_unittest(SCRIPT ${MODULE_NAME}ModuleTest.py)

#-----------------------------------------------------------------------------
set(MODULE_NAME BreachWarningSelfTest)

#-----------------------------------------------------------------------------
set(MODULE_PYTHON_SCRIPTS
  ${MODULE_NAME}.py
  )

set(MODULE_PYTHON_RESOURCES
  )

#-----------------------------------------------------------------------------
slicerMacroBuildScriptedModule(
  NAME ${MODULE_NAME}
  SCRIPTS ${MODULE_PYTHON_SCRIPTS}
  RESOURCES ${MODULE_PYTHON_RESOURCES}
  )

#-----------------------------------------------------------------------------
if(CMAKE_CONFIGURATION_TYPES)
  set(MODULE_BUILD_DIR "")
  foreach(config ${CMAKE_CONFIGURATION_TYPES})
    list(APPEND MODULE_BUILD_DIR "${CMAKE_BINARY_DIR}/${Slicer_QTLOADABLEMODULES_LIB_DIR}/${config}")
  endforeach()
else()
  set(MODULE_BUILD_DIR "${CMAKE_BINARY_DIR}/${Slicer_QTLOADABLEMODULES_LIB_DIR}")
endif()

# Register the unittest subclass in the main script as a ctest.
# Note that the test will also be available at runtime.
slicer_add_python_unittest(
  SCRIPT ${MODULE_NAME}.py
  # TODO: slicer_add_python_unittest should take care of adding the additional module paths
  SLICER_ARGS
    --disable-cli-modules
    --additional-module-paths ${MODULE_BUILD_DIR} ${CMAKE_BINARY_DIR}/${Slicer_QTSCRIPTEDMODULES_LIB_DIR}
  )
