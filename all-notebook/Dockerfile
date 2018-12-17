# Setup 
FROM quantecon/notebook  
LABEL maintainer="QuantEcon Project <admin@quantecon.org>"
ARG PACKAGEVERSION=v0.9.5

# Install base packages for the v1.0 environment 
RUN julia -e "using Pkg; pkg\"add Expectations Compat Interpolations Revise REPL QuantEcon InstantiateFromURL Plots GR Parameters Distributions\"; pkg\"build\"; pkg\"precompile\""

# Tagged version of QELP 
RUN julia -e "using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLectureAllPackages\", tag = \"${PACKAGEVERSION}\")"

# Master QELP
RUN julia -e "using InstantiateFromURL; activate_github(\"QuantEcon/QuantEconLectureAllPackages\")"
