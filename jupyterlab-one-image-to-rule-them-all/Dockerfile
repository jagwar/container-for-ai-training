FROM gcr.io/kaggle-gpu-images/python:v98

RUN chsh -s /bin/bash
ENV SHELL=/bin/bash
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN apt-get update && apt-get install -y \
    man \
    vim \
    nano \
    htop \
    curl \
    wget \
    rsync \
    ca-certificates \
    git \
    zip \
    procps \
    ssh \
    supervisor \
    gettext-base \
    && rm -rf /var/lib/apt/lists/*

# install nvm
# https://github.com/creationix/nvm#install-script
RUN curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash

ENV NVM_DIR /root/.nvm
ENV NODE_VERSION v12.20.1

# install node and npm
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/versions/node/$NODE_VERSION/bin
ENV PATH $NODE_PATH:$PATH

RUN pip install pip==20.3.4
RUN pip install jupyterlab==2.2.9 ipywidgets==7.6.3
RUN jupyter labextension install @jupyter-widgets/jupyterlab-manager
RUN jupyter nbextension enable --py widgetsnbextension #enable ipywidgets

#install xlrd zarr imbalanced-learn python module
RUN pip install xlrd
RUN pip install zarr
RUN pip install imbalanced-learn

# Add R to Jupyter Kernel
RUN conda install -y -c r r-irkernel

EXPOSE 8080

ADD start.sh /

WORKDIR /workspace
RUN chown -R 42420:42420 /workspace

ENTRYPOINT ["/start.sh"]