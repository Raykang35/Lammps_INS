# LAMMPS_INS
This is for LAMMPS MD simultion using ChIMES Force Field

1. Create unitcell for lammps input, the atom_style should be atomic. Need to add the code in your structure file in order to follow Phonopy convention.
```
Atom Type Labels

1 C
2 H
```
2. Run Phonopy to create supercell structure
   
```
phonopy --lammps -c anthracene_chimes111.lmp -d --dim 4 6 2
```

Create sub-folders to put supercell into the sub-folder (using parallel).
```
parallel "mkdir {} && mv supercell-{} {}/supercell" ::: {001..xyz}
```
