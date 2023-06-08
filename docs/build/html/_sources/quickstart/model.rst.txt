Model
=====

Upload Model
------------
.. code-block:: python

    from netspresso.compressor import ModelCompressor, Task, Framework


    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    model = compressor.upload_model(
        model_name="YOUR_MODEL_NAME",
        task=Task.IMAGE_CLASSIFICATION,
        framework=Framework.TENSORFLOW_KERAS,
        file_path="YOUR_MODEL_PATH", # ex) ./model.h5
        input_shapes="YOUR_MODEL_INPUT_SHAPES",  # ex) [{"batch": 1, "channel": 3, "dimension": [32, 32]}]
    )

Get Model
---------
.. code-block:: python

    from netspresso.compressor import ModelCompressor


    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    model = compressor.get_model(model_id="YOUR_MODEL_ID")

Get Uploaded Models
-------------------
.. code-block:: python

    from netspresso.compressor import ModelCompressor


    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    uploaded_models = compressor.get_uploaded_models()
    print(uploaded_models)

Get Compressed Models
---------------------
.. code-block:: python

    from netspresso.compressor import ModelCompressor


    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    compressed_models = compressor.get_compressed_models(model_id="YOUR_UPLOADED_MODEL_ID")
    print(compressed_models)

Get Total Models
----------------
.. code-block:: python

    from netspresso.compressor import ModelCompressor


    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    models = compressor.get_models()
    print(models)

Download Model
--------------
.. code-block:: python

    from netspresso.compressor import ModelCompressor


    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    compressor.download_model(
        model_id="YOUR_MODEL_ID",
        local_path="YOUR_LOCAL_PATH",  # ex) ./downloaded_model.h5
      )

Delete Model
------------
.. code-block:: python

    from netspresso.compressor import ModelCompressor


    compressor = ModelCompressor(email="YOUR_EMAIL", password="YOUR_PASSWORD")
    compressor.delete_model(model_id="YOUR_MODEL_ID")
    