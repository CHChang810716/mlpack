# Define the files we need to compile.
# Anything not in this list will not be compiled into mlpack.
set(SOURCES
  neighbor_search.hpp
  neighbor_search_impl.hpp
  neighbor_search_rules.hpp
  neighbor_search_rules_impl.hpp
  neighbor_search_stat.hpp
  ns_model.hpp
  ns_model_impl.hpp
  sort_policies/nearest_neighbor_sort.hpp
  sort_policies/nearest_neighbor_sort_impl.hpp
  sort_policies/furthest_neighbor_sort.hpp
  sort_policies/furthest_neighbor_sort_impl.hpp
  typedef.hpp
  unmap.hpp
  unmap.cpp
)

# Add directory name to sources.
set(DIR_SRCS)
foreach(file ${SOURCES})
  set(DIR_SRCS ${DIR_SRCS} ${CMAKE_CURRENT_SOURCE_DIR}/${file})
endforeach()
# Append sources (with directory name) to list of all mlpack sources (used at
# the parent scope).
set(MLPACK_SRCS ${MLPACK_SRCS} ${DIR_SRCS} PARENT_SCOPE)

# Add mlpack_knn and mlpack_kfn executables.
add_cli_executable(knn)
# add_cli_executable(kfn)

if (BUILD_CLI_EXECUTABLES)
  # -- mlpack_knn/mlpack_kfn compatibility start --
  # Make a copy of mlpack_knn/mlpack_kfn both on Windows and *unix for
  # compatibility.  This should be removed by mlpack 3.0.0.

  set(knn_ext ${CMAKE_EXECUTABLE_SUFFIX})
  add_custom_command(TARGET mlpack_knn POST_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy
      $<TARGET_FILE:mlpack_knn> $<TARGET_FILE_DIR:mlpack_knn>/mlpack_allknn${knn_ext}
  )

  # set(kfn_ext ${CMAKE_EXECUTABLE_SUFFIX})

  # add_custom_command(TARGET mlpack_kfn POST_BUILD
  #     COMMAND ${CMAKE_COMMAND} -E copy
  #     $<TARGET_FILE:mlpack_kfn> $<TARGET_FILE_DIR:mlpack_kfn>/mlpack_allkfn${kfn_ext}
  # )

  install(PROGRAMS $<TARGET_FILE_DIR:mlpack_knn>/mlpack_allknn${knn_ext}
      DESTINATION bin)
  # install(PROGRAMS $<TARGET_FILE_DIR:mlpack_kfn>/mlpack_allkfn${kfn_ext}
  #     DESTINATION bin)
endif ()
