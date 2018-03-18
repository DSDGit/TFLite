# TFLite

```
sudo docker run -it -d -v `pwd`:/notebooks tensorflow/tensorflow:1.2.0 /bin/bash
```

```
sudo docker ps
```

```
sudo docker exec -it (container-id) /bin/bash
```

```
python -m scripts.label_image \
  --graph=tf_files/retrained_graph.pb  \
  --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg
```

```
IMAGE_SIZE=224
toco \
  --input_file=tf_files/retrained_graph.pb \
  --output_file=tf_files/optimized_graph.pb \
  --input_format=TENSORFLOW_GRAPHDEF \
  --output_format=TENSORFLOW_GRAPHDEF \
  --input_shape=1,${IMAGE_SIZE},${IMAGE_SIZE},3 \
  --input_array=input \
  --output_array=final_result
```

```
  python -m scripts.label_image \
  --graph=tf_files/retrained_graph.pb\
  --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg
```

```
  python -m scripts.label_image \
    --graph=tf_files/optimized_graph.pb \
    --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg
```

```
IMAGE_SIZE=224
toco \
  --input_file=tf_files/retrained_graph.pb \
  --output_file=tf_files/optimized_graph.lite \
  --input_format=TENSORFLOW_GRAPHDEF \
  --output_format=TFLITE \
  --input_shape=1,${IMAGE_SIZE},${IMAGE_SIZE},3 \
  --input_array=input \
  --output_array=final_result \
  --inference_type=FLOAT \
  --input_type=FLOAT
```

```
  mkdir android_tf
```

```
  cp tf_files/optimized_graph.lite android_tf/
```

```
  cp tf_files/retrained_labels.txt android_tf/
```

```
  declare USER=yourname
```

```
  gsutil cp -R android_tf/* gs://flower_${USER}/android_tf/
```


