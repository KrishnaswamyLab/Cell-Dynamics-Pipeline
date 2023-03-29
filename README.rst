Cell Dynamics Pipeline Implementation
=======================================

WORK IN PROGRESS

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

