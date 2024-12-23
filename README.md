# Interface with dotThz files using Python

This crate provides an easy way to interface with [dotThz](https://github.com/dotTHzTAG) files in Python.

Install it

```shell
pip install pydotthz
```

and then use like specified in the following example:

```python
from pathlib import Path
import numpy as np

from dotthz import DotthzFile, DotthzMeasurement, DotthzMetaData

if __name__ == "__main__":

    # Create a new .thz file
    file = DotthzFile.new()

    # Sample data
    time = np.linspace(0, 1, 100)  # your time array
    data = np.random.rand(100)  # example 3D data array

    measurement = DotthzMeasurement()
    # for thzVer 1.00, we need to transpose the array!
    datasets = {"Sample": np.array([time, data]).T}
    measurement.datasets = datasets

    # set meta_data
    meta_data = DotthzMetaData()
    meta_data.user = "John Doe"
    meta_data.version = "1.00"
    meta_data.instrument = "Toptica TeraFlash Pro"
    meta_data.mode = "THz-TDS/Transmission"

    measurement.meta_data = meta_data

    file.groups["Measurement"] = measurement

    # save the file
    path1 = Path("test1.thz")
    file.save(path1)
    del file
    
    # create and save a second file
    path2 = Path("test2.thz")
    file = DotthzFile.new()
    file.groups["Measurment 2"] = measurement
    file.save(path2)
    del file

    # open the file again
    file = DotthzFile.load(path1)

    # add more measurements from the second file
    file.add_from_file(path2)

    # read the first group (measurement)
    key = list(file.groups.keys())[0]
    print(file.groups.get(key).meta_data)
    print(file.groups.get(key).datasets)


```
Requires hdf5 to be installed.
