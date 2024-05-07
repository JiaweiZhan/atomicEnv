# assignGrid
This project is to deal with a simple scenario in quantum chemistry, where one want to assign 
different grid points to different atoms based on a predefined radius. This could be useful in, for example, 
calculating the magnetic moment per atom site, where the local magnetic moment is defined as the integral of the spin density 
in a sphere around the atom site.


To tackle this problem, one brute force way is to loop over all grid points and check if it is within the sphere of each atom. 
And the time complexity is O(N^2), where N is the number of grid points. This is not efficient when the number of grid points is large. 


I would however express each grid point under orthogonal coordinate systems, and then define a small region around each atom. Then we can
loop over grid points within that region and compute the distance to the atom. If the distance is smaller than the predefined radius, then we assign the grid point to the atom. 
This is much more efficient than the brute force way, and the time complexity is O(N).

## Installation
After cloning the repository, run:
```bash
python setup.py
```

## Example:
```python
from assignGrid import assignGrid
import numpy as np
cell = np.eye(3) * 10
fftw = (200, 200, 200)
atom_pos = np.array([5.0, 5.0, 5.0])
rcut = 2
assign_grid = assignGrid(cell, fftw, atom_pos, rcut)
idx = assign_grid.compute_idx()
l, m, n = idx.T
grid = np.zeros(fftw, dtype=int)
grid[l, m, n] = 1
```
