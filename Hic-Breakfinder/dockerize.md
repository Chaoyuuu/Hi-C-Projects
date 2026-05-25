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
docker run --rm -it -v ./test_data:/app hic_breakfinder
```
You should see something like this:
```bash
unalee@biodepot-uwt:~/Hi-C-Project/Hic-Breakfinder$ docker run --rm -it -v ./hic-breakfinder-data:/app hic_breakfinder

./hic_breakfinder

Required options:

--bam-file [input bam file]
--exp-file-inter [Inter-chromosomal 1Mb expectation file]
--exp-file-intra [Intra-chromosomal 100kb expectation file]
--name [output file name prefix, will append with *.super_matrix.txt and *.SR.txt]
```
Verify with testing data:
Step1: Run `mkdir hic-breakfinder-data && cd hic-breakfinder-data` 
Step2: Downlaod three files `K562_in_house_b38d5.nodup.bam`, `intra_expect_100kb.hg38.txt` and `inter_expect_1Mb.hg38.txt` from https://salkinstitute.app.box.com/s/m8oyv2ypf8o3kcdsybzcmrpg032xnrgx it into `/hic-breakfinder-data`
Step2: Run
```bash
# Start container
unalee@biodepot-uwt:~/Hi-C-Project/Hic-Breakfinder$ docker run --rm -it -v ./hic-breakfinder-data:/app hic_breakfinder

# Run hic-breakfinder
hic_breakfinder \
  --bam-file K562_in_house_b38d5.nodup.bam \
  --exp-file-inter inter_expect_1Mb.hg38.txt \
  --exp-file-intra intra_expect_100kb.hg38.txt \
  --name demo
```
If success, you can see files start with `demo` in the `/hic-breakfinder-data` directory.
```
(base) unalee@biodepot-uwt:~/Hi-C-Project/Hic-Breakfinder/test-data$ ls
demo_100kb.bias_vector.txt        demo_100kb.super_matrix.txt  demo_10kb.super_matrix.norm.txt  demo_1Mb.super_matrix.norm.eig.txt  intra_expect_100kb.hg38.txt
demo_100kb.breaks.txt             demo_10kb.bias_vector.txt    demo_10kb.super_matrix.txt       demo_1Mb.super_matrix.norm.txt      K562_in_house_b38d5.nodup.bam
demo_100kb_intra.breaks.txt       demo_10kb.breaks.txt         demo_1Mb.bias_vector.txt         demo_1Mb.super_matrix.txt
demo_100kb.SR.txt                 demo_10kb_intra.breaks.txt   demo_1Mb.breaks.txt              demo.breaks.txt
demo_100kb.super_matrix.norm.txt  demo_10kb.SR.txt             demo_1Mb.SR.txt                  inter_expect_1Mb.hg38.txt
```


3. Usage
Input Requirements: You need three primary files for a successful run:
- **BAM file**: Filtered Hi-C alignments.
- **Inter-chromosomal Expectations**: Expected background noise between chromosomes.
- **Intra-chromosomal Expectations**: Expected background noise within chromosomes.

> Hic-Breaker has provided **Inter-chromosomal Expectations** and **Intra-chromosomal Expectations** files.
> You can download from: https://salkinstitute.app.box.com/s/m8oyv2ypf8o3kcdsybzcmrpg032xnrgx

Place your input files in a local directory (e.g., `/hic-breakfinder-data`) and run this in the docker bash:
```bash
docker run --rm -it -v ./hic-breakfinder-data:/app hic_breakfinder
`
Than run:
```bash
hic_breakfinder \
  --bam-file {INPUT_BAM} \
  --exp-file-inter {INTER_FILE} \
  --exp-file-intra {INTRA_FILE} \
  --name {OUTPUT_PREFIX}
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
