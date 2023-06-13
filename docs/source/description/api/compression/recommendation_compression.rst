Recommendation Compression
==========================

.. autofunction:: netspresso.compressor.__init__.ModelCompressor.recommendation_compression


Details of Parameters
---------------------


Compression Method
~~~~~~~~~~~~~~~~~~

.. autoclass:: netspresso.compressor.__init__.CompressionMethod
    :noindex:


Available Compression Method
++++++++++++++++++++++++++++
+------------+------------------------------+
| Name       | Description                  |
+============+==============================+
| PR_L2      | L2 Norm Pruning              |
+------------+------------------------------+
| PR_GM      | GM Pruning                   |
+------------+------------------------------+
| PR_NN      | Nuclear Norm Pruning         |
+------------+------------------------------+
| FD_TK      | Tucker Decomposition         |
+------------+------------------------------+
| FD_SVD     | Singular Value Decomposition |
+------------+------------------------------+

Example
+++++++

.. code-block:: python

    from netspresso.compressor import CompressionMethod

    COMPRESSION_METHOD = CompressionMethod.PR_L2

.. warning::
    - Nuclear Norm is only supported in the Tensorflow-Keras Framework.

Recommendation Method
~~~~~~~~~~~~~~~~~~~~~

.. autoclass:: netspresso.compressor.__init__.RecommendationMethod
    :noindex:


Available Recommendation Method
+++++++++++++++++++++++++++++++
+------------+---------------------------------------------------------------------+
| Name       | Description                                                         |
+============+=====================================================================+
| SLAMP      | Structured Layer-adaptive Sparsity for the Magnitude-based Pruning  |
+------------+---------------------------------------------------------------------+
| VBMF       | Variational Bayesian Matrix Factorization                           |
+------------+---------------------------------------------------------------------+

Example
+++++++

.. code-block:: python

    from netspresso.compressor import RecommendationMethod

    RECOMMENDATION_METHOD = RecommendationMethod.SLAMP

.. note::
    - If you selected PR_L2, PR_GM, PR_NN for compression_method
        - The recommended_method available is **SLAMP**.
    - If you selected FD_TK, FD_SVD for compression_method
        - The recommended_method available is **VBMF**.


Recommendation Ratio
~~~~~~~~~~~~~~~~~~~~~

.. note::

    - SLAMP (Pruning ratio)

        - Remove corresponding amounts of the filters. (e.g. 0.2 removes 20% of the filters in each layer)

        - Available ranges

        .. raw:: html

            <div align="center" style="padding: 20px;">
                <img src="https://latex.codecogs.com/svg.image?0&space;<ratio&space;\leq&space;&space;1&space;" title="https://latex.codecogs.com/svg.image?0 <ratio \leq 1 " />
            </div>
        
        - Click the link for more information. (`SLAMP`_)

    - VBMF (Calibration ratio)

        - This function control compression level of model if the result of recommendation doesn't meet the compression level user wants. Remained rank add or subtract (removed rank x calibration ratio) according to calibration ratio range.

            .. image:: ../../../_static/compression/calibration_ratio.png
                :width: 500
                :align: center


        - Available ranges

        .. raw:: html
                
            <div align="center" style="padding: 20px;">
                <img src="https://latex.codecogs.com/svg.image?-1&space;\leq&space;&space;ratio&space;\leq&space;&space;1&space;" title="https://latex.codecogs.com/svg.image?-1 \leq ratio \leq 1 " />
            </div>
        
        - Click the link for more information. (`VBMF`_)

.. _SLAMP : https://docs.netspresso.ai/docs/mc-structured-pruning#supported-functions
.. _VBMF: https://docs.netspresso.ai/docs/mc-filter-decomposition#recommendation-in-model-compressor


Details of Returns
------------------

.. autoclass:: netspresso.compressor.__init__.CompressedModel
    :noindex:


Example
-------

.. code-block:: python

    from netspresso.compressor import ModelCompressor


    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    compressed_model = compressor.recommendation_compression(
        model_id="YOUR_UPLOADED_MODEL_ID",
        model_name="YOUR_COMPRESSED_MODEL_NAME",
        compression_method=CompressionMethod.PR_L2,
        recommendation_method=RecommendationMethod.SLAMP,
        recommendation_ratio=0.5,
        output_path="OUTPUT_PATH",  # ex) ./compressed_model.h5
    )

Output
~~~~~~

.. code-block:: bash

    >>> compressed_model
    CompressedModel(
        model_id="78f65510-1f99-4856-99d9-60902373bd1d", 
        model_name="YOUR_COMPRESSED_MODEL_NAME", 
        task="image_classification", 
        framework="tensorflow_keras", 
        input_shapes=[InputShape(batch=1, channel=3, dimension=[32, 32])], 
        model_size=2.9439, 
        flops=24.1811, 
        trainable_parameters=0.6933, 
        non_trainable_parameters=0.01, 
        number_of_layers=0, 
        compression_id="b9feccee-d69e-4074-a225-5417d41aa572", 
        original_model_id="YOUR_UPLOADED_MODEL_ID"
    )
