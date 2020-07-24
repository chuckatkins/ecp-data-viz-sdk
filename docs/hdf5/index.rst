HDF5 Virtual Object Layer (VOL)
###############################

Note: this provides an except with the introduction to the HDF5 VOL taken from upstream documentation.  The complete documentation, including examples and quick start guides, can be found `here <https://bitbucket.hdfgroup.org/projects/HDFFV/repos/hdf5doc/browse/RFCs/HDF5/VOL/user_guide>`_.

Introduction
------------
The virtual object layer is an abstraction layer in the HDF5 library that intercepts all API calls that could potentially access objects in an HDF5 container and forwards those calls to a VOL connector, which implements the storage.  The user or application gets the benefit of using the familiar and widely-used HDF5 data model and API, but can map the physical storage of the HDF5 file and objects to storage that better meets the application’s data needs.

The VOL Abstraction Layer
^^^^^^^^^^^^^^^^^^^^^^^^^
The VOL lies just under the public API. When a storage-oriented public API call is made, the library performs a few sanity checks on the input parameters and then immediately invokes a VOL callback, which resolves to an implementation in the VOL connector that was selected when opening or creating the file. The VOL connector then performs whatever operations are needed before control returns to the library, where any final library operations such as assigning IDs for newly created/opened datasets are performed before returning. This means that, for calls that utilize the VOL, all of the functionality is deferred to the VOL connector and the HDF5 library does very little work. An important consideration of this is that most of the HDF5 caching layers (metadata and chunk caches, page buffering, etc.) will not be available as those are implemented in the HDF5 native VOL connector and cannot be easily reused by external connectors.

.. image:: https://bitbucket.hdfgroup.org/projects/HDFFV/repos/hdf5doc/browse/RFCs/HDF5/VOL/user_guide/vol_architecture.png?at=c323781cf89e12de483ff1a845ed90a3e4bf9851&raw
    :alt: vol-architecture

Not all public HDF5 API calls pass through the VOL. Only calls which require manipulating storage go through the VOL and require a VOL connector author to implement the appropriate callback. Dataspace, property list, error stack, etc. calls have nothing to do with storage manipulation or querying and do not use the VOL. This may be confusing when it comes to property list calls, since many of those calls set properties for storage. Property lists are just collections of key-value pairs, though, so a particular VOL connector is not required to set or get properties.

Another thing to keep in mind is that not every VOL connector will implement the full HDF5 public API. In some cases, a particular feature like variable-length types may not have been developed yet or may not have an equivalent in the target storage system. Also, many HDF5 public API calls are specific to the native HDF5 file format and are unlikely to have any use in other VOL connectors. A feature/capabilities flag scheme is being developed to help navigate this.

For more information about which calls go through the VOL and the mechanism by which this is implemented, see the connector author and library internals documentation.

Connectors
----------

PNetCDF
^^^^^^^
The PNetCDF connector provides read-only support for CDF files and it implemented using the pnetcdf format.  The connector is hosted at https://xgitlab.cels.anl.gov/ExaHDF5/pnetcdf_vol .  Included below is an exceprt describing the mapping of the CDF schema to the HDF5.  The full documentation, including implementation details and benchmark results can be found `here <https://xgitlab.cels.anl.gov/ExaHDF5/pnetcdf_vol/blob/master/cdf/doc/VOL_Milestone.md>`_.

CDF Schema Mapping
""""""""""""""""""
The CDF file format is relatively well-suited for the HDF5 VOL interface, because both data models were designed with multidimensional arrays in mind. In CDF, these arrays are called variables, while in HDF5 they are called datasets. What makes the mapping even more convenient is the fact that the CDF data model is essentially a subset of HDF5.  The CDF data schema consists of two fundamental components (variables and attributes), while HDF5 consists of three (groups, datasets, and attributes).  Since CDF does not support any notion of hierarchical groups, the most straightforward strategy is to treat CDF variables as HDF5 datasets, and to treat all CDF files as if there is only one group (the root group). In CDF, attributes can either be global (attached to the file), or variable-local (attached to a variable).

.. image:: https://xgitlab.cels.anl.gov/ExaHDF5/pnetcdf_vol/raw/master/cdf/doc/cdf-layout.png
    :alt: cdf-layout

ADIOS
^^^^^
An HDF5 VOL connector for the ADIOS 2.x library is currently in development and is planned for release in September of 2020.  Appropriate documentation will be added to the user guide at that time.
