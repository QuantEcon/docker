# docker

Docker Containers for QuantEcon

https://img.shields.io/docker/automated/quantecon-services/base.svg

## Notes

To build the base image you will need to do one of the following:

1. Comment out the line `COPY drive.json
   /home/jovyan/.jupyter/lab/user-settings/@jupyterlab/google-drive/drive.json`
   in `base/Dockerfile`
2. Create a file `base/drive.json` with one JSON object containing a single
   key `clientId` that you obtained by following the steps in the
   [jupyterlab-google-drive
   readme](https://github.com/jupyterlab/jupyterlab-google-drive). For
   quantecon collaborators, we have a copy of this file stored in a secret
   gist. Please email Spencer to ask for the file
