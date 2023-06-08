#####################################
Quick Start with sk-cyto
#####################################


Installation
============

From PyPI
.. code-block::
    pip install sk-cyto

Install from Github source code with

.. code-block::
    git clone git@github.com:MSHelm/sk-cyto.git
    cd sk-cyto
    pip install .

Training your first FlowSOM model
=========================

File reading
-------------
.. _flowIO: https://github.com/whitews/FlowIO
*sk-cyto* does not provide file reading capabilities. To read fcs files, please refer to
the excellent flowIO_ package, which has zero dependencies.

.. code-block::python
    import flowio
    import numpy as np
    import pandas as pd

    fcs_data = flowio.FlowData("example.fcs")
    X = np.reshape(fcs_data.events, (-1, fcs_data.channel_count))

    # If desired create a DataFrame with meaningful column names
    pnn_labels = [val["PnN"] for _, val in fcs_data.channels.items()]
    X_train = pd.DataFrame(X, columns = pnn_labels)

    # Extract compensation matrix
    C = flowio.parse_compensation_matrix(fcs_data.text["spill"])

Alternatively for testing purposes, we can create a fake data set with 5 populations like this:

.. code-block::python
    import numpy as np
    import pandas as pd
    from sklearn.datasets import make_blobs
    from sklearn.model_selection import train_test_split
    X, _ = make_blobs(n_samples=1000, n_features=10, centers = 5, random_state=42)

    X_train, X_test = train_test_split(X)

    # Create fake uncompensated data.
    c = np.random.randn(10, 10)
    C = (c + c.T)/2
    C.fill_diagonal(0)

    X_train = np.dot(X_train, C)


Preprocessing the data
----------------------

Flow cytometry data usually is compensated and transformed. This was mostly done historically,
to make it easier for the human analyzers to separate populations. Machine learning models often
do not require this kind of preprocessing, but it can help the training process. The FlowSOM
paper for example suggests to normalize your data in addition to compensation and logicle transformation.
Also, FlowSOM does usually only use fluorescence channels, and not the forward and side scatter channels.

To combine all these preprocessing steps, we can take advantage of the sklearn infrastructure.

.. code-block::python
    from sklearn.preprocessing import StandardScaler
    from sklearn.pipeline import Pipeline
    from sklearn.compose import ColumnTransformer, make_column_selector
    from skcyto.preprocessing import LogicleTransformer, CompensationTransformer
    from skcyto.flowsom import FlowSOM
    
    pipeline = Pipeline([
      ("compensation", CompensationTransformer(C)),
      ("logicle_transform", LogicleTransformer()),
      ("Standardization", StandardScaler())
    ])

    transform = ColumnTransformer(
      (pipeline, make_column_selector(pattern = "^(?!FL)"))
    )

    transform.fit_transform(X_train)


Training a FlowSOM model
------------------------

Now we can easily train a FlowSOM model.

.. code-block::python
    from skcyto import FlowSOM

    X_in = transform.X_

    fsom = FlowSOM(k_min = 2, k_max = 10)
    fsom.fit(X_)
    fsom.labels_


Combining everything using sklearn infrastructure
--------------------------------------------------

We can also easily integrate the FlowSOM training into the pipeline

.. code-block::python
    transform.pipeline.steps.append["FlowSOM", FlowSOM(k_min = 2, k_max = 10)]
    transform.fit_transform(X_train)


Using the model for inference
-----------------------------

FlowSOM can not only be used for unsupervised clustering, the model can also be used
for inference of new data points. For example, you might collect some reference data set
at the beginning of your experiments, to which you want to map other data. 

.. code-block::python

    transform.predict(X_test)

