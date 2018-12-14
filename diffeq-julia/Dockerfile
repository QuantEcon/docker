FROM julia:1.0.2-stretch

ARG PACKAGEVERSION=v0.9.5
RUN julia -e "using Pkg; pkg\"add InstantiateFromURL\"; using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLectureAllPackages\", tag = \"$PACKAGEVERSION\")" && \
    cp .projects/QuantEconLectureAllPackages-$PACKAGEVERSION/Project.toml ~/.julia/environments/v1.0 && \
    cp .projects/QuantEconLectureAllPackages-$PACKAGEVERSION/Manifest.toml ~/.julia/environments/v1.0 && \
    julia -e "using Pkg; pkg\"add DifferentialEquations\""
