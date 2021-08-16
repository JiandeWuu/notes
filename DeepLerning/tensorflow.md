# TensorFlow

- 第一步引入

    ```python
    import tensorflow as tf
    ```

- 建構 model

  ```python
  tf.keras.models.Sequential([
    # Layer
  ])
  ```

## Layer

---

### Input

```python
tf.keras.Input(
    shape=None, batch_size=None, name=None, dtype=None, sparse=False, tensor=None,
    ragged=False, **kwargs
)
```

### Dense

全連接層，可設定隱藏層神經元個數與 output 要使用的活化函數（可自訂）。

```python
tf.keras.layers.Dense({hidden_size}, activation='sigmoid'),
```

Dense : output = activation(dot(input, kernel) + bias)

### Flatten

補齊輸入尺寸。

```python
tf.keras.layers.Flatten(input_shape=(input_size, ))
```

## model compile

```python
compile(
    optimizer='rmsprop', loss=None, metrics=None, loss_weights=None,
    sample_weight_mode=None, weighted_metrics=None, **kwargs
)
```

```python
model.compile(optimizer=tf.keras.optimizers.SGD(learning_rate=learning_rate, momentum=0.0, nesterov=False, name='SGD'),
    loss=loss_fn,
    metrics=['accuracy'])
```

## model fit

```python
fit(
    x=None, y=None, batch_size=None, epochs=1, verbose=1, callbacks=None,
    validation_split=0.0, validation_data=None, shuffle=True, class_weight=None,
    sample_weight=None, initial_epoch=0, steps_per_epoch=None,
    validation_steps=None, validation_batch_size=None, validation_freq=1,
    max_queue_size=10, workers=1, use_multiprocessing=False
)
```

```python
model.fit(x, t, epochs=epochs)
```