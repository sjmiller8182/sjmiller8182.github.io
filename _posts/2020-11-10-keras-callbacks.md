---
title: 'Implementing Callbacks in TensorFlow 2'
date: 2020-11-10
permalink: /posts/2020/11/keras-callbacks/
tags:
  - python
  - keras
  - tensorflow
  - callbacks
---

Often when training neural networks, we will want to monitor the training process, record metrics, or make changes to the training process.
In TensorFlow 2, callbacks can used to call utilities at certain points during training of a neural network.
In this post, I'll describe some of the common use cases of callbacks, show how to implement some of the standard callbacks with the built-in classes,
 and show how to write custom callbacks to make changes to the training process.

## Callback Basics

Lets start with the general Callback class [tf.keras.callbacks.Callback](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/Callback).
This is an abstract base class that should be used for creating callbacks.
It provides several standard methods that can be implemented by subclasses.
Thw two callback methods I most commonly use are:

* `on_train_batch_end`: Executes at the end of a batch during training.
* `on_train_epoch_end`: Executes at the end of an epoch during training.

The callback documentation also shows the signature that should be used when implementing the callback.
For instance, `on_train_batch_end` has the following signature:

```python
on_train_batch_end(
    batch, logs=None
)
```

A subclass implementing `on_train_batch_end` should implement this same signature.
As an example:

```python
import tensorflow as tf

class MyCustomCallback(tf.keras.callbacks.Callback):
    """Example custom callback
    """
    def on_train_batch_end(batch, logs=None)
        """Custom Implementation of on_train_batch_end
        """
        < ... code to execcute when the training batch ends ... >

```


## Standard Callbacks

TensorFlow provides implementations of callbacks for the most common uses early stopping, logging, and learning rate scheduling, just to name a few.
In many cases, these built-in implementations are sufficient for monitoring or controlling training.
we will look at four that I commonly use: [EarlyStopping](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/EarlyStopping),
 [CSVLogger](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/CSVLogger),
 [LearningRateScheduler](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/LearningRateScheduler),
 and [ModelCheckpoint](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/ModelCheckpoint).

### EarlyStopping

The goal of training a nueral network is to minimize the loss (typically on a validation or test dataset).
The `EarlyStopping` callback implements this behavior.
The following four parameters should be set to configuring the stopping criterion:

* `monitor`: The metric to monitor (default: `'val_loss'`). Any metrics passed to `.fit` can be monitored (`accuracy`, `auc`, etc). When a validation set is provided, the metrics for the validation set are prepended with `'val_'`. So the metrics `'loss'` and `'val_loss'` refer to training loss and validation loss, respectively.
* `min_delta`: The minimum change in the metric required to continue training.
* `patience`: How many epochs to continuing training when the `min_delta` condition is not met.
* `mode`: Whether to consider the min or max value of the metric when checking `min_delta` (one of `{"auto", "min", "max"}`).
  * When monitoring a decreasing metric like `cross-entropy`, use `'min'`.
  * When monitoring an increasing metric like `AUC`, use `'max'`.
  * For some metrics (like `cross-entropy`), TensorFlow will know if the metric should minimized or maximized, `'auto'` can be used in these cases.

```python
# create a callback to monitor validation loss
early_stopping = tf.keras.callbacks.EarlyStopping(
    monitor='val_loss',
    min_delta=0.01,
    patience=5
)
```

### CSVLogger

When working with deep learning on smaller networks or smaller datasets, it may be sufficient to collect the results when the training is finished.
However, this is not possible when training larger network or using a larger dataset, which may take days or weeks and may require restarts.
In cases where it is expected that a network will take day or weeks to train, its a good idea to log the performance of each epoch.
This can be acomplished with the `CSVLogger` callback.
Only three parameters are required to setup CSV logging:

* `filename`: The filename of the CSV file to use for logging.
* `separator`: The delimitor between row elements (default ',').
* `append`: Whether to append to the file if it already exists (True) or overwrite an existing file (False).

```python
# save training log to a file called 'training.csv'
csv_logger = CSVLogger('./results/training.csv')
```

### LearningRateScheduler

It common practice to decay learning rates over the course of training a neural network.
The `LearningRateScheduler` implements a method for changing the learning rate at the end of each training epoch.
To utilize the `LearningRateScheduler`, we simply need to implement a function that accepts two arguments, `epoch` and `lr`, and returns a new learning rate.
This function will then be passed to the `LearningRateScheduler` and used to monify the learning rate during training.
The following example shows how this could be done:

```python
def lr_schedule(epoch, lr):
    """Linearly increase lr by 0.001 for first 10 epochs, then exponentially decay
    """
    if epoch < 10:
        return lr + 0.001
    else:
        return lr * tf.math.exp(-0.1)

# create the lr scheduler
lr_scheduler = LearningRateScheduler(lr_schedule)
```


### ModelCheckpoint

As mentioned previously, when training on large datasets, it may be necessary to stop and restart training.
The `ModelCheckpoint` callback can be used to save models, which can be restarted.
The following example shows a basic implementation for checkpointing.
Note there are a few optional arguments that might be useful for more specific implementations.

```python
checkpoint_filepath = './checkpoint'
model_checkpoint_callback = tf.keras.callbacks.ModelCheckpoint(
    filepath=checkpoint_filepath)

# < ... train the model ... >

# later reload the model
model.load_weights(checkpoint_filepath)
```

## Writing Custom Callbacks

The built-in callbacks provide functionality for most common cases,
 but sometimes more compilated changes during change are necessary.
For those more exotic instances, custom callbacks will be necessay.
First, I'll show how to implement a simple custom callback.
Second, I'll show how to implement changing momentum and learning rate on each batch
 during training to replicate the training process in the paper 
 “Searching for Exotic Particles in High-energy Physics with Deep Learning” \[2\].


### Basic Example: Naive Implementation of Early Stopping 

In this example, a class `StopOnThreshold` is subclassed from `tf.keras.callbacks.Callback` (the abstract class for callbacks).
The constructor takes an argument `stopping_accuracy` which is the validation accuracy threshold fro stopping.
This class implements `on_epoch_end` from the superclass, which runs at the end of each training epoch.
Here, it checks if the expected metric is in the training logs and reports a message if it is missing.
Then, if checks if the validation accuracy has met the stopping condition and sets the '`stop_training`' flag if the stopping condition is met.

```python
class StopOnThreshold(tf.keras.callbacks.Callback):
    """Monitor validation accuracy and stop when a threshold is reached
    """
    def __init__(self, stopping_accuracy):
        self.stopping_accuracy = stopping_accuracy
        super(StopOnThreshold, self).__init__()

    def on_epoch_end(self, epoch, logs=None):
        if 'val_accuracy' not in logs.keys():
            print('Stopping conditioned on ' + 'val_accuracy' + ', but this metric is not available.'
                  'Available keys are ' + ', '.join(list(logs.keys())
        else:
            val_accuarcy = logs.get('val_accuracy')
            if val_accuarcy > self.stopping_accuracy:
                self.model.stop_training = True
```

This shows how to implement a simple custom callback.
Next, we'll let a more complicated example.

### Advanced Example: Changing Optimizer Momentum and Learning Rate For Each Batch

A recent class assignment was to replicate the model and training process from the paper
 “Searching for Exotic Particles in High-energy Physics with Deep Learning” \[2\].
In this paper, the optimizer learning rate was updated on each batch and the momentum was updated on each epoch.
As discussed previously, the built-in callback `LearningRateScheduler` runs on each epoch.
So this requires a custom callback.
While these can be *implemented in the same callback*, I will break them into two callbacks for clearity.

**First, the learning rate callback:**

The changes to learning rate is descibed by the following quote in the paper.

> "The learning rate decayed by a factor of 1.0000002 every batch update until it reached a minimum of 10−6"

The following code block implements this learning rate schedule.
Note the use of `tensorflow.keras.backend` (conventionally imported as K) to interact with the optimizer.

```python
import tensorflow as tf
import tensorflow.keras.backend as K

class LRSchedule(tf.keras.callbacks.Callback):
    """Lower the learning rate by a factor of 1.0000002 until the learning
    rate is at 1e-6 at which point it should remain constant
    """
    def __init__(self):
        super(LRSchedule, self).__init__()

    def on_batch_end(self, batch, logs):
        current_lr = K.get_value(model.optimizer.lr)
        if current_lr > 1e-6:
            lr = current_lr / 1.0000002 #1.00002
            K.set_value(self.model.optimizer.lr, lr)
        else:
            K.set_value(self.model.optimizer.lr, 1e-6)

lr_scheduler = LRSchedule()
```

**Now, the momentum callback:**

The changes to momentum is descibed by the following quote in the paper.

> "A momentum term increased linearly over the first 200 epochs from 0.9 to 0.99, at which point it remained constant. "

Again, note the use of `tensorflow.keras.backend`

```python
import tensorflow as tf
import tensflow.keras.backend as K

class MomentumSchedule(tf.keras.callbacks.Callback):
    """Update the momentum linearly from 0.9 to 0.99 between epochs 1 and 200 
    """
    def __init__(self):
        super(MomentumSchedule, self).__init__()

    def on_epoch_end(self, epoch, logs=None):
        starting_value = 0.9
        ending_value = 0.99
        number_epochs = 200
        step_increase = (ending_value-starting_value) / number_epochs
        if epoch > number_epochs:
            K.set_value(self.model.optimizer.momentum, ending_value)
        else:
            current_momentum = K.get_value(self.model.optimizer.momentum)
            current_momentum += step_increase
            K.set_value(self.model.optimizer.momentum, current_momentum)

momentum_scheduler = MomentumSchedule()
```


## References

* \[1\] TensorFlow Developers, "TensorFlow Core 2.3.0", [https://www.tensorflow.org/api_docs](https://www.tensorflow.org/api_docs)
* \[2\] JBaldi, P., P. Sadowski, and D. Whiteson. “Searching for Exotic Particles in High-energy Physics with Deep Learning.” Nature Communications 5 (July 2, 2014). [https://arxiv.org/pdf/1402.4735.pdf](https://arxiv.org/pdf/1402.4735.pdf)
