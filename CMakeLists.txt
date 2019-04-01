cmake_minimum_required(VERSION 3.6)

option(IO "Enable the I/O Services SDK" OFF)
option(VIZ "Enable the Viz & Analysis SDK" OFF)

if(IO AND VIZ)
  message(FATAL_ERROR "Exactly one of IO or VIZ must be enabled")
elseif(IO)
  project(ecp-io-sdk)
elseif(VIZ)
  project(ecp-viz-sdk)
else()
  message(FATAL_ERROR "Exactly one of IO or VIZ must be enabled")
endif()

include(GNUInstallDirs)

message("This is a dummy placeholder for the ECP Data & Viz SDK spack packages")

install(FILES README DESTINATION ${CMAKE_INSTALL_DOCDIR})
