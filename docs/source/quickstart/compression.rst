Compression
===========

Advanced Compression
--------------------

Manual Compression
``````````````````
.. code-block:: python

    from netspresso.compressor import ModelCompressor

    
    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    compression = compressor.select_compression_method(
        model_id="YOUR_UPLOADED_MODEL_ID",
        compression_method=CompressionMethod.PR_L2
    )
    print(f"compression method: {compression.compression_method}")
    print(f"available layers: {compression.available_layers}")

    for available_layer in compression.available_layers:
        available_layer.values = [0.2]

    compressed_model = compressor.compress_model(
        compression=compression,
        model_name="YOUR_COMPRESSED_MODEL_NAME",
        output_path="OUTPUT_PATH",  # ex) ./compressed_model.h5
    )


Recommendation Compression
``````````````````````````
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


Automatic Compression
---------------------
.. code-block:: python

    from netspresso.compressor import ModelCompressor


    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    compressed_model = compressor.automatic_compression(
        model_id="YOUR_UPLOADED_MODEL_ID",
        model_name="YOUR_COMPRESSED_MODEL_NAME",
        output_path="OUTPUT_PATH",  # ex) ./compressed_model.h5
        compression_ratio=0.5,
    )
