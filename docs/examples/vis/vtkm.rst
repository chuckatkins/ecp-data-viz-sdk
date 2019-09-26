VTK-m
*****

Note: for more detailed examples and documentation see `VTK-m User's Guide <http://m.vtk.org/images/c/c8/VTKmUsersGuide.pdf>`_.

A simple example of using VTK-m to load a VTK image file, run the Marching Cubes algorithm on it, and render the results to an image:

.. code-block:: c++
    
    vtkm::io::reader::VTKDataSetReader reader("path/to/vtk_image_file");
    vtkm::cont::DataSet inputData = reader.ReadDataSet();
    std::string fieldName = "scalars";
    
    vtkm::Range range;
    inputData.GetPointField(fieldName).GetRange(&range);
    vtkm::Float64 isovalue = range.Center();
    
    // Create an isosurface filter
    vtkm::filter::Contour filter;
    filter.SetIsoValue(0, isovalue);
    filter.SetActiveField(fieldName);
    vtkm::cont::DataSet outputData = filter.Execute(inputData);
    
    // compute the bounds and extends of the input data
    vtkm::Bounds coordsBounds = inputData.GetCoordinateSystem().GetBounds();
    vtkm::Vec<vtkm::Float64,3> totalExtent( coordsBounds.X.Length(),
                                            coordsBounds.Y.Length(),
                                            coordsBounds.Z.Length() );
    vtkm::Float64 mag = vtkm::Magnitude(totalExtent);
    vtkm::Normalize(totalExtent);
    
    // setup a camera and point it to towards the center of the input data
    vtkm::rendering::Camera camera;
    camera.ResetToBounds(coordsBounds);
    
    camera.SetLookAt(totalExtent*(mag * .5f));
    camera.SetViewUp(vtkm::make_Vec(0.f, 1.f, 0.f));
    camera.SetClippingRange(1.f, 100.f);
    camera.SetFieldOfView(60.f);
    camera.SetPosition(totalExtent*(mag * 2.f));
    vtkm::cont::ColorTable colorTable("inferno");
    
    // Create a mapper, canvas and view that will be used to render the scene
    vtkm::rendering::Scene scene;
    vtkm::rendering::MapperRayTracer mapper;
    vtkm::rendering::CanvasRayTracer canvas(512, 512);
    vtkm::rendering::Color bg(0.2f, 0.2f, 0.2f, 1.0f);
    
    // Render an image of the output isosurface
    scene.AddActor(vtkm::rendering::Actor(outputData.GetCellSet(),
                                          outputData.GetCoordinateSystem(),
                                          outputData.GetField(fieldName),
                                          colorTable));
    vtkm::rendering::View3D view(scene, mapper, canvas, camera, bg);
    view.Initialize();
    view.Paint();
    view.SaveAs("demo_output.pnm");
