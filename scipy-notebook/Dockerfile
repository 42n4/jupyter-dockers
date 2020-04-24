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



# Install Python 3 packages
# Remove pyqt and qt pulled in for matplotlib since we're only ever going to
# use notebook-friendly backends in these images
RUN conda install --quiet --yes \
    'absl-py=0.9.0' \
    'astor' \
    'atomicwrites=1.3.0' \
    'attrs=19.3.0' \
    'backcall=0.1.0' \
    'beautifulsoup4=4.9.*' \
    'bleach=3.1.4' \
    'bokeh=2.0*' \
    'branca=0.4.0' \
    'catboost=0.22' \
    'certifi' \
    'chardet=3.0.4' \
    'cloudpickle=1.3*' \
    'conda-forge::blas=*=openblas' \
    'cufflinks-py' \
    'cycler=0.10.0' \
    'cython=0.29*' \
    'dask=2.14*' \
    'decorator=4.4.2' \
    'dill=0.3*' \
    'entrypoints=0.3' \
    'ffmpeg' \
    'folium=0.10.1' \
    'gast=0.3.3' \
    'graphviz' \
    'grpcio' \
    'h5py=2.*' \
    'hdf5=1.10*' \
    'html5lib=1.0.1' \
    'idna=2.9' \
    'ipykernel' \
    'ipytest' \
    'ipython=7.13.0' \
    'ipython_genutils=0.2.0' \
    'ipywidgets=7.5*' \
    'jedi' \
    'Jinja2' \
    'joblib=0.14.1' \
    'jsonschema=3.2.0' 

RUN conda install --quiet --yes \
    'Keras=2.3.1' \
    'Keras-Applications=1.0.8' \
    'Keras-Preprocessing=1.1.0' \
    'kiwisolver=1.1.0' \
    'lesscpy' \
    'lightgbm=2.3.1' \
    'lxml=4.5.0' \
    'markdown' \
    'MarkupSafe=1.1.1' \
    'mistune=0.8.4' \
    'matplotlib=3.2.1' \
    'mlxtend=0.17.2' \
    'nbconvert' \
    'nbformat' \
    'numba' \
    'numexpr=2.7*' \
    'numpy=1.18.1' \
    'opencv' \
    'pandas=1.0.1' \
    'pandas-datareader=0.8.1' \
    'pandocfilters=1.4.2' \
    'parso' \
    'patsy=0.5*' \
    'pexpect=4.8.0' \
    'pathlib2=2.3.5' \
    'pexpect=4.8.0' \
    'pickleshare=0.7.5' \
    'plotly=4.5.4' \
    'plotly_express' \
    'pluggy=0.13.1' \
    'ply=3.11' \
    'prompt_toolkit' \
    'protobuf=3.11.*' \
    'ptyprocess=0.6.0' \
    'py=1.8.1' \
    'pyaml' \
    'Pygments=2.6.1' \
    'pyparsing=2.4.6' \
    'python-dateutil=2.8.1' \
    'pytorch-gpu' \
    'pytorch-pretrained-bert' \
    'pytz=2019.3' \
    'pywavelets' \
    'PyYAML=5.3' \
    'pyzmq' 

RUN conda install --quiet --yes \
    'requests=2.23.0' \
    'retrying=1.3.3' \
    'scikit-learn=0.22*' \
    'scikit-image=0.16*' \
    'scikit-optimize=0.7*' \
    'scipy=1.4*' \
    'seaborn=0.10.0' \
    'Send2Trash=1.5.0' \
    'simplegeneric=0.8.1' \
    'six=1.14.0' \
    'sqlalchemy=1.3*' \
    'statsmodels=0.11*' \
    'sympy=1.5*' \
    'tensorflow-gpu' \
    'termcolor=1.1.0' \
    'terminado=0.8.3' \
    'testpath=0.4.4' \
    'tornado=6.0.4' \
    'tqdm=4.45*' \
    'traitlets=4.3.3' \
    'urllib3=1.25.8' \
    'vincent=0.4.*' \
    'wcwidth' \
    'webencodings=0.5.1' \
    'Werkzeug=1.0.1' \
    'wheel=0.34.2' \
    'widgetsnbextension=3.5.1' \
    'wrapt=1.12.1' \
    'xgboost=1.0.2' \
    'xlwt' \
    'xlrd'  && \
    conda remove --quiet --yes --force qt pyqt && \
    conda clean -tipsy 

#    'mlutils=0.1.0' \

RUN conda install --quiet --yes more-itertools tensorboard prometheus_client proj4
RUN conda install -c lewington lepmlutils 
RUN pip install -i https://pypi.anaconda.org/has2k1/simple plydata

    # Activate ipywidgets extension in the environment that runs the notebook server
RUN jupyter nbextension enable --py widgetsnbextension --sys-prefix && \
    # Also activate ipywidgets extension for JupyterLab
    # Check this URL for most recent compatibilities
    # https://github.com/jupyter-widgets/ipywidgets/tree/master/packages/jupyterlab-manager
	# jupyter labextension install jupyterlab_bokeh && \
    jupyter labextension install @jupyter-widgets/jupyterlab-manager && \
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
