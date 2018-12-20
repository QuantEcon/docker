FROM julia:1.0.2-stretch
ARG PACKAGEVERSION=v0.9.5

# Linux dependencies
# Sundials, NLopt, MPI, Nemo, Cairo
RUN apt-get update && apt-get install -y --no-install-recommends \
    g++ \
    gfortran \ 
    python3-dev \
    python3-pip \
    software-properties-common \
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
    wget \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Python stuff.
RUN pip3 install xlrd

# Julia stuff.
RUN julia -e "using Pkg; pkg\"add InstantiateFromURL\"; using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLectureAllPackages\", tag = \"$PACKAGEVERSION\")" && \
    cp .projects/QuantEconLectureAllPackages-$PACKAGEVERSION/Project.toml ~/.julia/environments/v1.0 && \
    cp .projects/QuantEconLectureAllPackages-$PACKAGEVERSION/Manifest.toml ~/.julia/environments/v1.0

# Cleanup.
