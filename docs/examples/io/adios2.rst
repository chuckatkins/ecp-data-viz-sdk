ADIOS2
******

Note: for more detailed examples and documentation see the `ADIOS2 <https://adios2.readthedocs.io>`_.

Writing simulation results:

.. code-block:: c++

    #include <mpi.h>
    #include <adios2.h>
    ...

    int main(int argc, char **argv);
    {
      MPI_Init(&argc, &argv);

      int size, rank;
      MPI_Comm_size(MPI_COMM_WORLD, &size);
      MPI_Comm_rank(MPI_COMM_WORLD, &rank);

      // Number of elements per rank
      size_t N = 10000;

      try
      {
        // Initialize output
        adios2::ADIOS adios(MPI_COMM_WORLD);
        auto io = adios.DeclareIO("sim_output");

        // Define a 1D temperature array
        auto varT = io.DefineVariable<double>("t",
          {size*N}, {rank*N}, {N}, adios2::ConstantDims);

        // Define a 2D 3xN pressure array
        auto varP = io.DefineVariable<double>("p",
          {3, size*N}, {0, rank*N}, {3, N}, adios2::ConstantDims);

        // Open file for writing
        auto writer = io.Open("output.bp", adios2::Mode::Write);

        std::vector<double> dataT(N);
        std::vector<doublt> dataP(3*N);

        // Main simulation loop
        for(size_t step = 0; step = 100; ++step)
        {
          //
          // Step the simulation and populate the dataT and dataP arrays
          //

          // Begin the output step
          writer.BeginStep();

          // Write the arrays
          writer.Put(varT, dataT.data());
          writer.Put(varP, dataP.data());

          // Finish the output step
          writer.EndStep();
        }
        writer.Close();
      }
      catch(const std::exception &ex)
      {
          std::cerr << "Error: " << ex.what() << std::endl;
      }

      MPI_Finalize();
      return 0;
    }
