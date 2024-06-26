# Copyright (c) Bo Peng and the University of Texas MD Anderson Cancer Center
# Distributed under the terms of the 3-clause BSD License.

# SoS official docker image for latest version of SoS. Use command
#
#     docker build -t vatlab/sos-notebook:latest docker-notebook
#
# to build it.
#

# tag created in Fev 2019
FROM jupyter/datascience-notebook:python-3.11

LABEL maintainer="Bo Peng <Bo.Peng@bcm.edu>"

USER    root

#       Tools
RUN     apt-get update
RUN     apt-get install -y graphviz zlib1g-dev libbz2-dev libcurl4-openssl-dev libssl-dev
RUN     apt-get install -y texlive-xetex texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended
RUN     apt-get install -y octave
RUN     octave --eval 'pkg install -forge dataframe'

RUN     apt-get install -y npm vim libgmp3-dev software-properties-common
RUN     apt-get install -y libtool libffi-dev ruby ruby-dev make  libzmq3-dev libczmq-dev

# Install some packages for our examples
RUN     conda update conda -y
RUN 	mamba install pip -y
RUN	mamba install -c conda-forge r-base -y
RUN     mamba install -c conda-forge -y r-arrow r-glmnet r-biocmanager
RUN 	R -e "install.packages(c('Quandl', 'NasdaqDataLink', 'fpp3'), dependencies=TRUE, repos='http://cran.rstudio.com/')"

#	ruby
RUN     gem install ffi-rzmq
RUN     gem install iruby --pre
RUN     gem install daru nmatrix
RUN     iruby register --force

RUN     cd /home/jovyan; chown -R jovyan ../jovyan
USER    jovyan

RUN     pip3 install scikit-learn

#       Bash
RUN     pip3 install bash_kernel
RUN     python3 -m bash_kernel.install --user

#       Octave
RUN     pip3 install octave_kernel
RUN     python3 -m octave_kernel install --user

#       JavaScript
RUN     npm rebuild
RUN     npm install -g ijavascript
RUN     ijsinstall --spec-path=full

#       Julia
RUN     julia -e "using Pkg;Pkg.add([\"Feather\", \"DataFrames\", \"NamedArrays\"])"

RUN     mamba install -y pyarrow -c conda-forge


#	SOS
RUN     pip3 install docker markdown wand graphviz imageio pillow nbformat

# 	Trigger rerun for sos updates
ARG	DUMMY=unknown
RUN     DUMMY=${DUMMY} pip3 install sos sos-notebook sos-r sos-julia sos-python sos-matlab sos-bash sos-ruby sos-javascript sos-bioinfo markdown-kernel
RUN     pip3 install jupyter-contrib-core transient-display-data jupyterlab-sos
RUN     jupyter lab build --dev-build=False --minimize=False

#       Markdown kernel
RUN     python3 -m markdown_kernel.install --prefix /opt/conda/
RUN     python3 -m sos_notebook.install --prefix /opt/conda/

ENV     JUPYTER_ENABLE_LAB TRUE

USER    jovyan

