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

3. Create sub-folders to put supercell into the sub-folder (using parallel).
```
parallel "mkdir {} && mv supercell-{} {}/supercell" ::: {001..xyz}
```

4. Copy lammps.in file to each subfolder
```
parallel 'cd {} && cp ../lammps.in . && cd ..' ::: {001..xyz}
```

5. Run lammps.in simulations at each folders (At NERSC, there is ChIMES version of Lammps installed using Docker) 
```
parallel 'cd {} && shifter --image docker:nersc/lammps_chimes:20.10 lmp -in lammps.in && cd ..' ::: {001..xyz}
```
