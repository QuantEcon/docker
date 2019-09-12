# Setup (latest for Julia 1.1 and bugfixes; the repo rarely has significant changes)
FROM jupyter/datascience-notebook:latest
LABEL maintainer="QuantEcon Project <admin@quantecon.org>"
ARG PACKAGEVERSION=master

# Tagged version of QELP
RUN julia -e "using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLecturePackages\", tag = \"${PACKAGEVERSION}\", add_default_environment = true)"
