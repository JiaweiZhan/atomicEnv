# atomicEnv

**atomicEnv** is a project designed to address a common problem in quantum chemistry: assigning uniform grid points to different atoms based on a predefined radius (ATOMIC ENVironment). This is particularly useful for tasks such as calculating the magnetic moment per atom site, where the local magnetic moment is defined as the integral of the spin density within a sphere around the atom site.


## Problem Statement
A brute-force approach to this problem involves looping over all grid points and checking if each point lies within the sphere of each atom. This approach has a time complexity of O(N^2), where N is the number of grid points, assuming the number of atoms is often proportional to the number of grid points. This becomes inefficient when dealing with a large number of grid points.


## Solution
A more efficient solution involves expressing each grid point in orthogonal coordinate systems and defining a small region around each atom. By looping over grid points within this region and computing their distances to the atom, we can determine if a point lies within the predefined radius. This approach reduces the time complexity to O(N).

## Installation
To install atomicEnv, clone the repository and run:
```bash
python setup.py
```
Then, add the current folder to the `PYTHONPATH`

## Example Usage:
Here's a simple example to demonstrate the usage of atomicEnv:
```python
from atomicEnv import atomicBox
import numpy as np

cell = np.eye(3) * 10     # define a cubic PBC with 10 a.u. on each dimension.
fftw = (200, 200, 200)    # define uniform grid
atom_pos = np.array([5.0, 5.0, 5.0])  # specify the position of atom in cartesion.
rcut = 2                  # define cutoff radius for atomic environment
ab = atomicBox(cell, fftw, atom_pos, rcut)
idx = ab.compute_idx()    # idx of grid points within atomicEnv [ngrid_in_atomicEnv, 3]
ngrid_AE = idx.shape[0]
print(f"ngrid: {ngrid}; {ngrid_AE / np.prod(fftw) * 100:.2f}%")
```
