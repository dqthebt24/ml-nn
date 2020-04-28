# Install Torch7 with CUDA10

## Install newest CMake
1. Clone CMake source and build from source
````
$git clone https://github.com/Kitware/CMake.git
$cd CMake/
$./bootstrap; make; sudo make install
````
2. If have issue with OpenSSL, do bellow steps:
    - Install it using `$sudo apt-get install libssl-dev` 
    - Run command `$./bootstrap; make; sudo make install` again
## Install Torch7 
1. Install the latest CMake from github repo (the latest FindCUDA.cmake will be installed)

````
$ sudo apt-get purge cmake
$ git clone https://github.com/Kitware/CMake.git
$ cd CMake
$ ./bootstrap; make; sudo make install
````

2. Remove `FindCUDA.cmake.`

````
$ cd ~/torch
$ rm -fr cmake/3.6/Modules/FindCUDA*
````

- Apply the following patch to cutorch

````nano ~/Desktop/atomic.patch````
3. Copy bellow contents to the file

````
diff --git a/lib/THC/THCAtomics.cuh b/lib/THC/THCAtomics.cuh
index 400875c..ccb7a1c 100644
--- a/lib/THC/THCAtomics.cuh
+++ b/lib/THC/THCAtomics.cuh
@@ -94,6 +94,7 @@ static inline __device__ void atomicAdd(long *address, long val) {
 }
 
 #ifdef CUDA_HALF_TENSOR
+#if !(__CUDA_ARCH__ >= 700 || !defined(__CUDA_ARCH__) )
 static inline  __device__ void atomicAdd(half *address, half val) {
   unsigned int * address_as_ui =
       (unsigned int *) ((char *)address - ((size_t)address & 2));
@@ -117,6 +118,7 @@ static inline  __device__ void atomicAdd(half *address, half val) {
    } while (assumed != old);
 }
 #endif
+#endif
````
````
$ cd extra/cutorch
$ patch -p1 < ~/Desktop/atomic.patch
````
4. Build
````
$ ./clean.sh
$ export TORCH_NVCC_FLAGS="-D__CUDA_NO_HALF_OPERATORS__"
$ ./install.sh
````

5. Test

    Type command `$th`
