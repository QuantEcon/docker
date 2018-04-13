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
    wget --quiet -O julia_binary.tar.gz https://julialang-s3.julialang.org/bin/linux/x64/0.6/julia-0.6.2-linux-x86_64.tar.gz && \
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

USER $NB_USER

# Add Julia packages
# Install IJulia as jovyan and then move the kernelspec out
# to the system share location. Avoids problems with runtime UID change not
# taking effect properly on the .local folder in the jovyan home dir.
RUN julia -e 'Pkg.init()' && \
    julia -e 'Pkg.update()' && \
    julia -e 'Pkg.add("HDF5")' && \
    julia -e 'Pkg.add("Gadfly")' && \
    julia -e 'Pkg.add("RDatasets")' && \
    julia -e 'Pkg.add("IJulia")' && \
    julia -e 'using HDF5' && \
    julia -e 'using Gadfly' && \
    julia -e 'using RDatasets' && \
    julia -e 'using IJulia' && \
    mv $HOME/.local/share/jupyter/kernels/julia* $CONDA_DIR/share/jupyter/kernels/ && \
    chmod -R go+rx $CONDA_DIR/share/jupyter && \
    rm -rf $HOME/.local && \
    fix-permissions $JULIA_PKGDIR $CONDA_DIR/share/jupyter

RUN julia -e 'Pkg.add("PlotlyJS"); using PlotlyJS' && \
    julia -e 'Pkg.add("BasisMatrices"); using BasisMatrices' && \
    julia -e 'Pkg.add("Interpolations"); using Interpolations' && \
    julia -e 'Pkg.add("MINPACK"); using MINPACK' && \
    julia -e 'Pkg.add("NLsolve"); using NLsolve' && \
    julia -e 'Pkg.add("Optim"); using Optim' && \
    julia -e 'Pkg.add("GR"); using GR' && \
    julia -e 'Pkg.add("Plots"); using Plots' && \
    julia -e 'Pkg.add("Memento"); using Memento' && \
    julia -e 'Pkg.add("Parameters"); using Parameters' && \
    julia -e 'Pkg.add("BenchmarkTools"); using BenchmarkTools' && \
    julia -e 'Pkg.add("JLD"); using JLD'

RUN julia -e 'Pkg.add("YAML"); using YAML' && \
    julia -e 'Pkg.add("Distances"); using Distances' && \
    julia -e 'Pkg.add("Plotly"); Pkg.checkout("Plotly"); Pkg.checkout("Plotly", "sl/api_v2"); using Plotly' && \
    julia -e 'Pkg.add("Colors"); using Colors' && \
    julia -e 'Pkg.add("Dolang"); using Dolang'

# RUN julia -e 'ENV["PYTHON"]="$(ENV["CONDA_DIR"])/bin/python"; Pkg.add("PyPlot"); using PyPlot'

RUN conda install --yes quantecon joblib line_profiler && \
    pip install linearmodels

# install some jupyterlab extensions
RUN jupyter labextension install @jupyterlab/github @jupyterlab/plotly-extension @jupyterlab/json-extension @jupyterlab/geojson-extension

COPY nbconfig.json /home/jovyan/.jupyter/nbconfig/notebook.json
