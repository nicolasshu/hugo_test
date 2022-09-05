+++
title = "Tensorflow Cheatsheet"
tags = ["Deep", "Learning", "Python", "Tensorflow"]
draft = false
+++

## Suppress Tensorflow Warnings {#suppress-tensorflow-warnings}

```python
import os
os.environ["TF_CPP_MIN_LOG_LEVEL"] = "2"
```


## Allow for GPU Memory Growth {#allow-for-gpu-memory-growth}

```python
import tensorflow as tf
physical_devices = tf.config.list_physical_devices("GPU")
tf.config.experimental.set_memory_growth(physical_devices[0], True)
```