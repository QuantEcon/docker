FROM quantecon/base
USER root 

# Sundials, NLopt, MPI, Nemo, Cairo
RUN apt-get update && apt-get install -y --no-install-recommends \
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

# User 
USER $NB_UID

# QELAP 
RUN julia -e "using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLectureAllPackages\", tag = \"v0.9.1\")"

