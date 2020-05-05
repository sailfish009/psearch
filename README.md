# PSearch - 3D ligand-based pharmacophore modeling

PSearch is a tool to generate 3D ligand-based pharmacophore models and perform virtual screening with them.

## Installation

```bash
pip install psearch
```

## Dependency

`pmapper >= 0.4.0`

## Example

### Creation of ligand-based pharmacophore models
It is recommended to create an empty dir which would be your `$PROJECT_DIR` and copy an input file to that location.  
There are two steps of pharmacophore model generation.  

1. Generation of a database with precomputed conformers and pharmacophores. 

```python
gen_db -i input.smi -o input.dat -c 4
```
`-i` - path to the input SMILES file
`-o` - path to database (should have extension .dat)  
`-c` - number of CPUs to use  
There are other arguments which one can tune. Invoke script with `-h` key to get full information.  

The script takes as input a tab-separated SMILES file containing `SMILES`, `compound id`, `activity` columns. 
The third column should contain a word `active` or `inactive`.
The script generates stereoisomers and conformers, creates the database of compounds with labeled pharmacophore features.  

2. Model building.  

```python
psearch -p $PROJECT_DIR -i input.smi -d input.dat -c 4
```
`-p` - path to the project dir where output and intermediate files will be stored
`-i` - path to the input SMILES file
`-d` - path to the database generated on the previous step  
`-c`- number of CPUs to use

Other arguments are available at the command line.

This will create several dirs within `$PROJECT_DIR`. `models` contains generated pharmacophore models. File names use the following naming convention: t<train_set_number>_f<number_of_features>_p<sequentional_model_number>.pma. `results` contains a file with validation statistics (molecules which were not used to train particular models are used as corresponding test sets). `screen` contains results of screening of test set compounds. `trainset` contains training sets. `files` contain intermediate files generated by the program.  

### Virtual screening with pharmacophore models 

1. Database creation using the same procedure as described above. 
 
2. Virtual screening.
  
```python
screen_db -d compounds.db -q $PROJECT_DIR/models/ -o screen_results/ -c 4
```
`-d` - input generated database  
`-q` - pharmacophore model or models or a directory with models. If a directory would be specified all pma- and xyz-files will be recognized as pharmacophores and will be used for screening.  
`-o` - path to an output directory if multiple models were supplied for screening or a path to a text/sdf file    
`-c`- number of CPUs to use

If multiple models are used for screening and sdf output is desired a user should add `-z` argument which will force output format to be sdf.

## Documentation

All scripts have `-h` argument to retrieve descriptions of all available options and arguments.

## Authors
Alina Kutlushina, Pavel Polishchuk

## Citation
Ligand-Based Pharmacophore Modeling Using Novel 3D Pharmacophore Signatures  
Alina Kutlushina, Aigul Khakimova, Timur Madzhidov, Pavel Polishchuk  
*Molecules* **2018**, 23(12), 3094  
https://doi.org/10.3390/molecules23123094

## License
BSD-3 clause
