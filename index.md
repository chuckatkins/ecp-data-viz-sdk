# ECP Data and Visualization SDK

The Data and Visualization SDK is an integration effort for the collection of I/O, compression, visualization, and analysis software developed under the DOE's Exascale Computing Program (ECP).  Software developed under ECP targets using [Spack](https://spack.io), an HPC targeted source-based package manager, for installation on HPC platforms.  The primary product of this project is the `ecp-data-vis-sdk` Spack meta-package which allows the set of SDK member packages to be built together in a way that enables optimal features for ECP target environments as well as interoperable features provided by other packages within the SDK.

## Included Projects

The following projects are included as part of the Data & Vis SDK:

* I/O and Data Management
  * [ADIOS](https://csmd.ornl.gov/software/adios2): An adaptable framework for HPC I/O supporting files, in situ, and in transit data movement.
  * [Darshan](https://www.mcs.anl.gov/research/projects/darshan/): An HPC I/O characterization tool.
  * [FAODEL](https://github.com/faodel/faodel): A collection of software libraries that are used to implement different data management services on high-performance computing (HPC) platforms.
  * [HDF5](https://www.hdfgroup.org/solutions/hdf5/): A data model, library, and file format for storing and managing data.
  * [PNetCDF](https://parallel-netcdf.github.io/): A high-performance parallel I/O library for accessing Unidata's NetCDF, files in classic formats, specifically the formats of CDF-1, 2, and 5.
  * [UnifyFS](https://unifyfs.readthedocs.io/en/latest/): A filesystem for burst buffers.
  * [VeloC](https://veloc.readthedocs.io/en/latest/): A multi-level checkpoint-restart runtime for HPC supercomputing infrastructures and large-scale data centers.
* Visualization & Analysis
  * [Ascent](https://github.com/Alpine-DAV/ascent): An open source many-core capable lightweight in situ visualization and analysis infrastructure for multi-physics HPC simulations.
  * [Cinema](https://cinemascience.github.io): An image-based approach to extreme scale in situ visualization and analysis.
  * [ParaView](https://paraview.org): An open-source, multi-platform data analysis and visualization application.
  * [SENSEI](https://sensei-insitu.org/): An interface for scalable in situ analysis and visualization of simulation data.
  * [VisIt](https://visit-dav.github.io/visit-website/): An open source, interactive, scalable, visualization, animation and analysis tool.
  * [VTK-m](https://m.vtk.org): A toolkit if scientific visualization algorithms for emerging processor architectures.
* Compression
  * [SZ](https://szcompressor.org): An error-bounded lossy data compressor for floating-point and integer datasets.
  * [ZFP](https://computing.llnl.gov/projects/zfp): An open-source library for compressed floating-point arrays that support high throughput read and write random access.

## Current Project Status

TODO: Add differentiators for tested compilers

#### Key
* CPU: If the spack package exists for CPU-only features
  * \- : None
  * 0 : Broken
  * 1 : Confirmed working
* CUDA: If the spack package supports NVIDIA CUDA
  * \- : Not applicable to this package
  * 0 : Broken
  * 1 : Confirmed working
* ROCm: If the spack package supports AMD ROCm
  * Same options as for CUDA
* SDK CPU: If the package is enabled in the SDK without GPU support
  * 0 : Broken when enabled in the SDK
  * 1 : Can only be enabled with some of the SDK packages while conflicting with others
  * 2 : Can be enabled with all the other SDK packages
* SDK CUDA: If the package is enabled in the SDK with NVIDIA CUDA enabled
  * \- : Not applicable to this package
  * All other options for SDK CPU
* SDK ROCm: If the package is enabled in the SDK with AMD ROCm enabled
  * Same options as for SDK CUDA

### General Spack support

| Project  | CPU | CUDA | ROCm | SDK CPU | SDK CUDA | SDK ROCm |
|----------|-----|------|------|---------|----------|----------|
| ADIOS    | 1   | -    | -    | 2       | -        | -        |
| Darshan  | 1   | -    | -    | 2       | -        | -        |
| FAODEL   | 1   | -    | -    | 2       | -        | -        |
| HDF5     | 1   | -    | -    | 2       | -        | -        |
| PNetCDF  | 1   | -    | -    | 2       | -        | -        |
| UnifyFS  | 1   | -    | -    | 2       | -        | -        |
| VeloC    | 1   | -    | -    | 2       | -        | -        |
| Ascent   | 1   | 1    | 1    | 2       | 1 *      | 1 *      |
| Cinema   | 1   | -    | -    | 2       | -        | -        |
| ParaView | 1   | 0 *  | -    | 2       | 0 *      | -        |
| SENSEI   | 1   | -    | -    | 0 *     | -        | -        |
| VisIt    | 1   | -    | -    | 0 *     | -        | -        |
| VTK-m    | 1   | 1    | 1    | 2       | 1 *      | 1 *      |
| SZ       | 1   | -    | -    | 2       | -        | -        |
| ZFP      | 1   | 1    | -    | 2       | 2        | -        |

#### Notes
* Ascent
  * The lastest available verison in the spack package depends on an older version of VTK-m which doesn't properly implement CUDA and ROCm.  This creates a conflict with being able to enable the latest versions of all packages with GPU features enabled.  The most recent upstream Ascent release uses an appropriately newer version of VTK-m but it has not yet made it to the spack package.
* ParaView
  * While CUDA can be enabled in ParaView it requires some recent patches to the `master` branch to work correctly.  We are iterating with the ParaView team to get this enabled in Spack.
* SENSEI
  * SENSEI has just recently become part of ECP and has not yet been fully integrated into the SDK
* VisIt
  * The VisIt package has recently undergone a whole-package rewrite addressing many of the long standing isuses preventing it from being integrated into the SDK.  The updated package has not yet been worked into the SDK.
* VTK-m
  - See Ascent issues


### Perlmutter Spack support

| Project  | CPU | CUDA | ROCm | SDK CPU | SDK CUDA |
|----------|-----|------|------|---------|----------|
| ADIOS    | 1   | -    | -    | 2       | -        |
| Darshan  | 1   | -    | -    | 2       | -        |
| FAODEL   | 1   | -    | -    | 2       | -        |
| HDF5     | 1   | -    | -    | 2       | -        |
| PNetCDF  | 1   | -    | -    | 2       | -        |
| UnifyFS  | 1   | -    | -    | 2       | -        |
| VeloC    | 1   | -    | -    | 2       | -        |
| Ascent   | 1   | 1    | 1    | 2       | 1        |
| Cinema   | 1   | -    | -    | 2       | -        |
| ParaView | 1   | 0    | -    | 2       | 0        |
| SENSEI   | 1   | -    | -    | 0       | -        |
| VisIt    | 1   | -    | -    | 0       | -        |
| VTK-m    | 1   | 1    | 1    | 2       | 1 *      |
| SZ       | 1   | -    | -    | 2       | -        |
| ZFP      | 1   | 1    | -    | 2       | 2        |

#### Notes
* VTK-m
  * Several versions of the NVIDIA CUDA compiler are known to crash when building VTK-m


### Frontier (currently Spock) Spack support

| Project  | CPU | CUDA | ROCm | SDK CPU | SDK ROCm |
|----------|-----|------|------|---------|----------|
| ADIOS    | 1   | -    | -    | 2       | -        |
| Darshan  | 1   | -    | -    | 2       | -        |
| FAODEL   | 1   | -    | -    | 2       | -        |
| HDF5     | 1   | -    | -    | 2       | -        |
| PNetCDF  | 1   | -    | -    | 2       | -        |
| UnifyFS  | 1   | -    | -    | 2       | -        |
| VeloC    | 1   | -    | -    | 2       | -        |
| Ascent   | 1   | 1    | 1    | 2       | 1        |
| Cinema   | 1   | -    | -    | 2       | -        |
| ParaView | 1   | 0    | -    | 2       | -        |
| SENSEI   | 1   | -    | -    | 0       | -        |
| VisIt    | 1   | -    | -    | 0       | -        |
| VTK-m    | 1   | 1    | 1    | 2       | 1 *      |
| SZ       | 1   | -    | -    | 2       | -        |
| ZFP      | 1   | 1    | -    | 2       | -        |

#### Notes
* VTK-m
  * Several versions of the AMD compiler are known to crash when building VTK-m
