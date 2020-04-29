## These are Tensor knownledge in Torch7
### General knowledge
- A **Tensor** is simply a view on a specific underlying ***Storage*** (== **C** array)
- The assignment between **Tensor**s is simply a copy of its reference
    > *Example:*
    ````
    t = torch.Tensor(2,3,4)
    r = torch.DoubleTensor(t):resize(3,8)
    #r -- View size of r
    #t -- View size of t
    r:zero()
    r
    t
    s = t -- Assignment
    #s
    #t
    s.resize(4,6)
    #t
    u = t:clone()
    u:random()
    t
    ````
### Tensors types
--- 
**1. List all Tensor types**

- Here are all Tensor types
    - **ByteTensor**: contains unsigned chars
    - **CharTensor**: contains signed chars
    - **IntTensor**: contains ints
    - **LongTensor**: contains longs
    - **FloatTensor**: contains floats
    - **DoubleTensor**: contains doubles

**2. View default Tensor type**
```
torch.type(torch.Tensor(1,2,3))
```

**3. Chang default tensor type**

````
torch.setdefaulttensortype('torch.FloatTensor')
````

**4. Vector in tensor**
- Vector creation
	````
	v = torch.Tensor{1,2,3,4}
	````

    > **v** is vector with 4 elements as 4 rows, just like 4x1 matrix

- Vector resize
	````
	v.resize(4)
	````

    > **v** is now a vector with 4 elements as 4 columns, just like 1x4 matrix