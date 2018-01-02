# Run with the DockerSpawner in JupyterHub

ARG BASE_IMAGE=jupyter/scipy-notebook
FROM $BASE_IMAGE
MAINTAINER i <ibelyalov@yandex.ru>

ADD install_jupyterhub /tmp/install_jupyterhub
ARG JUPYTERHUB_VERSION=master
# install pinned jupyterhub and ensure notebook is installed
RUN python3 /tmp/install_jupyterhub && \
python3 -m pip install notebook

# Install jupyterlab
RUN conda install -c conda-forge jupyterlab
RUN jupyter serverextension enable --py jupyterlab --sys-prefix

USER root

# python 3 environment is named root
RUN conda install --name root \
    rpy2 nbgrader 

USER jovyan
# RUN conda install --name root 
# python 2 environement is named python2
# RUN conda install --name python2\
#     pymongo
 
RUN conda install r-essentials
RUN conda update --all

# install libraries via pip using bash and activating respective environment
RUN ["bash", "-c", "source activate root && pip install twitter"]
# RUN ["bash", "-c", "source activate python2 && pip install twitter "]

RUN Rscript -e "IRkernel::installspec()"
RUN chown -R jovyan /home/jovyan/.local 
RUN mkdir /home/jovyan/shared && \
	chown -R jovyan:users /home/jovyan/shared && \
	chmod 777 /home/jovyan/shared
RUN ls -l /home/jovyan

# USER jovyan

RUN mkdir /home/jovyan/.jupyter
ADD nbgrader_config.py /home/jovyan/.jupyter
