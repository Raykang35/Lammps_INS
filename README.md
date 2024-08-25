# LAMMPS_INS
This is for LAMMPS MD simultion using ChIMES Force Field

Create sub-folders to put supercell into the sub-folder (using parallel).
```
parallel "mkdir {} && mv supercell-{} {}/supercell" ::: {001..xyz}
```
