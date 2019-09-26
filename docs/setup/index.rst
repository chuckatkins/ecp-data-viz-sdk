Spack Packages
##############
Spack, a source-based package manager for supercomputing environments, is the chosen deployment mechanism for the Data and Vis SDK. For both the ``ecp-io-sdk``  and ``ecp-viz-sdk`` packages, each currently supported product is enabled through a spack variant.  

ecp-io-sdk
----------
The I/O and data services products are available through the ``ecp-io-sdk`` spack meta-package.  As of the publishing of this guide all variants are on by default allowing all products to be installed simultaneously::

    $ spack info ecp-io-sdk
    ...
    Description:
        ECP I/O Services SDK
    ...
    Variants:
    ...
        adios2 [on]                    True, False             Enable ADIOS2
        darshan [on]                   True, False             Enable Darshan
        hdf5 [on]                      True, False             Enable HDF5
        mercury [on]                   True, False             Enable Mercury
        pnetcdf [on]                   True, False             Enable PNetCDF
        unifyfs [on]                   True, False             Enable UnifyFS
        veloc [on]                     True, False             Enable VeloC
    ...

    $ spack install ecp-io-sdk

ecp-viz-sdk
-----------
The visualization and data reduction products are available through the ``ecp-viz-sdk`` spack meta-package.  As of the publishing of this guide all variants are off by default allowing each product to be installed one at a time::

    $ spack info ecp-viz-sdk
    ...
    Description:
        ECP Viz & Analysis SDK
    ...
    Variants:
    ...
        catalyst [off]                 True, False             Enable Catalyst
        paraview [off]                 True, False             Enable ParaView
        sz [off]                       True, False             Enable SZ
        vtkm [off]                     True, False             Enable VTK-m
        zfp [off]                      True, False             Enable ZFP
    ...

    $ spack install ecp-viz-sdk
