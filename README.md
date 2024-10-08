# LAMMPS_Phonopy
Calculating second order derivative potential energy (force constant) using Phonopy with LAMMPS ChIMES Force Field MD simultion !!!

1. Create unitcell for lammps input, the atom_style should be atomic. Need to add the code in your structure file in order to follow Phonopy convention.
```
Atom Type Labels

1 C
2 H
```

2. Relax the structure using Lammps (relax.in)

3. Run Phonopy to create supercell structure
   
```
phonopy --lammps -c relax.lmp -d --dim 4 6 2
```

4. Create sub-folders to put supercell into the sub-folder (using parallel).
```
parallel "mkdir {} && mv supercell-{} {}/supercell" ::: {001..xyz}
```

5. Copy lammps.in file to each subfolder
```
parallel 'cd {} && cp ../force.in . && cd ..' ::: {001..xyz}
```

6. Before running Lammps, you have to modify the supercell file. Because ChIMES version of Lammps is 2020 and is not support "Atom Type Label". Therefore, you have to change this part back to

```
Masses

1 12.0107 # C
2 1.00794 # H
```

You have to run this to quickly make it.

```
parallel 'cd {} && sed -i "s/Atom Type Labels/Masses/g" supercell && cd ..' ::: {001..xyz}
```
```
parallel 'cd {} && sed -i "12,17 s/1 H/1 12.0107 # C/" supercell && cd ..' ::: {001..xyz}
```
```
parallel 'cd {} && sed -i "12,17 s/2 C/2 1.00794 # H/" supercell && cd ..' ::: {001..xyz}
```
```
parallel 'cd {} && sed -i "s/C/1/g" supercell && cd ..' ::: {001..xyz}
```
```
parallel 'cd {} && sed -i "s/H/2/g" supercell && cd ..' ::: {001..xyz}
```

7. Run lammps.in simulations at each folders (At NERSC, there is ChIMES version of Lammps installed using Docker) 
```
parallel --delay .5 'cd {} && shifter --image docker:nersc/lammps_chimes:20.10 lmp -in force.in && cd ..' ::: {001..xyz}
```

8. Create FORCE_SETS using Phonopy
```
phonopy -f {001..072}/force.0
```
