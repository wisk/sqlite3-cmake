PROJECT(sqlite3)
cmake_minimum_required(VERSION 2.8)

option(SQLITE_BUILD_SHELL "Build sqlite3 shell" FALSE)

include_directories(${CMAKE_SOURCE_DIR}/src)
add_library(sqlite3 STATIC src/sqlite3.c src/sqlite3.h src/sqlite3ext.h)


target_compile_definitions(sqlite3
  PRIVATE
  -DSQLITE_ENABLE_RTREE
  -DSQLITE_ENABLE_FTS4
  -DSQLITE_ENABLE_FTS5
  -DSQLITE_ENABLE_JSON1
  -DSQLITE_ENABLE_UNLOCK_NOTIFY
)

install(FILES src/sqlite3.h src/sqlite3ext.h DESTINATION include)
install(TARGETS sqlite3 LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)

if (SQLITE_BUILD_SHELL)
  add_executable(shell src/sqlite3.c src/shell.c src/sqlite3.h src/sqlite3ext.h)
  find_package(Threads REQUIRED)
  target_link_libraries(shell ${CMAKE_THREAD_LIBS_INIT})
  install(TARGETS shell RUNTIME DESTINATION bin)
endif()
