FROM julia:1.0.2-stretch

ARG PACKAGEVERSION=v0.9.5
RUN julia -e "using Pkg; pkg\"add InstantiateFromURL\"; using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLecturePackages\", tag = \"$PACKAGEVERSION\")" && \
    cp .projects/QuantEconLecturePackages-$PACKAGEVERSION/Project.toml ~/.julia/environments/v1.0 && \
    cp .projects/QuantEconLecturePackages-$PACKAGEVERSION/Manifest.toml ~/.julia/environments/v1.0
