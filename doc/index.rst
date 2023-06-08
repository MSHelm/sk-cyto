.. project-template documentation master file, created by
   sphinx-quickstart on Mon Jan 18 14:44:12 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to sk-cyto's documentation!
============================================

Flow cytometry is very amenable to classical Machine Learning. Still, data preprocessing 
specialized analysis algorithms are largely implemented in R or in their own scattered repositories
with very dissimlar API that one always needs to learn from scratch.

This package marries flow cytometry with the well-established sk-learn ecosystem.
It provides preprocessing functions, as well as ports of the major R algorithms, both
in sk-learn compliant and homogenous API.

sk-cyto does not provide data reading capabilities, as these have been implemented 
several times already. For example, have a look at the excellent `flowio <https://github.com/whitews/FlowIO>`_
module, which has 0 dependencies and can convert your fcs file to a numpy array. 


.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Getting Started

   quick_start

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Documentation

   api

`Getting started <quick_start.html>`_
-------------------------------------

How to use sk-cyto in conjunction with sk-learn

`API Reference <api.html>`_
-------------------------------

All public ``Transformers`` and ``Estimators``
