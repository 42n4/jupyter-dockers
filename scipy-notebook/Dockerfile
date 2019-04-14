# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
ARG BASE_CONTAINER=42n4/minimal-notebook
FROM $BASE_CONTAINER

LABEL maintainer="Jupyter Project <jupyter@googlegroups.com>"

USER root

# ffmpeg for matplotlib anim
RUN apt-get update && \
    apt-get install -y --no-install-recommends ffmpeg libyaml-dev && \
    rm -rf /var/lib/apt/lists/*

USER $NB_UID

#numpy=1.14.5
#opencv-python=4.0.0.21
#prometheus-client=0.3.1
#tensorboard=1.10.0
#tensorflow=1.10.0
#    'nbconvert=5.4.1' \
#    'nbformat=4.4.0' \
#    'pyzmq=17.1.2' \
#    'tensorflow==2.0.0-alpha0' \
#    'ipytest=0.5.0' \
#    'mlxtend=0.15.0.0' \
#    'more-itertools=7.0.0' \
#    'Markdown=3.1' \
#    'grpcio=1.19.0' \

# Install Python 3 packages
# Remove pyqt and qt pulled in for matplotlib since we're only ever going to
# use notebook-friendly backends in these images
RUN conda install --quiet --yes \
    'backcall=0.1.0' \
    'bleach=3.1.0' \
    'certifi=2019.3.9' \
    'chardet=3.0.4' \
    'cycler=0.10.0' \
    'decorator=4.4.0' \
    'entrypoints=0.3' \
    'gast=0.2.2' \
    'html5lib=1.0.1' \
    'idna=2.8' \
    'ipykernel=5.1.0' \
    'ipython=7.4.0' \
    'ipython_genutils=0.2.0' \
    'jedi=0.13.3' \
    'Jinja2=2.10.1' \
    'jsonschema=3.0.1' \
    'kiwisolver=1.0.1' \
    'lesscpy=0.13.0' \
    'lxml=4.3.3' \
    'MarkupSafe=1.1.1' \
    'mistune=0.8.4' \
    'Keras=2.2.4' \
    'Keras-Applications=1.0.7' \
    'Keras-Preprocessing=1.0.9' \
    'pandas-datareader=0.7.0' \
    'pandocfilters=1.4.2' \
    'parso=0.4.0' \
    'pathlib2=2.3.3' \
    'pexpect=4.7.0' \
    'pickleshare=0.7.5' \
    'pluggy=0.9.0' \
    'ply=3.11' \
    'ptyprocess=0.6.0' \
    'py=1.8.0' \
    'Pygments=2.3.1' \
    'pyparsing=2.4.0' \
    'python-dateutil=2.8.0' \
    'pytz=2018.9' \
    'PyYAML=5.1' \
    'requests=2.21.0' \
    'Send2Trash=1.5.0' \
    'simplegeneric=0.8.1' \
    'six=1.12.0' \
    'termcolor=1.1.0' \
    'terminado=0.8.2' \
    'testpath=0.4.2' \
    'tornado=6.0.2' \
    'tqdm=4.31.1' \
    'traitlets=4.3.2' \
    'urllib3=1.24.1' \
    'wcwidth=0.1.7' \
    'webencodings=0.5.1' \
    'Werkzeug=0.15.2' \
    'widgetsnbextension=3.4.2' \
    'wrapt=1.11.1' \
    'absl-py=0.7.1' \
    'astor=0.7.1' \
    'atomicwrites=1.3.0' \
    'attrs=19.1.0' \
    'conda-forge::blas=*=openblas' \
    'ipywidgets=7.4*' \
    'pandas=0.24*' \
    'numexpr=2.6*' \
    'matplotlib=3.0*' \
    'scipy=1.2*' \
    'seaborn=0.9*' \
    'scikit-learn=0.20*' \
    'scikit-image=0.14*' \
    'sympy=1.3*' \
    'cython=0.29*' \
    'patsy=0.5*' \
    'statsmodels=0.9*' \
    'cloudpickle=0.8*' \
    'dill=0.2*' \
    'dask=1.1.*' \
    'numba=0.42*' \
    'bokeh=1.0*' \
    'sqlalchemy=1.3*' \
    'hdf5=1.10*' \
    'h5py=2.9*' \
    'vincent=0.4.*' \
    'beautifulsoup4=4.7.*' \
    'protobuf=3.7.*' \
    'xlrd'  && \
    conda remove --quiet --yes --force qt pyqt && \
    conda clean -tipsy && \
    # Activate ipywidgets extension in the environment that runs the notebook server
    jupyter nbextension enable --py widgetsnbextension --sys-prefix && \
    # Also activate ipywidgets extension for JupyterLab
    # Check this URL for most recent compatibilities
    # https://github.com/jupyter-widgets/ipywidgets/tree/master/packages/jupyterlab-manager
    jupyter labextension install @jupyter-widgets/jupyterlab-manager@^0.38.1 && \
    jupyter labextension install jupyterlab_bokeh@0.6.3 && \
    npm cache clean --force && \
    rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
    rm -rf /home/$NB_USER/.cache/yarn && \
    rm -rf /home/$NB_USER/.node-gyp && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# Install facets which does not have a pip or conda package at the moment
RUN cd /tmp && \
    git clone https://github.com/PAIR-code/facets.git && \
    cd facets && \
    jupyter nbextension install facets-dist/ --sys-prefix && \
    cd && \
    rm -rf /tmp/facets && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME /home/$NB_USER/.cache/
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions /home/$NB_USER

USER $NB_UID
