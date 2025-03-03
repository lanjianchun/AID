---
title: Upload Package
---

## Prepare Your Model Locally

### Project Structure

Your package is firstly a Python Package. Therefore, it will look like:

```
- example_package
    - example_package
        - solver.py // required
        - bundle.py // optional
    - pretrained.toml // optional, required if your package needs to download file
    - tests // optional
    - examples // optional
    - aid.toml // required
    - requirements.txt // required
    - README.md
```

You can create the following structure by running the command ```aid new ```. It will promopt you to enter your package information, like below:
![aid-new.PNG](https://i.loli.net/2020/04/14/WI4UDNzsQGkwVn9.png)

### Solvers

Solver is a ```class``` extended from ```aid.Solver``` class. In this class, you need to implement 2 functions: ```__init__``` and ```infer```. As their literal meanings, in the ```init``` function, parameters in your package should be initiate, and in the ```infer``` function, a image file path will be passed in the parameter, and the ```infer``` function is supposed to return your result in a dict or list.

For example, a simple solver frame will look like 

```python
from aid.solver import Solver

class SampleSolver(Solver):
    def __init__(self, toml_file=None):
        super().__init__(toml_file)
        # Do you Init Work here
        self.classifer = get_classifier()
        self.ready()
    def infer(self, data):
        # if you need to get file uploaded, get the path from input_file_path in data
        image = load_image(data['input_file_path'])
        result = self.classifier(image)
        return result # return a dict
```

### aid.toml

aid.toml is the entry file where aid will look for your solvers. It uses [toml](https://github.com/toml-lang/toml) syntax and should look like the following format:

``` toml
[package]
name="Your_Package_Name"

[[solvers]] 
name="Your_Solver_1_Name"
class="path/to_your_folder/Solver_Class_Name"

[[solvers]]
name="Your_Solver_2_Name"
class="path/to_your_folder/Solver_Class_Name"

[[solvers]]
name="Your_Solver_3_Name"
class="path/to_your_folder/Solver_Class_Name"
```

Make sure this file is located at the root path of your project.

### Pre-trained models

In most occasions, your algorithms (or neural networks etc.) should be uploaded along with some pre-trained models. You can upload it onto any services that allow direct downloads via HTTP requests, e.g. GitHub releases or so.

You need to define your pre-trained models in ```pretrained.toml``` file as following using [toml syntax](https://github.com/toml-lang/toml):

``` toml
[[models]]
name = "model_file_name"
url = "https://www.example.com/your_model_file.zip" # Remote File

[[models]]
name = "model_file_name"
url = "pretrained/name_of_your_modelfile" # Local File
```

Make sure this file is located at the root path of your project. AID will automatically download the file when install packages.

## Build Locally

## Publish to GitHub

We encourage you to upload your package to GitHub repositories, as long as you want it to be open sourced.