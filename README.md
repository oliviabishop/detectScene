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
