FROM julia:1.1.0-stretch

ARG PACKAGEVERSION=master
RUN julia -e "using Pkg; pkg\"add InstantiateFromURL\"; using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLecturePackages\", tag = \"$PACKAGEVERSION\", add_default_environment = true)"
