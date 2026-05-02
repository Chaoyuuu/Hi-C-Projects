# HiC_breakfinder Docker Implementation Guide
This project provides a dockerfile for running hic_breakfinder in easy.
To learn how to use hic_breakfinder, please check out the original repo: https://github.com/dixonlab/hic_breakfinder

## 🚀 Build with docker
1. Build the Docker Image:
```bash
docker build -t hic_breakfinder:1.0 .
```

2. Verify installation:
```bash
docker run --rm hic_breakfinder:1.0 --help
```
You should see something like this:
```bash
unalee@biodepot-uwt:~/Hi-C-Project/Hic-Breakfinder$ docker run --rm hic_breakfinder:1.0 --help
hic_breakfinder: unrecognized option '--help'
./hic_breakfinder

Required options:

--bam-file [input bam file]
--exp-file-inter [Inter-chromosomal 1Mb expectation file]
--exp-file-intra [Intra-chromosomal 100kb expectation file]
--name [output file name prefix, will append with *.super_matrix.txt and *.SR.txt]
```

3. Usage
Input Requirements: You need three primary files for a successful run:
- **BAM file**: Filtered Hi-C alignments.
- **Inter-chromosomal Expectations**: Expected background noise between chromosomes.
- **Intra-chromosomal Expectations**: Expected background noise within chromosomes.

Place your input files in a local directory (e.g., `~/my_data`) and run:
```bash
docker run --rm -v ~/my_data:/data hic_breakfinder:1.0 \
  -b /data/input.bam \
  -e /data/inter_expectations.txt \
  -i /data/intra_expectations.txt \
  -o /data/output_results.txt
```

## 🧪 (Option) Manual Installation (Linux/Ubuntu)
If you prefer to run locally, follow these steps:

1. Install Eigen3:
```bash
sudo apt update && sudo apt install libeigen3-dev zlib1g-dev
```

2. Build BamTools:
```bash
git clone https://github.com/pezmaster31/bamtools
mkdir -p bamtools/build && cd bamtools/build
cmake -DCMAKE_INSTALL_PREFIX=$HOME/.local ..
make && make install
```

3. Build hic_breakfinder
```bash
git clone https://github.com/dixonlab/hic_breakfinder.git
cd hic_breakfinder
./configure CXXFLAGS="-fpermissive -O3" \
            CPPFLAGS="-I$HOME/.local/include/bamtools -I/usr/include/eigen3" \
            LDFLAGS="-L$HOME/.local/lib" \
            LIBS="-lbamtools -lz -lpthread"
make && sudo make install
```
