# Copyright (c) QuantEcon Development Team
# Distributed under the terms of the Modified BSD License.
# Modified by QuantEcon Development Team

FROM quantecon/base

USER root

# R pre-requisites
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    fonts-dejavu \
    gfortran \
    gcc && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

USER $NB_USER

# R packages including IRKernel which gets installed globally.
# Pin r-base to a specific build number for https://github.com/jupyter/docker-stacks/issues/210#issuecomment-246081809
RUN conda config --add channels r && \
   conda install --quiet --yes \
   'rpy2=2.8*' \
   'r-base=3.3.1 1' \
   'r-irkernel=0.6*' \
   'r-plyr=1.8*' \
   'r-devtools=1.11*' \
   'r-dplyr=0.4*' \
   'r-ggplot2=2.1*' \
   'r-tidyr=0.5*' \
   'r-shiny=0.13*' \
   'r-rmarkdown=0.9*' \
   'r-forecast=7.1*' \
   'r-stringr=1.0*' \
   'r-rsqlite=1.0*' \
   'r-reshape2=1.4*' \
   'r-nycflights13=0.2*' \
   'r-caret=6.0*' \
   'r-rcurl=1.95*' \
   'r-randomforest=4.6*' \
    && conda clean -tipsy
RUN conda install -c r r-essentials