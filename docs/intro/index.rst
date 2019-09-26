What is it?
###########
The Data and Vis SDK is a collection of software packages being developed under the DOE's Exascale Computing Program with a focus on I/O, compression, and visualization.  The SDK provides a mechanism to bring common capabilities with respect to development practices, software quality, installation, and usability.

What's in it?
#############
The Data and Vis SDK is currently broken into two distinct packages containing the following products:

ecp-io-sdk
----------
I/O and data management

HDF5
  A data model, library, and file format for storing and managing data.

ADIOS
  A framework designed for scientific data I/O to publish and subscribe (put/get) data when and where required.

PNetCDF
  A parallel I/O library for NetCDF file access.

Darshan
  A scalable HPC I/O characterization tool.

Mercury
  A C library for implementing Remote Procedure Call, optimized for High-Performance Computing Systems.

UnifyFS
  A user-level burst buffer file system under active development. UnifyFS supports scalable and efficient aggregation of I/O bandwidth from burst buffers while having the same life cycle as a batch-submitted job.

VeloC
  A multi-level checkpoint/restart runtime that delivers high performance and scalability for complex heterogeneous storage hierarchies without sacrificing ease of use and flexibility.


ecp-viz-sdk
-----------
Visualization, analysis, and data reduction

ParaView
  A multi-platform data analysis and visualization application.

Catalyst
  An in-situ use case library for ParaView.

VTK-m
  A toolkit of scientific visualization algorithms for emerging processor architectures.

ZFP
  A library for compressed numerical arrays that support high throughput read and write random access.

SZ
  An error-bounded lossy data compressor.
