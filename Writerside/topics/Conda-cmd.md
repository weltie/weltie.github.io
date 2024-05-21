# Conda-cmd

## create

`conda create -n py311 python=3.11.2`

## clone

`conda create -n py311_clone --clone py311`

## remove

`conda remove -n py311_clone --all`

## export

`conda env export --no-builds > export.yaml`

## create -f

`conda env create -f export.yaml`

## install

`conda install xxx=1.0.0`

`conda install scipy --channel conda-forge`

## update base python

`conda activate base`

`conda update conda`

`conda search "^python$"`

`conda install python=3.11.0`