FROM quantecon/lectures-base:latest
MAINTAINER me@arnavsood.com 
USER quantecon 

# So we can build on all sorts of machines (i.e. Xeon processors)
ARG OPENBLAS_CORETYPE=HASWELL

# Install Julia
ARG JULIA_VERSION=1.2.0
RUN sudo apt-get update && sudo  apt-get install wget
RUN cd ${HOME}/utils && mkdir julia-${JULIA_VERSION} && cd julia-${JULIA_VERSION} && \ 
    wget -q https://julialang-s3.julialang.org/bin/linux/x64/`echo ${JULIA_VERSION} | cut -d. -f 1,2`/julia-${JULIA_VERSION}-linux-x86_64.tar.gz && \
    echo "926ced5dec5d726ed0d2919e849ff084a320882fb67ab048385849f9483afc47 *julia-${JULIA_VERSION}-linux-x86_64.tar.gz" | sha256sum -c - && \
    tar xzf julia-${JULIA_VERSION}-linux-x86_64.tar.gz --strip-components=1 && \
    rm *.tar.gz
RUN sudo ln -fs ${HOME}/utils/julia-*/bin/julia /usr/local/bin/julia

# Install Jupinx
RUN /home/quantecon/utils/conda/bin/pip install sphinxcontrib-jupyter sphinxcontrib.bibtex 

# Add Julia Packages
RUN julia -e "using Pkg; pkg\"add InstantiateFromURL IJulia\""
RUN julia -e "using InstantiateFromURL; github_project(\"QuantEcon/quantecon-notebooks-julia\"); packages_to_default_environment()"
