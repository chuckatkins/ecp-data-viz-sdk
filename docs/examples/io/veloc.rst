VeloC
*****
    
Note: for more detailed examples and documentation see the `Veloc <https://veloc.readthedocs.io>`_.
 
Write a checkpoint file every `K` iteratons.

.. code-block:: c
    
    MPI_Init(&argc, &argv);
    VELOC_Init(MPI_COMM_WORLD, argv[2]); // (1): init

    // further initialization code
    // allocate two critical double arrays of size M
    h = (double *) malloc(sizeof(double *) * M * nbLines);
    g = (double *) malloc(sizeof(double *) * M * nbLines);

    // (2): protect
    VELOC_Mem_protect(0, &i, 1, sizeof(int));
    VELOC_Mem_protect(1, h, M * nbLines, sizeof(double));
    VELOC_Mem_protect(2, g, M * nbLines, sizeof(double));

    // (3): check for previous checkpoint version
    int v = VELOC_Restart_test("heatdis", 0);

    // (4): restore memory content if previous version found
    if (v > 0) {
        printf("Previous checkpoint found at iteration %d, initiating restart...\n", v);
        assert(VELOC_Restart_begin("heatdis", v) == VELOC_SUCCESS);
        char veloc_file[VELOC_MAX_NAME];
        assert(VELOC_Route_file(veloc_file) == VELOC_SUCCESS);
        int valid = 1;
        FILE* fd = fopen(veloc_file, "rb");
        if (fd != NULL) {
            if (fread(&i, sizeof(int),            1, fd) != 1)         { valid = 0; }
            if (fread( h, sizeof(double), M*nbLines, fd) != M*nbLines) { valid = 0; }
            if (fread( g, sizeof(double), M*nbLines, fd) != M*nbLines) { valid = 0; }
            fclose(fd);
        } else
            // failed to open file
            valid = 0;
            assert(VELOC_Restart_end(valid) == VELOC_SUCCESS);
    } else
        i = 0;

    while (i < n) {
        // iteratively compute the heat distribution

        // (5): checkpoint every K iterations
        if (i % K == 0) {
            assert(VELOC_Checkpoint_wait() == VELOC_SUCCESS);
            assert(VELOC_Checkpoint_begin("heatdis", i) == VELOC_SUCCESS);
            char veloc_file[VELOC_MAX_NAME];
            assert(VELOC_Route_file(veloc_file) == VELOC_SUCCESS);
            int valid = 1;
            FILE* fd = fopen(veloc_file, "wb");
            if (fd != NULL) {
                if (fwrite(&i, sizeof(int),            1, fd) != 1)         { valid = 0; }
                if (fwrite( h, sizeof(double), M*nbLines, fd) != M*nbLines) { valid = 0; }
                if (fwrite( g, sizeof(double), M*nbLines, fd) != M*nbLines) { valid = 0; }
                fclose(fd);
            } else
                // failed to open file
                valid = 0;
            assert(VELOC_Checkpoint_end(valid) == VELOC_SUCCESS);
        }
        // increment the number of iterations
        i++;
    }
    VELOC_Finalize(0); // (6): finalize
    MPI_Finalize();
