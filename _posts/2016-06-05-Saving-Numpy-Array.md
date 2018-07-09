---
layout: post
title: You're up and running!
published: true
---
Recently, I am trying to implement a DQN in Python using Keras. I would like to continue previous training so I have to save experiences and weights for each layer (I have save weights for each layer rather than using `model.save` because I want a **multi-output network**). I investigated options for NumPy array, list and deque. There are some bad options like `ndarray.tofile` ignored.

# Summary (TL;DR)
`np.save`, `np.savez` and `np.savez_compressed` are probably the best options if you don’t need human-readable output.

If you want human-readable output, use `np.savetxt`. If you really concern about the output size of `np.savetxt`, try add `.gz` to the filename which will compress the text file.

# NumPy Array
## Machine-readable Options
### ```ndarray.dump, ndarray.dumps, pickle.dump, pickle.dumps```
`ndarray.dump` and `ndarray.dumps` are pretty similar to `pickle.dump` and `pickle.dumps` with slightly more metadata, but can be loaded by `np.load`, `np.loads` as well as `pickle.load`, `pickle.loads`.
### ```np.save, np.savez, np.savez_compressed```
`np.save` will save an array to a binary file (`.npy`). `np.savez` can save multiple arrays in a single file (`.npz`). `np.savez_compressed` saves a compressed `.npz`. While `np.load` will directly return an array for `.npy`, it will return a `NpzFile` object with a `.file` attribute for `.npz`:
```
>>> npzfile = np.load(outfile)
>>> npzfile.files
['arr_1', 'arr_0']
>>> npzfile['arr_0']
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

## Human-readable Options
### ```np.savetxt```
np.savetxt saves an array to a text file. If you the filename ends in .gz, the file is automatically saved in compressed gzip format. Restore the array with np.loadtxt which can load .gz directly.
### JSON
You might want to convert a NumPy arrays to a Python list by ndarray.tolist, so you can utilise options for list. If you want to use JSON, you have two options. First is using ndarray.tolist, which usually works but might lose information as data items are converted to the nearest compatible Python type. Second is writing your own JSONEncoder and JSONDecoder which can be tricky though.

Note that ndarray.tolist() and list(ndarray) are different, although results of recreating by np.array are same:
```
>>> a = np.array([[1, 2], [3, 4]])
>>> b = a.tolist()
>>> c = list(a)
>>> a
array([[1, 2],
       [3, 4]])
>>> b
[[1, 2], [3, 4]]
>>> c
[array([1, 2]), array([3, 4])]
>>> np.array(b)
array([[1, 2],
       [3, 4]])
>>> np.array(c)
array([[1, 2],
       [3, 4]])
```
# Python List and Deque
For me, I implemented experience with deque. However, in terms of saving data, it’s same as list. There aren’t many options here. pickle.dump is machine-readable and json.dump is human readable. Of course, csv.writer.writerows can also be used.

# Output File Size
As a simple comparison of size, I tested with:


And here is the result:

> Post migration unfinished...
