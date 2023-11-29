FROM jupyter/base-notebook

# mamba is better conda
RUN mamba install -y -c conda-forge \ 
  numpy \
  matplotlib \
  biorbd \
  bioviz \
  python-graphviz \
  jupyter_contrib_nbextensions \
  ipywidgets \
  pyqtgraph \
  GitPython \
  git 
  

# Perform bioptim setup steps. I am installing it from source as I ran into problem when using the
# conda. It should be the same version though.
RUN cd /home/jovyan && \
    git clone https://github.com/pyomeca/bioptim.git && \
    cd bioptim && \
    python setup.py install


USER root

# bioptim needs opengl for visualization
RUN apt-get update -y && apt-get install -y libgl1-mesa-dev

USER ${NB_UID}


# remove default folder
RUN rmdir /home/jovyan/work
