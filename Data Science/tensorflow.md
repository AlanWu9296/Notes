# Keras

```python
import tensorflow as tf
from tensorflow import keras
```

## Build a simple model

```python
model = keras.Sequential()
# Adds a densely-connected layer with 64 units to the model:
model.add(keras.layers.Dense(64, activation='relu'))
# Add another:
model.add(keras.layers.Dense(64, activation='relu'))
# Add a softmax layer with 10 output units:
model.add(keras.layers.Dense(10, activation='softmax'))
```

There are many [`tf.keras.layers`](https://www.tensorflow.org/api_docs/python/tf/keras/layers) available with some common constructor parameters:

- `activation`: Set the activation function for the layer. This parameter is specified by the name of a built-in function or as a callable object. By default, no activation is applied.
- `kernel_initializer` and `bias_initializer`: The initialization schemes that create the layer's weights (kernel and bias). This parameter is a name or a callable object. This defaults to the `"Glorot uniform"` initializer.
- `kernel_regularizer` and `bias_regularizer`: The regularization schemes that apply the layer's weights (kernel and bias), such as L1 or L2 regularization. By default, no regularization is applied.

```python
# Create a sigmoid layer:
layers.Dense(64, activation='sigmoid')
# Or:
layers.Dense(64, activation=tf.sigmoid)

# A linear layer with L1 regularization of factor 0.01 applied to the kernel matrix:
layers.Dense(64, kernel_regularizer=keras.regularizers.l1(0.01))
# A linear layer with L2 regularization of factor 0.01 applied to the bias vector:
layers.Dense(64, bias_regularizer=keras.regularizers.l2(0.01))

# A linear layer with a kernel initialized to a random orthogonal matrix:
layers.Dense(64, kernel_initializer='orthogonal')
# A linear layer with a bias vector initialized to 2.0s:
layers.Dense(64, bias_initializer=keras.initializers.constant(2.0))
```

## Train and evaluate

```python
model.compile(optimizer=tf.train.AdamOptimizer(0.001),
              loss='categorical_crossentropy',
              metrics=['accuracy'])
```

[`tf.keras.Model.compile`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#compile) takes three important arguments:

- `optimizer`: This object specifies the training procedure. Pass it optimizer instances from the [`tf.train`](https://www.tensorflow.org/api_docs/python/tf/train) module, such as [`AdamOptimizer`](https://www.tensorflow.org/api_docs/python/tf/train/AdamOptimizer), [`RMSPropOptimizer`](https://www.tensorflow.org/api_docs/python/tf/train/RMSPropOptimizer), or [`GradientDescentOptimizer`](https://www.tensorflow.org/api_docs/python/tf/train/GradientDescentOptimizer).
- `loss`: The function to minimize during optimization. Common choices include mean square error (`mse`), `categorical_crossentropy`, and `binary_crossentropy`. Loss functions are specified by name or by passing a callable object from the [`tf.keras.losses`](https://www.tensorflow.org/api_docs/python/tf/keras/losses) module.
- `metrics`: Used to monitor training. These are string names or callables from the [`tf.keras.metrics`](https://www.tensorflow.org/api_docs/python/tf/keras/metrics) module.

**evaluate**

[`tf.keras.Model.fit`](https://www.tensorflow.org/api_docs/python/tf/keras/Model#fit) takes three important arguments:

- `epochs`: Training is structured into *epochs*. An epoch is one iteration over the entire input data (this is done in smaller batches).
- `batch_size`: When passed NumPy data, the model slices the data into smaller batches and iterates over these batches during training. This integer specifies the size of each batch. Be aware that the last batch may be smaller if the total number of samples is not divisible by the batch size.
- `validation_data`: When prototyping a model, you want to easily monitor its performance on some validation data. Passing this argument—a tuple of inputs and labels—allows the model to display the loss and metrics in inference mode for the passed data, at the end of each epoch.

```python
import numpy as np

data = np.random.random((1000, 32))
labels = np.random.random((1000, 10))

val_data = np.random.random((100, 32))
val_labels = np.random.random((100, 10))

model.fit(data, labels, epochs=10, batch_size=32,
          validation_data=(val_data, val_labels))
```

### Input tf.data datasets

Use the [Datasets API](https://www.tensorflow.org/guide/datasets) to scale to large datasets or multi-device training. Pass a [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset) instance to the `fit` method:

```python
# Instantiates a toy dataset instance:
dataset = tf.data.Dataset.from_tensor_slices((data, labels))
dataset = dataset.batch(32)
dataset = dataset.repeat()

# Don't forget to specify `steps_per_epoch` when calling `fit` on a dataset.
model.fit(dataset, epochs=10, steps_per_epoch=30)
```