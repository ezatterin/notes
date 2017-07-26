# Howto h5py

Most important:

> Groups behave like *dictionaries*; Datasets behave like *numpy arrays*

Open file:

```python
f = h5py.File('whatev.h5')
```

Create a dataset - behaves like an array:

```python
f.create_dataset('name', (100,100), dtype='i') # second arg is shape
```

So that you can slice it etc

```python
f['name'][0:50]
```



