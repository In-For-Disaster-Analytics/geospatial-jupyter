FROM quay.io/jupyter/pytorch-notebook:pytorch-2.1.1
 # FROM r2dhttps-3a-2f-2fgithub-2ecom-2fin-2dfor-2ddisaster-2danalytics-2fsites-2dand-2dstories-2dnlp8484276:latest


# The user must be swtiched to root in order to install and update packages with apt-get.
# See https://github.com/jupyter/docker-stacks/blob/master/base-notebook/Dockerfile for info.
USER root
run wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb &&\
    sudo dpkg -i cuda-keyring_1.1-1_all.deb &&sudo apt-get update && \
    sudo apt-get -y install cuda-toolkit-12-3
RUN apt-get update && apt-get install -y \
    ssh \
    vim \
    curl \
    && rm -rf /var/lib/apt/lists/*

COPY run.sh /tapis/run.sh

RUN chmod +x /tapis/run.sh

# The user is switched back to the one from set in the base image.
# USER 1000
# arg GH_USER=In-For-Disaster-Analytics 
# arg GH_REPO=sites-and-stories-nlp 
# arg GH_BRANCH=jupyterenv 

# RUN wget https://github.com/$GH_USER/$GH_REPO/archive/refs/heads/$GH_BRANCH.zip 
# RUN unzip *.zip 
# # RUN rm $GH_REPO-$GH_BRANCH.zip
# RUN conda env create -n llm -f $GH_REPO-$GH_BRANCH/.binder/environment.yml
# SHELL ["conda","run","-n","llm","/bin/bash","-c"]

# RUN python -m ipykernel install --user --name llm --display-name "Python (llm)"

# #New Code

ENTRYPOINT [ "/tapis/run.sh" ]
