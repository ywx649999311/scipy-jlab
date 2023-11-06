ARG REGISTRY=quay.io
ARG OWNER=jupyter
ARG BASE_CONTAINER=$REGISTRY/$OWNER/scipy-notebook
FROM --platform=linux/arm64 $BASE_CONTAINER

LABEL maintainer="Weixiang Yu <astro.flyfish@gmail.com>"

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
    wget \
    curl \
    pandoc \
    texlive texlive-latex-extra \
    latexmk \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# switch back jovyan
USER $NB_UID

# Packages: notebook-as-pdf
RUN python -m pip install -U notebook-as-pdf && \
    pyppeteer-install && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Packages: pip
RUN python -m pip install -U black && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"    

# Extension: install with mamba
RUN mamba install --quiet --yes -c conda-forge \
    jupyterlab \
    jupyter-resource-usage \
    jupyterlab-git \
    jupyterlab-spellchecker \
    jupyterlab_code_formatter && \
    # jupyterlab-lsp && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Other extensions: install with pip
RUN pip install --upgrade --quiet --no-cache-dir \
    jupyterlab-code-snippets \
    aquirdturtle_collapsible_headings \
    lckr-jupyterlab-variableinspector \
    # jupyterlab-topbar \
    # jupyterlab-system-monitor && \
    jupyter-resource-usage && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"    

## clone/setup configs
WORKDIR $HOME
RUN git clone https://github.com/ywx649999311/configs.git && \
    cd configs && \
    /bin/bash -c "bash setup_remote_bash.sh" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"