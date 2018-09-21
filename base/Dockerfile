# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
# Modified by QuantEcon Development Team

FROM jupyter/scipy-notebook

LABEL maintainer="QuantEcon Project <admin@quantecon.org>"

RUN conda install pandas plotly quantecon jupyter_contrib_nbextensions && \
pip install cufflinks

ENV JULIA_PKGDIR=/opt/julia/pkg \
    PATH=/opt/julia/bin:$PATH

USER root

# install julia
RUN cd /tmp && \
    wget --quiet -O julia_binary.tar.gz https://julialang-s3.julialang.org/bin/linux/x64/1.0/julia-1.0.0-linux-x86_64.tar.gz && \
    rm -rf /opt/julia/src && \
    mkdir -p /opt/julia/src && \
    tar -C /opt/julia/src -zxf julia_binary.tar.gz --strip-components=1 && \
    rm julia_binary.tar.gz && \
    rm -f /opt/julia/bin/julia && \
    mkdir -p /opt/julia/bin && \
    ln -s /opt/julia/src/bin/julia /opt/julia/bin/julia && \
    mkdir -p /usr/etc/julia && \
    echo 'push!(Libdl.DL_LOAD_PATH, "$CONDA_DIR/lib")' >> /usr/etc/julia/juliarc.jl && \
    mkdir -p $JULIA_PKGDIR && \
    chown $NB_USER $JULIA_PKGDIR && \
    fix-permissions $JULIA_PKGDIR

RUN apt-get update && apt-get install curl wget && rm -rf /var/lib/apt/lists/*

USER $NB_USER

RUN mkdir -p /home/$NB_USER/.julia/environments/v1.0
COPY Project.toml /home/$NB_USER/.julia/environments/v1.0/Project.toml
COPY Manifest.toml /home/$NB_USER/.julia/environments/v1.0/Manifest.toml
RUN julia -e 'using Pkg; pkg"instantiate"; pkg"precompile"'

RUN mv $HOME/.local/share/jupyter/kernels/julia* $CONDA_DIR/share/jupyter/kernels/ && \
    chmod -R go+rx $CONDA_DIR/share/jupyter && \
    rm -rf $HOME/.local && \
    fix-permissions $JULIA_PKGDIR $CONDA_DIR/share/jupyter

# RUN julia -e 'ENV["PYTHON"]="$(ENV["CONDA_DIR"])/bin/python"; Pkg.add("PyPlot"); using PyPlot'

RUN conda install --yes quantecon joblib line_profiler && \
    pip install linearmodels

# install some jupyterlab extensions
RUN jupyter labextension install @jupyterlab/github @jupyterlab/plotly-extension @jupyterlab/json-extension @jupyterlab/geojson-extension

COPY nbconfig.json /home/jovyan/.jupyter/nbconfig/notebook.json
