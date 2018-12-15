FROM julia:1.0.2-stretch

ARG PACKAGEVERSION=v0.9.5

# Linux dependencies
# Sundials, NLopt, MPI, Nemo, Cairo
RUN apt-get update && apt-get install -y --no-install-recommends \
    g++ \
    python3-dev \
    python3-pip \
    software-properties-common \
    libsundials-serial-dev \
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
    wget \
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
  && ldconfig &&  rm -rf /opt/ipopt

RUN pip3 install xlrd

# Julia stuff.
RUN julia -e "using Pkg; pkg\"add InstantiateFromURL\"; using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLectureAllPackages\", tag = \"$PACKAGEVERSION\")" && \
    cp .projects/QuantEconLectureAllPackages-$PACKAGEVERSION/Project.toml ~/.julia/environments/v1.0 && \
    cp .projects/QuantEconLectureAllPackages-$PACKAGEVERSION/Manifest.toml ~/.julia/environments/v1.0 && \
    julia -e "using Pkg; pkg\"add DifferentialEquations\""
