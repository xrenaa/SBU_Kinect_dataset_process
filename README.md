# SBU_dataset_process
a python code to pre-process of SBU Kinect Interaction Dataset: https://www3.cs.stonybrook.edu/~kyun/research/kinect_interaction/index.html


You should check the jupyter notebook "download.ipynb"

it includes:
+ the code to display the dataset,
+ the code to directly download the dataset and unzip the dataset.
+ the code to load the dataset to json files
+ the code to split it to training data and val data
+ the code to interpolate to the same size (need pytorch)

There may be some adjustment for your own case.

And this repo provides the json file directly in "./json"
to load the json file, you should use the function blew:
```
import json
class NumpyEncoder(json.JSONEncoder):
    """ Special json encoder for numpy types """
    def default(self, obj):
        if isinstance(obj, (np.int_, np.intc, np.intp, np.int8,
            np.int16, np.int32, np.int64, np.uint8,
            np.uint16, np.uint32, np.uint64)):
            return int(obj)
        elif isinstance(obj, (np.float_, np.float16, np.float32, 
            np.float64)):
            return float(obj)
        elif isinstance(obj,(np.ndarray,)): #### This is the fix
            return obj.tolist()
        return json.JSONEncoder.default(self, obj) 
    
def save_to_json(dic,target_dir):
    dumped = json.dumps(dic, cls=NumpyEncoder)  
    file = open(target_dir, 'w')  
    json.dump(dumped, file)
    file.close()
    
def read_from_json(target_dir):
    f = open(target_dir,'r')
    data = json.load(f)
    data = json.loads(data)
    f.close()
    return data 
    
```

For example:
```
dict = read_from_json("./json/train.json")
x = dict["x"]
label = dict["label"]
# the shape of x is (198, 2, 25, 15, 2)-> (N,M,T,V,C)
# Where N is the size of the dataset, M is the num of person, T is the size of frame, V is the size of joint, C is the num of Channel which is (x,y). 
# For my purpose of use, I just pick x and y coordinate you can change the code in download.ipynb and load the 3 channel one.

# the shape of label is (198,)
```

Note:
For my purpose of use, I just pick x and y coordinate you can change the code in download.ipynb and load the 3 channel one.



