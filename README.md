# TransPPImd
**Exploring the conformational space of protein-protein complexes with transformer-based generative neural networks**  

Protein-protein interactions are the basis of many protein functions, and understanding the contact and conformational changes of protein-protein interactions is crucial for linking protein structure to biological function. Although difficult to detect experimentally, molecular dynamics (MD) simulations are widely used to study the conformational space and dynamics of protein-protein complexes, but there are significant limitations in sampling efficiency and computational costs. In this study, a generative neural network was trained on protein-protein complex conformations obtained from molecular simulations to directly generate new conformations with physical realism. We demonstrated the use of a deep learning model based on the transformer architecture to explore the conformational space of protein-protein complexes through MD simulations. The results showed that the learned latent space can be used to generate unsampled conformations of protein-protein complexes for obtaining new conformations complementing pre-existing ones, which can be used as an exploratory tool for the analysis and enhancement of molecular simulations of protein-protein complexes.


## Acknowledgements
We thank the authors of C5T5: Controllable Generation of Organic Molecules with Transformers and IUPAC2Struct: Transformer-based artificial neural networks for the conversion between chemical notations for releasing their code. The code in this repository is based on their source code release (https://github.com/dhroth/c5t5 and https://github.com/sergsb/IUPAC2Struct). If you find this code useful, please consider citing their work.


## Requirements
```python
Python==3.7
biopython==1.7.9
openmm==7.7.0
mdanalysis==2.1.0
mdtraj==1.9.7
pytraj==2.0.6
modeller==10.2
nglview==3.0.3
tensorflow==2.8.0
```


## Molecular dynamics

### prpepare  topology and coordinate files
```python
pdb4amber -i A.pdb -o a.pdb -y -d

pdb4amber -i a.pdb -o inputA.pdb --reduce

pdb4amber -i B.pdb -o b.pdb -y -d

pdb4amber -i b.pdb -o inputB.pdb --reduce


************tleap.in**********************************************
source leaprc.protein.ff14SB #Source leaprc file for ff14SB protein force field
source leaprc.water.tip3p #Source leaprc file for TIP3P water model

ProA = loadpdb inputA.pdb #Load A PDB file for protein
ProB = loadpdb inputB.pdb #Load B PDB file for protein

mol = combine {ProA,ProB}

check mol
solvatebox mol TIP3PBOX 12.0 #Solvate the complex with a cubic water box
addions mol Cl- 0 #Add Cl- ions to neutralize the system
addions mol Na+ 0 #Add Na+ ions to neutralize the system
charge mol
savepdb mol ppcomplex_solv.pdb
saveamberparm mol ppcomplex_solv.prmtop ppcomplex_solv.inpcrd #Save AMBER topology and coordinate files
quit #Quit tleap program
**********************************************************************

tleap -s -f tleap.in > tleap.out

```

### run rolecular dynamics

```python

python run_openmm_simulation.py

```


## Model Metrics

### DockQ
DockQ: A Quality Measure for Protein-Protein Docking Models

https://github.com/bjornwallner/DockQ

*  Basu, Sankar, and Björn Wallner. "DockQ: a quality measure for protein-protein docking models." PloS one 11, no. 8 (2016): e0161879.

## License
Code is released under MIT LICENSE.


## Cite:



