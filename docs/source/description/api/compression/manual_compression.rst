Manual Compression
==================

Select Compression Method
-------------------------

.. autofunction:: netspresso.compressor.__init__.ModelCompressor.select_compression_method


Details of Parameters
~~~~~~~~~~~~~~~~~~~~~


Compression Method
++++++++++++++++++

.. autoclass:: netspresso.compressor.__init__.CompressionMethod
    :noindex:


Available Compression Method
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
+------------+------------------------------+
| Name       | Description                  |
+============+==============================+
| PR_L2      | L2 Norm Pruning              |
+------------+------------------------------+
| PR_GM      | GM Pruning                   |
+------------+------------------------------+
| PR_NN      | Nuclear Norm Pruning         |
+------------+------------------------------+
| PR_ID      | Pruning By Index             |
+------------+------------------------------+
| FD_TK      | Tucker Decomposition         |
+------------+------------------------------+
| FD_SVD     | Singular Value Decomposition |
+------------+------------------------------+
| FD_CP      | CP Decomposition             |
+------------+------------------------------+

Example
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    from netspresso.compressor import CompressionMethod

    COMPRESSION_METHOD = CompressionMethod.PR_L2

.. warning::
    - Nuclear Norm is only supported in the Tensorflow-Keras Framework.

.. note::
    - Structured Pruning (Criteria)
        - Difference of each pruning method is about measuring importance of filters in each layer. Filters in each layer will be automatically pruned based on certain criteria.

        - L2 Norm Pruning
            - L2-Norm is used to represent the importance of the corresponding filter. In other words, this method prunes filters based on the magnitude of weights.

            .. image:: ../../../_static/compression/pruning_l2.png
                :width: 500
                :align: center

        - GM Pruning
            - Geometric Median is used to measure the redundancy of the corresponding filter and remove redundant filters.

            .. image:: ../../../_static/compression/pruning_gm.png
                :width: 500
                :align: center

        - Nuclear Norm Pruning
            - The Nuclear Norm is the sum of the singular values representing the energy. It computes the nuclear norm on the feature map to determine the filter's relevance. For this reason, a portion of the dataset is needed.

            .. image:: ../../../_static/compression/pruning_nn.png
                :width: 500
                :align: center

    - Structured Pruning (Index)
        - This function prunes the chosen filters of each layer through the index without certain criteria.
        - You can apply your own criteria to prune the model.
        - If the selected filters are redundant or less important, it will return a better performing model.

        .. image:: ../../../_static/compression/pruning_by_index.png
            :width: 400
            :align: center

    - Filter Decomposition
        - Tucker Decomposition
            - Approximating the original filters by Tucker decomposition method.
            - This method decomposes the convolution with a 4D kernel tensor into two factor matrices and one small core tensor.

            .. image:: ../../../_static/compression/tucker_decomposition.png
                :width: 500
                :align: center
            
            - In Channel: The number of input channels in each layer.
            - Out Channel: The number of output channels in each layer.
            - In Rank: The number of input channel of core tensor that represent relation level of low-rank factor matrix.
            - Out Rank: The number of output channel of core tensor that represent relation level of low-rank factor matrix.

        - Singular Value Decomposition
            - Approximating the original filters by Singular value decomposition method.
            - This method decomposes the pointwise convolution or fully-connected layer into two pointwise or fully-connected layers.

            .. image:: ../../../_static/compression/singular_value_decomposition.png
                :width: 500
                :align: center
            
            - In Channel: The number of input channels in each layer.
            - Out Channel: The number of output channels of the target layer.
            - Rank: The condition number of weight matrix W.

        - CP Decomposition
            - Approximating the original filters by CP decomposition method.
            - This method approximates the convolution with a 4D kernel tensor by the sequence of four convolutions with small 2D kernel tensors.

            .. image:: ../../../_static/compression/cp_decomposition.png
                :width: 500
                :align: center
            
            - In Channel: The number of input channels in each layer.
            - Out Channel: The number of output channels in each layer.
            - Rank: A sum of N-way outer products of rank-one tensor for estimating original convolution filter.



Details of Returns
~~~~~~~~~~~~~~~~~~

.. autoclass:: netspresso.compressor.__init__.CompressionInfo
    :noindex:


Example
+++++++

.. code-block:: python

    from netspresso.compressor import ModelCompressor


    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    compression_info = compressor.select_compression_method(
        model_id="YOUR_UPLOADED_MODEL_ID",
        compression_method=CompressionMethod.PR_L2,
    )

Output
^^^^^^

.. code-block:: bash

    >>> compression_info
    CompressionInfo(
        compression_method="PR_L2", 
        available_layers=[
            AvailableLayer(name='conv1', values=[""], use=False, channels=[32]), 
            AvailableLayer(name='layers.0.conv2', values=[""], use=False, channels=[64]), 
            AvailableLayer(name='layers.1.conv2', values=[""], use=False, channels=[128]), 
            AvailableLayer(name='layers.2.conv2', values=[""], use=False, channels=[128]), 
            AvailableLayer(name='layers.3.conv2', values=[""], use=False, channels=[256]), 
            AvailableLayer(name='layers.4.conv2', values=[""], use=False, channels=[256]), 
            AvailableLayer(name='layers.5.conv2', values=[""], use=False, channels=[512]), 
            AvailableLayer(name='layers.6.conv2', values=[""], use=False, channels=[512]), 
            AvailableLayer(name='layers.7.conv2', values=[""], use=False, channels=[512]), 
            AvailableLayer(name='layers.8.conv2', values=[""], use=False, channels=[512]), 
            AvailableLayer(name='layers.9.conv2', values=[""], use=False, channels=[512]), 
            AvailableLayer(name='layers.10.conv2', values=[""], use=False, channels=[512]), 
            AvailableLayer(name='layers.11.conv2', values=[""], use=False, channels=[1024]), 
            AvailableLayer(name='layers.12.conv2', values=[""], use=False, channels=[1024])
        ], 
        original_model_id="YOUR_UPLOADED_MODEL_ID",
        compressed_model_id="", 
        compression_id="", 
    )


Set Compression Params
----------------------

.. rst-class:: table

+--------------------+------------------+--------+---------------------------------------+
| Compression Method | Number of Values | Type   | Range                                 |
+====================+==================+========+=======================================+
| PR_L2              | 1                | Float  | 0.0 < ratio ≤ 1.0                     |
+--------------------+------------------+--------+---------------------------------------+
| PR_GM              | 1                | Float  | 0.0 < ratio ≤ 1.0                     |
+--------------------+------------------+--------+---------------------------------------+
| PR_NN              | 1                | Float  | 0.0 < ratio ≤ 1.0                     |
+--------------------+------------------+--------+---------------------------------------+
| PR_ID              | (Num of Out      | Int    | 0 < channels ≤ Num of Out Channels    |
|                    | Channels - 1)    |        |                                       |
+--------------------+------------------+--------+---------------------------------------+
| FD_TK              | 2                | Int    | 0 < rank ≤ (Num of In Channels or     |
|                    |                  |        | Num of Out Channels)                  |
+--------------------+------------------+--------+---------------------------------------+
| FD_CP              | 1                | Int    | 0 < rank ≤ min(Num of In Channels or  |
|                    |                  |        | Num of Out Channels)                  |
+--------------------+------------------+--------+---------------------------------------+
| FD_SVD             | 1                | Int    | 0 < rank ≤ min(Num of In Channels or  |
|                    |                  |        | Num of Out Channels)                  |
+--------------------+------------------+--------+---------------------------------------+


Example
+++++++

.. code-block:: python

   from netspresso.compressor import ModelCompressor


   for available_layer in compression_info.available_layers:
      available_layer.values = [0.2]

Output
^^^^^^

.. code-block:: bash

      >>> compression_info
      CompressionInfo(
         compression_method="PR_L2", 
         available_layers=[
            AvailableLayer(name='conv1', values=[0.2], use=False, channels=[32]), 
            AvailableLayer(name='layers.0.conv2', values=[0.2], use=False, channels=[64]), 
            AvailableLayer(name='layers.1.conv2', values=[0.2], use=False, channels=[128]), 
            AvailableLayer(name='layers.2.conv2', values=[0.2], use=False, channels=[128]), 
            AvailableLayer(name='layers.3.conv2', values=[0.2], use=False, channels=[256]), 
            AvailableLayer(name='layers.4.conv2', values=[0.2], use=False, channels=[256]), 
            AvailableLayer(name='layers.5.conv2', values=[0.2], use=False, channels=[512]), 
            AvailableLayer(name='layers.6.conv2', values=[0.2], use=False, channels=[512]), 
            AvailableLayer(name='layers.7.conv2', values=[0.2], use=False, channels=[512]), 
            AvailableLayer(name='layers.8.conv2', values=[0.2], use=False, channels=[512]), 
            AvailableLayer(name='layers.9.conv2', values=[0.2], use=False, channels=[512]), 
            AvailableLayer(name='layers.10.conv2', values=[0.2], use=False, channels=[512]), 
            AvailableLayer(name='layers.11.conv2', values=[0.2], use=False, channels=[1024]), 
            AvailableLayer(name='layers.12.conv2', values=[0.2], use=False, channels=[1024])
         ], 
         original_model_id="YOUR_UPLOADED_MODEL_ID",
         compressed_model_id="", 
         compression_id="", 
      )


Compress Model
--------------

.. autofunction:: netspresso.compressor.__init__.ModelCompressor.compress_model


Details of Returns
~~~~~~~~~~~~~~~~~~

.. autoclass:: netspresso.compressor.__init__.CompressedModel
    :noindex:


Example
+++++++

.. code-block:: python

    from netspresso.compressor import ModelCompressor


    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    compress_model = compressor.compress_model(
        compression=compression_info,
        model_name="YOUR_COMPRESSED_MODEL_NAME",
        output_path="OUTPUT_PATH",  # ex) ./compressed_model.h5
    )

Output
^^^^^^

.. code-block:: bash

    >>> compress_model
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
