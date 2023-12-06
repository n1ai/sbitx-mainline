cmake_minimum_required(VERSION 3.0.0)
project(sBitx VERSION 0.1.0)

add_subdirectory(ft8_lib)

# Add GTK3+
find_package(PkgConfig REQUIRED)
pkg_check_modules(GTK3 REQUIRED gtk+-3.0)

# Setup CMake to use GTK+, tell the compiler where to look for headers
# and to the linker where to look for libraries
include_directories(${GTK3_INCLUDE_DIRS})
link_directories(${GTK3_LIBRARY_DIRS})

# Add other flags to the compiler
add_definitions(${GTK3_CFLAGS_OTHER})

add_executable(sBitx 
  vfo.c si570.c sbitx_sound.c fft_filter.c  sbitx_gtk.c sbitx_utils.c 
  i2cbb.c si5351v2.c ini.c hamlib.c queue.c modems.c logbook.c modem_cw.c 
  telnet.c macros.c modem_ft8.c remote.c mongoose.c webserver.c sbitx.c  
)

target_link_libraries(sBitx
  ${GTK3_LIBRARIES}
  pthread
  wiringPi
  asound
  m
  fftw3f
  ncurses
  libft8
  sqlite3
  )

include(CTest)
enable_testing()

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
