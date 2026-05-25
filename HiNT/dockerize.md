# HiNT Docker Implementation Guide
- original repo: https://github.com/parklab/HiNT

## Issue
Running HiNT requires a background matrix, which is one of the input files created by the HiNT team. However, the [reference link](https://github.com/parklab/HiNT#download-reference-files-used-in-hint-here) is currently unavailable. The authors mentioned that they developed a new method **EagleC**, and as a result, they no longer support HiNT. To learn more, please check: https://github.com/parklab/HiNT/issues/18



## 🚀 Build and Run
The orignial repo provided the docker version
1. Build the Docker Image:
```bash
docker pull suwangbio/hint
```

2. Start
```bash
docker run -it -v /path/to/your/data:/data suwangbio/hint /bin/bash
```
Note: It take 30s to start running the container. If you see the bash, it means the container is running successfully. Now you can start `hint` in this bash.

Test installation, type `hint` in the bash. You should see something like this:
```bash
unalee@biodepot-uwt:~$ docker run -it -v /path/to/your/data:/data suwangbio/hint /bin/bash
root@f93567112f8a:/usr/local/bin# hint <---- container start, type `hint`

usage: hint [-h] [--version] {pre,cnv,tl} ...

HiNT --- Hic for copy Number variations and Translocations detection

positional arguments:
  {pre,cnv,tl}  sub-command help
    pre         HiNT for Hi-C data preprocessing: raw Hi-C --> HiC contact
                matrix and chimeric read pairs
    cnv         Copy Number Vairations detection from Hi-C
    tl          Identify translocated chromosomal pairs, and detect the
                breakpoints in 100kb as well as 1bp resolution

optional arguments:
  -h, --help    show this help message and exit
  --version     show program's version number and exit

For command line options of each command, type: hint COMMAND -h
```