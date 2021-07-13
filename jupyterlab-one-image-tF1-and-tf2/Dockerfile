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
    less \
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
RUN jupyter labextension install jupyterlab-plotly@4.14.3

#install xlrd python module
RUN pip install pyradiomics
RUN pip install xlrd==1.2.0
RUN pip install zarr
RUN pip install imbalanced-learn
RUN pip install openpyxl 
RUN pip install efficientnet-pytorch
RUN pip install monai
RUN pip install prince
RUN pip install vit-pytorch
RUN pip install lifelines==0.25.11
RUN pip install timm==0.3.2
RUN pip install keras-retinanet==1.0.0
RUN python -m pip install histomicstk --find-links https://girder.github.io/large_image_wheels
RUN pip install luminoth ipympl pysurvival missingpy pyinform pingouin pyAgrum missingno autoimpute networkx community yellowbrick factor_analyzer hdbscan pyitlib
#RUN pip install ipympl
#RUN pip install pysurvival
#RUN pip install missingpy
#RUN pip install pyinform
#RUN pip install pingouin
#RUN pip install pyAgrum
#RUN pip install missingno
#RUN pip install autoimpute
#RUN pip install networkx
#RUN pip install community
#RUN pip install yellowbrick
#RUN pip install factor_analyzer
#RUN pip install hdbscan
#RUN pip install pyitlib
RUN pip install eli5 dtreeviz gower batchgenerators mlinsights efficientnet-pytorch pretrainedmodels
#RUN pip install dtreeviz
#RUN pip install gower
#RUN pip install batchgenerators
#RUN pip install mlinsights
#RUN pip install efficientnet-pytorch
#RUN pip install pretrainedmodels
# Add R to Jupyter Kernel
RUN conda install -y -c r r-irkernel

#Install survival, sm, ggplot2, Hmisc, mixOmics (ce dernier est en repositoire Bioconductor)
#RUN conda install -y -c conda-forge r-rcpp
#RUN conda install -y -c conda-forge r-survival 
#RUN conda install -y -c conda-forge r-sm
#RUN conda install -y -c conda-forge r-ggplot2 
#RUN conda install -y -c conda-forge r-hmisc
#RUN conda install -y -c conda-forge r-mixomics

RUN conda install -y -c cran r-survival 
RUN conda install -y -c cran r-sm
RUN conda install -y -c cran r-ggplot2 
RUN conda install -y -c cran r-hmisc
RUN conda install -y -c cran r-mixomics
RUN conda install -y -c cran r-caret
RUN conda install -y -c cran r-survminer
RUN conda install -y -c cran r-ggfortify
RUN conda install -y -c cran r-wordcloud
RUN conda install -y -c cran r-tm
RUN conda install -y -c cran r-prioritylasso
RUN conda install -y -c cran r-blockforest
RUN conda install -y -c cran r-mice
#RUN conda install -y -c conda-forge r-mixomics

#### tensorflow 1
# Create the environment:
SHELL ["/bin/bash", "-c"]
COPY tfenv.yml .
RUN conda env create -f tfenv.yml
SHELL ["conda", "run", "-n", "tf1", "/bin/bash", "-c"]
RUN python -m ipykernel install --name=tensorflow1


EXPOSE 8080

ADD start.sh /

WORKDIR /workspace
RUN chown -R 42420:42420 /workspace

ENTRYPOINT ["/start.sh"]
