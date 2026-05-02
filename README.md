# Hi-C-Project

This project aims to explore and containerize software for structural variant calling based on raw contact matrices. 
In this repository, we dockerize the HiSV and hic_breakfinder. Please read the following instruction to run these project in docker.

## HiSV
- Paper: Li et al. (2023). HiSV: A control-free method for structural variation detection from Hi-C data. https://arc.net/l/quote/mhpfnyvn. 
- Github: https://github.com/GaoLabXDU/HiSV

### Start
please refer to `/HiSV` in this repo and follow steps in `dockerize.md`.

> We have modified the source code to make it support general use cases because we identified several hard-coded file paths and names in the original repo. Recommand to use the code provided in /HiSV this repo to build your Docker image.

## Hic_Breakfinder
- Paper:  Song et al. (2022). Analysis of Hi-C Data for Discovery of Structural Variations in Cancer. https://pmc.ncbi.nlm.nih.gov/articles/PMC9890901/?utm_source=chatgpt.com. 
- Github: https://github.com/dixonlab/hic_breakfinder


### Start
please refer to `/HiC-Breakfinder` in this repo and follow steps in `dockerize.md`.