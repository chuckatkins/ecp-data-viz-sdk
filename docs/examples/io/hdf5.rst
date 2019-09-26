HDF5
****

Note: for more detailed examples and documentation see the `HDF5 Support Portal <https://portal.hdfgroup.org/display/HDF5/HDF5>`_.

Write out dataset using dynamic array:

.. code-block:: c

    #include "hdf5.h"
    #include <stdlib.h>

    #define FILE        "dyn.h5"
    #define DATASETNAME "IntArray" 
    #define NX     5                     /* dataset dimensions */
    #define NY     6
    #define RANK   2

    int
    main (void)
    {
        hid_t       file, dataset;         /* file and dataset handles */
        hid_t       datatype, dataspace;   /* handles */
        hsize_t     dimsf[2];              /* dataset dimensions */
        herr_t      status;                             
        int         **data;
        int         rows, cols;
        int         i, j;


        rows = NX;
        cols = NY;

    /**************************** BEGIN *******************************/

    /* Allocate memory for new integer array[row][col]. First allocate 
       the memory for the top-level array (rows).  Make sure you use the 
       sizeof a *pointer* to your data type. */

        data = (int**) malloc(rows*sizeof(int*));

    /* Allocate a contiguous chunk of memory for the array data
       values.  Use the sizeof data type */

        data[0] = (int*)malloc( cols*rows*sizeof(int) );

    /* Set the pointers in the top-level (row) array to the correct 
       memory locations in the data value chunk. */

        for (i=1; i < rows; i++) data[i] = data[0]+i*cols;

    /*************************** END *********************************/

        /* Data and output buffer initialization. */
        for (j = 0; j < NX; j++) {
	    for (i = 0; i < NY; i++)
	        data[j][i] = i + j;
        }     
        /*
         * 0 1 2 3 4 5 
         * 1 2 3 4 5 6
         * 2 3 4 5 6 7
         * 3 4 5 6 7 8
         * 4 5 6 7 8 9
         */

        file = H5Fcreate(FILE, H5F_ACC_TRUNC, H5P_DEFAULT, H5P_DEFAULT);

        dimsf[0] = NX;
        dimsf[1] = NY;
        dataspace = H5Screate_simple(RANK, dimsf, NULL); 

        datatype = H5Tcopy(H5T_NATIVE_INT);
        status = H5Tset_order(datatype, H5T_ORDER_LE);

        dataset = H5Dcreate(file, DATASETNAME, datatype, dataspace,
                            H5P_DEFAULT, H5P_DEFAULT, H5P_DEFAULT);

        status = H5Dwrite(dataset, H5T_NATIVE_INT, H5S_ALL, H5S_ALL,
                          H5P_DEFAULT, &data[0][0]);

        free(data[0]);
        free(data);

        H5Sclose(dataspace);
        H5Tclose(datatype);
        H5Dclose(dataset);
        H5Fclose(file);
 
        return 0;
    } 
