# detectScene
An explanation of the detectScene function in the Machine Learning Vision project


## Step 1 - Declaring the function 
- This function as a whole serves the purpose of detecting and analyzing different objects in a provided image.
- Type should be CIImage 

        func detectScene(image: CIImage) { }
        
## Step 2 - Loading the Machine Learning Model
- The first line alerts the user that the scene is being detected
- VNCoreModel will create a class from the ML model
- The ML model will be loaded through its generated class, if this fails it should alert the user that the ML model could not be loaded


        displayString(string: "detecting scene...")
        
        guard let model = try? VNCoreMLModel(for: GoogLeNetPlaces().model) else {
         displayString(string: "Can't load ML model.")
         return
         }
        
        guard let model = try? VNCoreMLModel(for: VGG16().model) else {
            displayString(string: "Can't load ML model.")
            return
        }      
        
        
## Step 3 - Create a Vision Request
- After the specified ML model is loaded a vision request needs to be created 
- A method needs to be set using the object's completetion handler, specifying where the results will be sent after the request has been run
- The user will recieve a message if the vision request recieves an unexpected result

        let request = VNCoreMLRequest(model: model) { [weak self] request, error in
            guard let results = request.results as? [VNClassificationObservation],
                let _ = results.first else {
                    self?.displayString(string: "Unexpected result type from VNCoreMLRequest")
                    return
            }
           }
            
## Step 4 - Update the UI
- DispatchQueue manages the execution of tasks on the apps main (.main) queue. This is scheduled asynchronously (.async) meaning the code will continuously execute while the item runs somewhere else
- At this point the activity indicator will be set to stop the animation of the progress indicator
- A string will be displayed based on cofidence/indetifier

        DispatchQueue.main.async { [weak self] in
                self?.activityIndicator.stopAnimating()
                for result in results {
                    self?.displayString(string: "\(Int(result.confidence * 100))% \(result.identifier)")
                }
               }
              }
              
## Step 5 - Run core ML classifier
- The activity indicator is set to start the animation of the progress indicator
- VNImageRequestHandler processes image analysis requests for the provided image
- The core ML will be processed on a gloabl dispatch queue
- perform is called to execute the requests
- Error handling is used if the code in the do clause fails

        activityIndicator.startAnimating()
        
         let handler = VNImageRequestHandler(ciImage: image)
        DispatchQueue.global(qos: .userInteractive).async {
            do {
                try handler.perform([request])
            } catch {
                DispatchQueue.main.async { [weak self] in
                    self?.displayString(string: error.localizedDescription)
                    self?.activityIndicator.stopAnimating()
                }
            }
        }
    }
    
    
#### To view function in its entirety click here
https://github.com/oliviabishop/detectScene/wiki/detectScene-Function
