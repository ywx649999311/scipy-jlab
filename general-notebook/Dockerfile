ARG BASE_CONTAINER=jupyter/scipy-notebook
FROM $BASE_CONTAINER

LABEL maintainer="Weixiang Yu <astro.flyfish@google.com>"

## Install linux packages needed for JLab extensions
USER root

RUN apt-get update \
    && apt-get install -yq --no-install-recommends \
    libx11-xcb1 \
    libxtst6 \
    libxrandr2 \
    libasound2 \
    libpangocairo-1.0-0 \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    libgtk-3-0 \
    libnss3 \
    libxss1 \
    tree \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# switch back jovyan
USER $NB_UID

## Install Extra JLab extensions
# install noteboo-as-pdf
RUN python -m pip install -U notebook-as-pdf && \
    pyppeteer-install && \
    pip install --upgrade --quiet --no-cache-dir \
    jupyterlab \
    jupyterlab-git && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# install other extension
RUN jupyter labextension install @jupyterlab/toc --no-build && \
    jupyter labextension install @ijmbarr/jupyterlab_spellchecker --no-build && \
    jupyter labextension install jupyterlab-code-snippets --no-build && \
    jupyter lab build -y && \
    jupyter lab clean -y && \
    npm cache clean --force && \
    rm -rf "/home/${NB_USER}/.cache/yarn" && \
    rm -rf "/home/${NB_USER}/.node-gyp" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

## clone/setup configs
WORKDIR $HOME
RUN git clone https://github.com/ywx649999311/configs.git && \
    cd configs && \
    /bin/bash -c "bash setup.sh" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"