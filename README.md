# detectScene
An explanation of the detectScene function in the Machine Learning Vision project


## Step 1 - Declaring the function 
- This function as a whole serves the purpose of detecting and analyzing different objects in a provided image.
- Type should be CIImage 

        func detectScene(image: CIImage) { }
        
## Step 2 - Loading the Machine Learning Model


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

        let request = VNCoreMLRequest(model: model) { [weak self] request, error in
            guard let results = request.results as? [VNClassificationObservation],
                let _ = results.first else {
                    self?.displayString(string: "Unexpected result type from VNCoreMLRequest")
                    return
            }
           }
            
## Step 4 - Update the UI

        DispatchQueue.main.async { [weak self] in
                self?.activityIndicator.stopAnimating()
                for result in results {
                    self?.displayString(string: "\(Int(result.confidence * 100))% \(result.identifier)")
                }
               }
              }
              
## Step 5 - Run core ML classifier

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
