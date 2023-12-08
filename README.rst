Cell Dynamics Pipeline Implementation
=======================================

This pipeline implements our TrajectoryNet pipeline for discovering gene
regulatory structure from single cell dynamics in continuous time and space. We
first learn likely individual cell trajectories in a maximum likelihood
fashion. We then learn a graph between genes using a granger causality based
score to control the edge weights. We reference this graph and known regulatory
interactions to discovery likely regulatory relationships in the mesenchymal to
epithelial transition.

In brief, TrajectoryNet is a Continuous Normalizing Flow model which can
perform dynamic optimal transport using energy regularization and / or a
combination of velocity, density, and growth regularizations to better match
cellular trajectories. 

Our setting is similar to that of `WaddingtonOT
<https://broadinstitute.github.io/wot/>`_. In that we have access to a bunch of
population measurements of cells over time and would like to model the dynamics
of cells over that time period. TrajectoryNet is trained end-to-end and is
continuous both in gene space and in time.



Installation
------------

TrajectoryNet is available in `pypi`. Install by running the following

.. code-block:: bash

    pip install -r requirements.txt

This code was tested with python 3.7 and 3.8.


Basic Usage
-----------

Run with

.. code-block:: bash

    python -m TrajectoryNet.main --dataset SCURVE

To run TrajectoryNet on a small `S Curve` example. To use a
custom dataset expose the coordinates and timepoint information according
to the example jupyter notebooks in the `/notebooks/` folder.

If you have an `AnnData <https://anndata.readthedocs.io>`_ object then take a look at
`notebooks/Example_Anndata_to_TrajectoryNet.ipynb
<https://github.com/KrishnaswamyLab/Cell-Dynamics-Pipeline/blob/master/notebooks/Example_Anndata_to_TrajectoryNet.ipynb>`_,
which shows how to load one of the example `scvelo <https://scvelo.readthedocs.io>`_ anndata objects into
TrajectoryNet. Alternatively you can use the custom (compressed) format for
TrajectoryNet as described below. Running this code can take on the order of a day for a computer with a single GPU.

For this format TrajectoryNet requires the following:

1. An embedding matrix titled `[embedding_name]` (Cells x Dimensions)
2. A sample labels array titled `sample_labels` (Cells)

To run TrajectoryNet with a custom dataset use:

.. code-block:: bash

    python -m TrajectoryNet.main --dataset [PATH_TO_NPZ_FILE] --embedding_name [EMBEDDING_NAME]
    python -m TrajectoryNet.eval --dataset [PATH_TO_NPZ_FILE] --embedding_name [EMBEDDING_NAME]


See `notebooks/EB-Eval.ipynb` for an example on how to use TrajectoryNet on
a PCA embedding to get trajectories in the gene space.

TrajectoryNet save `backwards_trajectories.npy` which is a numpy file of trajectories. This file can be loaded and further processed in the Cell Dynamics Pipeline to learn Granger causal gene structures. This step can be found in `notebooks/granger-analysis.ipynb <https://github.com/KrishnaswamyLab/Cell-Dynamics-Pipeline/blob/master/notebooks/granger-analysis.ipynb>`_. This takes the data and learned trajectories and produces gene x gene graphs using the Total Granger Causal Score (TGCS).

Data
----

All processed data can be found in the `release <https://github.com/KrishnaswamyLab/Cell-Dynamics-Pipeline/releases/tag/v0.0.1>`_.
