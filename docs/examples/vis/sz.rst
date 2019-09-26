SZ
**

Note: for more detailed examples and documentation see `SZ User Guide <https://www.mcs.anl.gov/~shdi/download/sz-2.0-user-guide.pdf>`_.

An example to illustrate compression in C code.

.. code-block:: c

    int main (int argc , char * argv [])
    {
        size_t r5 =0 , r4 =0 , r3 =0 , r2 =0 , r1 =0;
       
        /* Get the dimension information and set configuration file - cfgFile */
        .....
       
        /* Initializing the compression environment by loading the configuration file .
        SZ_Init ( null ) will adopt default setting in the compression .*/
        int status = SZ_Init ( cfgFile ) ;
        if( status == SZ_NSCS )
            exit (0) ;
        sprintf ( outputFilePath , "%s.sz", oriFilePath ) ; // specify compression file path
       
        // read the binary data
        size_t nbEle ;
        float * data = readFloatData ( oriFilePath , & nbEle , & status ) ;
        if( status != SZ_SCES )
        {
            printf (" Error : data file %s cannot be read !\n", oriFilePath ) ;
            exit (0) ;
        }
       
        /* Perform compression . r5 , .... , r1 are sizes at each dimension . The size of a
           nonexistent dimension is 0. For instance , for a 3D dataset (10 x20x30 ), the
           setting is r5 = 0 , r4 = 0 , r3 = 10 , r2 = 20 , r3 = 30. SZ_FLOAT indicates single
           - precision . */
        size_t outSize ;
        unsigned char * bytes = SZ_compress ( SZ_FLOAT , data , & outSize , r5 , r4 , r3 , r2 , r1 ) ;
       
        // write the compression bytes to ’ outputFilePath ’
        writeByteData ( bytes , outSize , outputFilePath , & status ) ;
        if( status != SZ_SCES )
        {
            printf (" Error : data file %s cannot be written !\n", outputFilePath ) ;
            exit (0) ;
        }
       
        /* Do not forget to free the memory of compressed data if they are not useful any more .*/
        free ( bytes ) ;
        free ( data ) ;
        SZ_Finalize () ;
        return 0;
    }
