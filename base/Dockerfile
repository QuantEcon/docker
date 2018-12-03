# Copyright (c) Jupyter Development Team. 
# Distributed under the terms of the Modified BSD License. 
# Modified by the QuantEcon Development Team. 

# This contains the minimal set of assets needed to run the lectures and develop in Julia/Python.
# If you need more, consider pulling QuantEcon/all-notebook.

# Setup instructions 
# base-notebook is the smallest one
FROM jupyter/base-notebook 
LABEL maintainer="QuantEcon Project <admin@quantecon.org>"
USER root 

# Linux setup 
RUN apt-get update && apt-get install -yq --no-install-recommends \
    build-essential \ 
    git \ 
    texlive-latex-base \
    texlive-xetex \
    pandoc \ 
    libsm6 \
    libxext-dev \
    libxrender1 \
    inkscape && \ 
    apt-get clean && \ 
    rm -rf /var/lib/apt/lists/*

# Julia setup 
# JULIA_PKGDIR will soon be deprecated; keep an eye on the jupyter upstream to see the new standard
ENV JULIA_PKGDIR=/opt/julia
ENV JULIA_VERSION=1.0.2

# Install Julia 
RUN mkdir /opt/julia-${JULIA_VERSION} && \
    cd /tmp && \
    wget -q https://julialang-s3.julialang.org/bin/linux/x64/`echo ${JULIA_VERSION} | cut -d. -f 1,2`/julia-${JULIA_VERSION}-linux-x86_64.tar.gz && \
    tar xzf julia-${JULIA_VERSION}-linux-x86_64.tar.gz -C /opt/julia-${JULIA_VERSION} --strip-components=1 && \
    rm /tmp/julia-${JULIA_VERSION}-linux-x86_64.tar.gz
RUN ln -fs /opt/julia-*/bin/julia /usr/local/bin/julia

# Show Julia where conda libraries are
RUN mkdir /etc/julia && \
    echo "push!(Libdl.DL_LOAD_PATH, \"$CONDA_DIR/lib\")" >> /etc/julia/juliarc.jl && \
    # Create JULIA_PKGDIR \
    mkdir $JULIA_PKGDIR && \
    chown $NB_USER $JULIA_PKGDIR && \
    fix-permissions $JULIA_PKGDIR

USER $NB_UID

# Install Julia packages 
# Base packages
RUN julia -e "using Pkg; pkg\"add Expectations Compat IJulia Interpolations Revise REPL QuantEcon InstantiateFromURL Plots GR Parameters Distributions\"; pkg\"build\"; pkg\"precompile\"" 
# v0.9.0 QELP
RUN julia -e "using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLecturePackages\", tag = \"v0.9.4\")"
# Master QELP 
RUN julia -e "using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLecturePackages\")"

# Mount the user depot locally. 
VOLUME /home/jovyan/.julia/

# Install Python packages (gives us quantecon, numba, pandas, scipy/numpy, hdf5, matplotlib...)
RUN conda install --yes quantecon && \ 
    pip install linearmodels && \ 
    conda clean --all

# Jupyter kernelspec stuff 
USER root 
RUN mv $HOME/.local/share/jupyter/kernels/julia* $CONDA_DIR/share/jupyter/kernels/ && \
    chmod -R go+rx $CONDA_DIR/share/jupyter && \
    rm -rf $HOME/.local && \
    fix-permissions $JULIA_PKGDIR $CONDA_DIR/share/jupyter

COPY nbconfig.json /home/jovyan/.jupyter/nbconfig/notebook.json
USER $NB_UID
