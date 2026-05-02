FROM continuumio/miniconda3:latest

RUN conda config --add channels defaults && \
    conda config --add channels conda-forge && \
    conda config --add channels bioconda

RUN conda config --set remote_connect_timeout_secs 180.0 && \
    conda config --set remote_read_timeout_secs 180.0 && \
    conda config --set ssl_verify false

RUN conda create -n HiSV python=3.7 -y
RUN conda run -n HiSV pip install hisv -U

RUN apt-get update && apt-get install -y \
    gcc \
    g++ \
    make \
    libffi-dev \
    libbz2-dev \
    liblzma-dev \
    zlib1g-dev \
    libcurl4-openssl-dev \
    && rm -rf /var/lib/apt/lists/*

RUN conda run -n HiSV pip install --upgrade pip setuptools wheel
RUN conda run -n HiSV pip install hic-straw

WORKDIR /app
COPY ./hisv ./hisv
COPY ./scripts ./scripts
COPY ./setup.py ./setup.py
RUN conda run -n HiSV pip install .

RUN conda init bash
RUN echo "conda activate HiSV" >> ~/.bashrc
ENTRYPOINT ["/bin/bash"]