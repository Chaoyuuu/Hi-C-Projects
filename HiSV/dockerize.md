# HiSV Docker Implementation Guide
This repository provides a Dockerized environment for HiSV, optimized for general support and ease of use. This version includes custom modifications to `coolToMatrix.py` for improved compatibility.

## 🚀 Getting Started
1. Build the Docker Image
Navigate to the project root and build the image:
```bash
docker build -t hisv-docker .
```
2. Prepare Data Directories
Create two local directories to handle file I/O. These will be mapped to the Docker container:
- `./data`: Place your input files here (e.g., .mcool, chromosome size files).
- `./result`: Analysis outputs (e.g., SV matrices and calling results) will be saved here.

3. Run the Container
Launch an interactive shell within the container while mounting your data and result folders:
```bash
docker run -it --rm \
  -v ./data:/app/data \
  -v ./result:/app/result \
  hisv-docker
```
Note: Using --rm ensures the container is cleaned up after you exit.

## 🧪 Example Execution Workflow
Once inside the container shell, follow these steps to process your data:

### Step 1: Convert mcool to Matrix
Use hiccovert to transform your raw Hi-C data into a processable matrix format:

```bash
hiccovert \
  --hic_file ./data/SRR11016318.mcool \
  --binsize 50000 \
  --format mcool \
  --ref ./data/chr.sizes \
  --output ./result \
  --name SRR11016318 \
  --cores 20
```
### Step 2: Identify Structural Variants (SVs)
Once the matrices are generated, run the core HiSV script:

```bash
HiSV \
  --inter_hic ./result/Inter_matrix \
  --intra_hic ./result/Intra_matrix \
  --ref ./data/chr.sizes \
  --output ./result \
  --name SRR11016318 \
  --cores 20
```

## 📝 Technical Notes & Modifications
- Upstream Source: This project is a fork of the original GaoLabXDU/HiSV repository.
https://github.com/GaoLabXDU/HiSV/tree/main

- Key Changes:
    - `coolToMatrix.py`: Modified to provide more generalized support for different .mcool structures.

- Files descriptions:
    - `/scripts`: Contains the executable entry points for the four primary commands provided by HiSV. These scripts act as the interface for starting analysis workflows.

    - `/hisv`: The core package directory containing the underlying logic, algorithmic implementations, and supporting modules for the tool.

    - `/HiSV_test_data`: A collection of sample datasets provided by the original authors (GaoLabXDU) for testing and validating the installation.
