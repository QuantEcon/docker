
# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
# Modified by QuantEcon Development Team

FROM jupyter/scipy-notebook

MAINTAINER QuantEcon Project <admin@quantecon.org>

USER root

# Julia dependencies
RUN apt-get update \
    && apt-get upgrade -y -o Dpkg::Options::="--force-confdef" -o DPkg::Options::="--force-confold" \
    && apt-get install -y \
    man-db \
    libc6 \
    libc6-dev \
    build-essential \
    wget \
    curl \
    file \
    vim \
    screen \
    tmux \
    unzip \
    pkg-config \
    cmake \
    gfortran \
    gettext \
    libreadline-dev \
    libncurses-dev \
    libpcre3-dev \
    libgnutls30 \
    libzmq3-dev \
    libzmq5 \
    python \
    python-yaml \
    python-m2crypto \
    python-crypto \
    msgpack-python \
    python-dev \
    python-setuptools \
    supervisor \
    python-jinja2 \
    python-requests \
    python-isodate \
    python-git \
    python-pip \
    iputils-ping \
    dnsutils \
    net-tools \
    inetutils-traceroute \
    hdf5-tools \
    libnlopt0 \
    && apt-get clean

# Julia Installation (Latest Binaries)
RUN mkdir -p $HOME/Downloads && cd $HOME/Downloads && \
    wget -O julia_binary.tar.gz https://julialang-s3.julialang.org/bin/linux/x64/0.6/julia-0.6.0-linux-x86_64.tar.gz && \
    mkdir -p $HOME/bin/juliav6 && \
    tar -C $HOME/bin/juliav6 -zxf julia_binary.tar.gz --strip-components=1 && \
    rm julia_binary.tar.gz && \
    ln -s $HOME/bin/juliav6/bin/julia /usr/bin/julia

USER $NB_USER

# Install IJulia packages as jovyan and then move the kernelspec out
# to the system share location. Avoids problems with runtime UID change not
# taking effect properly on the .local folder in the jovyan home dir.
RUN julia -e 'Pkg.add("IJulia")' && \
    mv /home/$NB_USER/.local/share/jupyter/kernels/julia* $CONDA_DIR/share/jupyter/kernels/ && \
    chmod -R go+rx $CONDA_DIR/share/jupyter

# Show Julia where conda libraries are
# Add essential packages
#RUN echo "push!(Sys.DL_LOAD_PATH, \"$CONDA_DIR/lib\")" > /home/$NB_USER/.juliarc.jl && \

#Add Julia Packages
#&& julia -e 'Pkg.update("Plots")'
RUN julia -e 'Pkg.add("Plots")'  && julia -e 'using Plots'
RUN julia -e 'Pkg.add("NLopt")' && julia -e 'using  NLopt'
RUN julia -e 'Pkg.add("QuantEcon")' && julia -e 'using QuantEcon'
RUN julia -e 'Pkg.add("PlotlyJS")' && julia -e 'using PlotlyJS'
RUN julia -e 'Pkg.add("BenchmarkTools")' && julia -e 'using BenchmarkTools'
RUN julia -e 'Pkg.add("PyPlot")' && julia -e 'using PyPlot'
RUN julia -e 'Pkg.add("Interpolations")' && julia -e 'using Interpolations'
RUN julia -e 'Pkg.add("Optim")'  && julia -e 'using Optim'
RUN julia -e 'Pkg.add("StatsBase")'  && julia -e 'using StatsBase'
RUN julia -e 'Pkg.add("KernelDensity")' && julia -e 'using KernelDensity'

# Install Python Libraries
RUN conda config --add channels conda-forge && conda install --yes quantecon joblib line_profiler
RUN pip install linearmodels