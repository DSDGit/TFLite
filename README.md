# TFLite

1. Setup the docker container for Tensorflow 1.6

```
sudo docker run -it -d -v `pwd`:/notebooks tensorflow/tensorflow:1.2.0 /bin/bash
```


2. List the dockers
```
sudo docker ps
```

3. Get into the docker container
```
sudo docker exec -it (container-id) /bin/bash
```

4. Test the already existing model
```
python -m scripts.label_image \
  --graph=tf_files/retrained_graph.pb  \
  --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg
```

5. Optimize your model using toco(TensorFlow Lite Optimizing Converter)
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

6. Retest the existing model
```
  python -m scripts.label_image \
  --graph=tf_files/retrained_graph.pb\
  --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg
```

7. Test the optimized model
```
  python -m scripts.label_image \
    --graph=tf_files/optimized_graph.pb \
    --image=tf_files/flower_photos/daisy/3475870145_685a19116d.jpg
```

8. Convert the model to TensorflowLite format
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

9. Create a directory for your reference
```
  mkdir android_tf
```

10. Copy the .lite file to the reference folder
```
  cp tf_files/optimized_graph.lite android_tf/
```

11. Copy the labels file to the reference folder
```
  cp tf_files/retrained_labels.txt android_tf/
```

12. Declare in case required
```
  declare USER=yourname
```

13. Copy to your storage drive
```
  gsutil cp -R android_tf/* gs://flower_${USER}/android_tf/
```


