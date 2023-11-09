# Biomechanics Lab WS23 

Python code for the Biomechanics Lab WS23

## Option A: Use the docker container

**WARNING: This is only works on Linux**

For that docker have to be installed on your system. For Ubuntu you would use apt:

```bash
sudo apt install docker.io
```

Or follow the [official tutorial](https://docs.docker.com/engine/install/ubuntu/) to get the newest
version. It's also recommended to follow the first 3 steps of the [post-installation
tutorial](https://docs.docker.com/engine/install/linux-postinstall/) (non-root access). Also don't
forget to start your docker daemon (on Ubuntu it's `systemctl start docker`).

Now you can build the docker container for the exercises:

```bash
cd $THIS_REPO_PATH
docker build docker -t biomechanics-lab
```

Once it's built you can start the container. Depending on your setup you might have to call `xhost +` before 
to allow access to the x-socket.

**Jupyter Lab:**

```bash
docker run \
      -p 8888:8888 \
      -e DISPLAY=$DISPLAY \
      -e JUPYTER_ENABLE_LAB=yes \
      -e JUPYTER_TOKEN=docker \
      --rm \
      -v $THIS_REPO_PATH:/home/jovyan/exercises \
      -v /tmp/.X11-unix:/tmp/.X11-unix \
      -it biomechanics-lab
```

**Interactive Shell (bash):**

```bash
docker run \
      -e DISPLAY=$DISPLAY \
      --rm \
      -v $THIS_REPO_PATH:/home/jovyan/exercises \
      -v /tmp/.X11-unix:/tmp/.X11-unix \
      -it biomechanics-lab bash
cd bioptim/bioptim/examples/getting_started
python3 custom_dynamics.py
```

A short explanation for the docker commands:

- `-p 8888:8888` - forward the port to the host
- `-e DISPLAY=$DISPLAY` - use the same display in the container as on the host
- `--rm` - one-off container (deletes after use)
- `-v $THIS_REPO_PATH:/home/jovyan/exercises` - mount the exercise folder in the container
- `-v /tmp/.X11-unix:/tmp/.X11-unix` - mount the x-socket for gui

## Option B: Set it up yourself

For that you have to download and install miniconda on you own machine.

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
# probably have to restart your shell to source conda or do a source ~/.bashrc

# create the environment
conda create --name bioptim

# activate the environment
conda activate bioptim

# install deps
conda install -y -c conda-forge \ 
  jupyterlab \
  numpy \
  matplotlib \
  biorbd \
  bioviz \
  python-graphviz \
  jupyter_contrib_nbextensions \
  ipywidgets \
  pyqtgraph

# install bioptim from source
cd $YOUR_WORKING_DIR
git clone https://github.com/pyomeca/bioptim.git
cd bioptim
python setup.py install
```

You can now run your jupyter lab using

```bash
# activate the environment
conda activate bioptim
jupyter-lab
```

Similarly for the examples provided by `bioptim`.

```bash
conda activate bioptim
cd $YOUR_WORKING_DIR/bioptim/bioptim/examples/getting_started
python3 custom_dynamics.py
```
