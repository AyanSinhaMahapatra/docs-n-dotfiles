## Conda Commands Cheatsheet

1. Create Environment

   `conda create --name venv-conda python=3.6`

2. Activate/Deactivate

	`conda activate py35`
	`deactivate`

3. Create environment from a .yml file and saving an environment to file

	`conda env create -f environment.yml`
	`conda env export > environment.yml`

4. List all packages

	`conda list`

5. List Revisions and go back to a revision 

	`conda list --revisions`
	`conda install --revision 2`

6. Save and Create environment to/from a text file

	`conda list --explicit > env.txt`
	`conda env create --file env.txt`

7. Search package

	`conda search PACKAGENAME`

8. Delete environment

	`conda env remove --name bio-env`

9. Install/Update package (from a a channel)

	`conda install -c conda-forge PACKAGENAME`
	`conda update PACKAGENAME`

10. Version Numbers

	Fuzzy 						numpy=1.11 				1.11.0, 1.11.1, 1.11.2 etc.
	Exact 						numpy==1.11 			1.11.0 
	Greater than or equal to 	"numpy>=1.11" 			1.11.0 or higher
	OR 							"numpy=1.11.1|1.11.3" 	1.11.1, 1.11.3
	AND 						"numpy>=1.8,<2" 		1.8, 1.9, not 2.0
