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

   user_guide
   api

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Tutorial - Examples

   auto_examples/index

`Getting started <quick_start.html>`_
-------------------------------------

Information regarding this template and how to modify it for your own project.

`User Guide <user_guide.html>`_
-------------------------------

An example of narrative documentation.

`API Reference <api.html>`_
-------------------------------

All public APIs

`Examples <auto_examples/index.html>`_
--------------------------------------

A set of examples. It complements the `User Guide <user_guide.html>`_.