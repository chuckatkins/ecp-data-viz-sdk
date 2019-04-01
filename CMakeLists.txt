cmake_minimum_required(VERSION 3.6)

option(IO "Enable the I/O Services SDK" OFF)
option(VIS "Enable the Vis & Analysis SDK" OFF)

if(IO AND VIS)
  message(FATAL_ERROR "Exactly one of IO or VIS must be enabled")
elseif(IO)
  project(ecp-io-sdk)
elseif(VIS)
  project(ecp-vis-sdk)
else()
  message(FATAL_ERROR "Exactly one of IO or VIS must be enabled")
endif()

include(GNUInstallDirs)

message("This is a dummy placeholder for the ECP Data & Viz SDK spack packages")

install(FILES README DESTINATION ${CMAKE_INSTALL_DOCDIR})
