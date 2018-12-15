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

# Sundials, NLopt, MPI, Nemo, Cairo
RUN apt-get update && apt-get install -y --no-install-recommends \
    g++ \
    libsundials-dev \
    libnlopt0 \
    libnlopt-dev \
    openmpi-bin \
    libopenmpi-dev \
    m4 \
    yasm \
    libacl1-dev \
    gettext \
    zlib1g-dev \
    libffi-dev \
    libpng-dev \
    libpixman-1-dev \
    libpoppler-dev \
    librsvg2-dev \
    libcairo2-dev \
    libpango1.0-0 \
    tk-dev \
    pkg-config \
    curl \
  && apt-get clean && rm -rf /var/lib/apt/lists/*

# ipopt
RUN mkdir -p /opt/ipopt \
  && curl -s -L http://www.coin-or.org/download/source/Ipopt/Ipopt-3.12.10.tgz | \
     tar -C /opt/ipopt -x -z --strip-components=1 -f - \
  && cd /opt/ipopt/ThirdParty/Blas && ./get.Blas \
  && ./configure --prefix=/usr/local --disable-shared --with-pic \
  && make install \
  && cd /opt/ipopt/ThirdParty/Lapack && ./get.Lapack \
  && ./configure --prefix=/usr/local --disable-shared --with-pic \
  && make install \
  && cd /opt/ipopt/ThirdParty/Mumps && ./get.Mumps \
  && cd /opt/ipopt/ThirdParty/ASL && ./get.ASL && cd /opt/ipopt \
  && env LIBS='-lgfortran' ./configure --prefix=/usr/local \
    --enable-dependency-linking \
  && make install && echo "/usr/local/lib" > /etc/ld.so.conf.d/ipopt.conf \
  && ldconfig && rm -rf /opt/ipopt

# Julia setup
# JULIA_PKGDIR will soon be deprecated; keep an eye on the jupyter upstream to see the new standard
ENV JULIA_PKGDIR=/opt/julia
ENV JULIA_VERSION=1.0.2
ENV PACKAGEVERSION=v0.9.5

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
RUN julia -e "using Pkg; pkg\"add Expectations DifferentialEquations Compat IJulia Interpolations Revise REPL QuantEcon InstantiateFromURL Plots GR Parameters Distributions\"; pkg\"build\"; pkg\"precompile\"" && \
    pip install xlrd
# QELAP
RUN julia -e "using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLectureAllPackages\", tag = \"${PACKAGEVERSION}\")" && \
    julia -e "using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLectureAllPackages\")"

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
