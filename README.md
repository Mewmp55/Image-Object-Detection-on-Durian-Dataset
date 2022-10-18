# DADS7202HW02

## Introduce: This work is to try to create an object detection model using Tensorflow, following the steps of [TensorFlow 2 Object Detection API tutorial](https://tensorflow-object-detection-api-tutorial.readthedocs.io/en/latest/index.html) and YouTube Chanel [Krish Naik](https://www.youtube.com/watch?v=XoMiveY_1Z4) video.

### *Prepare DATASET*
<details>
<summary>Details: Prepare DATASET</summary>

- Prepare the images you want to use in the folder.

   <img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196252482-65f86535-9a44-4862-95d2-8c8587fb10bb.png">

- Go to the roboflow website.
- Sign up for a new account then sign in.
- Create new project and select project type as object detection.

   <img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196253940-498d7780-cbde-4fd4-855c-c44ff6b354d8.png">

- Upload image to project

   <img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196254333-26f139ce-e1d9-47d9-a04f-bde69b2a242d.png">

- Finishing upload

   <img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196255979-447657ef-52f8-415e-91e5-7a3bca21078a.png">

- Annotate images
  - Click on the image to annotate, then drag a frame around the object's area and classify it.
  - Go to the next image and repeat previous step to all images.
  
   <img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196256383-e0a00bc0-78f0-4293-bd19-f2fbfc39ee3a.png">

- Add images to dataset
  - Click the Add n images to Dataset at the top right of the website.
  - Choose the method adjust required value ,then click add images.
 
   <img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196257692-58a054ec-7793-4116-879d-3d38a442be8d.png">

- Generate new version of Dataset
  - Click the generate button at the bottom left of the screen.
  - In Section 3.Preprocessing Can be used to resize the image, and in Section 4. It can be used to do Augmentation and then generate a new dataset immediately in Section 5.

   <img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196258333-54abf1fe-a431-4845-b135-143d18e7ffd9.png">

- Export dataset

   <img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196258535-d99d1a9b-ecd7-42d6-8611-cdca47147589.png">

</details>

### *Install Tensorflow*

<details>
<summary>Details: Install Tensorflow</summary>
   
```python
!pip install tensorflow-gpu
```
---output---

```python
import tensorflow as tf
print(tf.__version__)
```
---output---

```python
!nvidia-smi -L
```
---output---
</details>

### *Cloning TFOD 2.0 Github to drive*

<details>
<summary>Details: Cloning TFOD 2.0 Github to drive</summary>
   
- Mounting Google Drive

```python
from google.colab import drive
drive.mount('/content/drive')
```
---output---

- Go to the folder symbol on the left side of the screen.
- Create the folder you want to clone the repository into.

<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196510070-ba902de7-9c4b-4f42-9003-ae7fefddeedf.png">

- Change directory to your folder.

```python
cd /content/drive/MyDrive/DADS7202
```
---output---

```python
!git clone https://github.com/tensorflow/models.git
```
---output---

- Cloning Tensorflow github repository
- In the created folder, you will see a new folder.

<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196510281-b8e23663-5209-4b88-957e-5438d5b4bbab.png">

- In the **models folder** ,go to research folder ,then go to the **object_detection** folder and download files **export_tflite_graph_tf2.py**, **exporter_main_v2.py** and **model_main_tf2.py**

<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196510913-9edc9d28-423c-4e76-ae88-cc794618cd15.png">
</details>
   
### *COCO API installation*

<details>
<summary>Details: COCO API installation</summary>

- Change directory to research folder

```python
cd /content/drive/MyDrive/DADS7202/models/research
```
---output---

- Install coco API

```python
!protoc object_detection/protos/*.proto --python_out=.
```
```python
!git clone https://github.com/cocodataset/cocoapi.git
```
---output---
```python
cd cocoapi/PythonAPI
```
---output---
```python
!make
```
---output---
```python
cp -r pycocotools /content/drive/MyDrive/DADS7202/models/research
```
---output---
</details>

### *Object Detection API installation*

<details>
<summary>Details: Object Detection API installation</summary>

- Back to **research** folder

```python
cd /content/drive/MyDrive/DADS7202/models/research
```
---output---

- Installing the object detection package

```python
cp object_detection/packages/tf2/setup.py .
```
---output---


> #python -m pip install --use-feature=2020-resolver .

```python
!python -m pip install .
```
---output---

- Test Installation

```python
!python object_detection/builders/model_builder_tf2_test.py
```
---output---
</details>
   
### *Training Custom Object Detector*

<details>
<summary>Details: Training Custom Object Detector</summary>
   
Preparing the Workspace

- In the DADS7202 folder (or other created folder in cloning TFOD github step), create a workspace folder and a subfolder, as shown below.

<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196512776-d4ddc051-f929-4215-b592-744c34821783.png">

- In the **annotations** folder right click + new file create *label_map.pbtxt*
  - double click on label_map.pbtxt and edit the label
  
  <img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196512933-eea5a62b-1a52-45f5-b059-84c77e1fba5d.png">

- In the **test** and **train** folders, upload the images to use train and test the model. In this work uses JPG+XML files.

<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196513043-0177688f-0c77-428a-ad49-785537b3acd5.png">

- Change directory to pre-trained-models folder

```python
cd /content/drive/MyDrive/DADS7202/workspace/training_demo/pre-trained-models
```
---output---

- Download Pre-Trained Model which are listed in [TensorFlow 2 Detection Model Zoo](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md).
- In this work we try to use Pre-Train model as **SSD ResNet101 V1 FPN 640x640 (RetinaNet101)**.

```python
!wget http://download.tensorflow.org/models/object_detection/tf2/20200711/ssd_resnet101_v1_fpn_640x640_coco17_tpu-8.tar.gz
```
---output---

- Extracted our pre-trained model and The pre-trained-model folder should look like this.

<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196516271-a753f502-a217-41ec-b39a-a6ae2258592a.png">

```python
!tar -xvf ssd_resnet101_v1_fpn_640x640_coco17_tpu-8.tar.gz
```
---output---

- In the **training-demo** folder, upload the previously downloaded files **export_tflite_graph_tf2.py**, **exporter_main_v2.py** and **model_main_tf2.py**.
  - This step is for easier to call this script.
  - Able to call the script in research folder without download and re-upload step.
  
  <img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196516574-10db4ccb-d2dd-48a7-b4a3-f62f01dc9d53.png">

- Download **partition_dataset.py** and **generate_tfrecord.py**
  - Go to [TensorFlow 2 Object Detection API tutorial](https://tensorflow-object-detection-api-tutorial.readthedocs.io/en/latest/training.html).
  - Download **Partition Dataset script**, then partition the Dataset. (In this work we skip this step because we preprocessing dataset on [Roboflow](https://roboflow.com/) already.)
  - Download **Generate TensorFlow Records script**.
  - Upload file into **training_demo** folder.
    <img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196516987-a58c2eb9-a3a3-48eb-8617-2ab2ab39d39d.png">

- Create TensorFlow Records.
  - Change directory to **training_demo**.
  - Run **generate_tfrecord.py** script to create tensorflow records.
  - Check the train folder, test folder, label_map.pbtxt and the ourput path before running.
  
```python
cd /content/drive/MyDrive/DADS7202/workspace/training_demo
```
---output---

> Create train data:
```python
!python generate_tfrecord.py -x /content/drive/MyDrive/DADS7202/workspace/training_demo/images/train -l /content/drive/MyDrive/DADS7202/workspace/training_demo/annotations/label_map.pbtxt -o /content/drive/MyDrive/DADS7202/workspace/training_demo/annotations/train.record
```

> Create test data:
```python
!python generate_tfrecord.py -x /content/drive/MyDrive/DADS7202/workspace/training_demo/images/test -l //content/drive/MyDrive/DADS7202/workspace/training_demo/annotations/label_map.pbtxt -o /content/drive/MyDrive/DADS7202/workspace/training_demo/annotations/test.record
```
---output---

- The annotations folder should be look like this.

<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196517704-15e14d48-0aa9-4872-8ca0-1a3ca4df162e.png">

- In **models** folder (**inside training_demo folder**) create a new directory named **my_ssd_resnet101_v1_fpn** and download **pipeline.config** from **pre-train-models/ssd_resnet101...**, then re-upload to the newly created directory. Our **training_demo** should now look like this:

<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196517998-799e438a-4ee8-4836-89de-79ed5746e519.png">

- Configure the Training Pipeline
  - Double click into pipeline.config in model/my_ssd_resnet101_v1_fpn
  - **Looking at line 3, let's change the number of different label classes**.
  - Line 6,7 can set image resizer height and width.
  - **Line 131 to set batch size**.
  - Line 136 to set augmentation options.
  - **Line 161 change the Path to checkpoint of pre-trained model**.
  - **Line 152,162 change number of step**.
  - **Line 167 change fine tune checkpoint type to detection**.
  - **Line 168 set it to false**.
  - **Line 172 change Path to label map file**.
  - **Line 174 change Path to training TFRecord file**.
  - **Line 182 change Path to label map file**.
  - **Line 186 change Path to testing TFRecord**.
  
<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196518462-62833952-1c11-4428-8d16-06d8fc907e26.png">

</details>


### *Training the model*

<details>
<summary>Details: Training the model</summary>

- Change directory to training_demo.

```python
cd /content/drive/MyDrive/DADS7202/workspace/training_demo
```
---output---

- Training the model

```python
!python model_main_tf2.py --model_dir=/content/drive/MyDrive/DADS7202/workspace/training_demo/models/my_ssd_resnet101_v1_fpn --pipeline_config_path=/content/drive/MyDrive/DADS7202/workspace/training_demo/models/my_ssd_resnet101_v1_fpn/pipeline.config
```
---output---
</details>

### *Evaluating the Model*

<details>
<summary>Details: Evaluating the Model</summary>

- Set metric type

```python
from object_detection.protos import eval_pb2
eval_config = eval_pb2.EvalConfig()
eval_config.metrics_set.extend(['coco_detection_metrics'])
```
---output---

- Change directory to training_demo

```python
cd /content/drive/MyDrive/DADS7202/workspace/training_demo
```
---output---

- Model evaluate using Tensorboard

```python
!python model_main_tf2.py --model_dir=/content/drive/MyDrive/DADS7202/workspace/training_demo/models/my_ssd_resnet101_v1_fpn --pipeline_config_path=/content/drive/MyDrive/DADS7202/workspace/training_demo/models/my_ssd_resnet101_v1_fpn/pipeline.config --checkpoint_dir=/content/drive/MyDrive/DADS7202/workspace/training_demo/models/my_ssd_resnet101_v1_fpn
```
---output---
</details>

### *Inferencing Trained Models*

<details>
<summary>Details: Inferencing Trained Models</summary>

- In exported-models folder create my_model folder
- Export the model to */content/drive/MyDrive/DADS7202/workspace/training_demo/exported-models/my_model*

```python
!python exporter_main_v2.py --input_type image_tensor --pipeline_config_path /content/drive/MyDrive/DADS7202/workspace/training_demo/models/my_ssd_resnet101_v1_fpn/pipeline.config --trained_checkpoint_dir /content/drive/MyDrive/DADS7202/workspace/training_demo/models/my_ssd_resnet101_v1_fpn --output_directory /content/drive/MyDrive/DADS7202/workspace/training_demo/exported-models/my_model
```
---output---

- Inferencing trained model

```python
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'    # Suppress TensorFlow logging (1)
import pathlib
import tensorflow as tf
import cv2
import argparse
from google.colab.patches import cv2_imshow

# Enable GPU dynamic memory allocation
gpus = tf.config.experimental.list_physical_devices('GPU')
for gpu in gpus:
    tf.config.experimental.set_memory_growth(gpu, True)

# PROVIDE PATH TO IMAGE DIRECTORY
IMAGE_PATHS = '/content/drive/MyDrive/DADS7202/workspace/training_demo/images/test/1_jpg.rf.24fda645c9751b1f97ca006a4c164020.jpg'

# PROVIDE PATH TO MODEL DIRECTORY
PATH_TO_MODEL_DIR = '/content/drive/MyDrive/DADS7202/workspace/training_demo/pre-trained-models/ssd_resnet101_v1_fpn_640x640_coco17_tpu-8'

# PROVIDE PATH TO LABEL MAP
PATH_TO_LABELS = '/content/drive/MyDrive/DADS7202/workspace/training_demo/annotations/label_map.pbtxt'

# PROVIDE THE MINIMUM CONFIDENCE THRESHOLD
MIN_CONF_THRESH = float(0.60)

# LOAD THE MODEL

import time
from object_detection.utils import label_map_util
from object_detection.utils import visualization_utils as viz_utils

PATH_TO_SAVED_MODEL = PATH_TO_MODEL_DIR + "/saved_model"

print('Loading model...', end='')
start_time = time.time()

# LOAD SAVED MODEL AND BUILD DETECTION FUNCTION
detect_fn = tf.saved_model.load(PATH_TO_SAVED_MODEL)

end_time = time.time()
elapsed_time = end_time - start_time
print('Done! Took {} seconds'.format(elapsed_time))

# LOAD LABEL MAP DATA FOR PLOTTING

category_index = label_map_util.create_category_index_from_labelmap(PATH_TO_LABELS,
                                                                    use_display_name=True)

import numpy as np
from PIL import Image
import matplotlib.pyplot as plt
import warnings
warnings.filterwarnings('ignore')   # Suppress Matplotlib warnings

def load_image_into_numpy_array(path):
    """Load an image from file into a numpy array.
    Puts image into numpy array to feed into tensorflow graph.
    Note that by convention we put it into a numpy array with shape
    (height, width, channels), where channels=3 for RGB.
    Args:
      path: the file path to the image
    Returns:
      uint8 numpy array with shape (img_height, img_width, 3)
    """
    return np.array(Image.open(path))




print('Running inference for {}... '.format(IMAGE_PATHS), end='')

image = cv2.imread(IMAGE_PATHS)
image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
image_expanded = np.expand_dims(image_rgb, axis=0)

# The input needs to be a tensor, convert it using `tf.convert_to_tensor`.
input_tensor = tf.convert_to_tensor(image)
# The model expects a batch of images, so add an axis with `tf.newaxis`.
input_tensor = input_tensor[tf.newaxis, ...]

# input_tensor = np.expand_dims(image_np, 0)
detections = detect_fn(input_tensor)

# All outputs are batches tensors.
# Convert to numpy arrays, and take index [0] to remove the batch dimension.
# We're only interested in the first num_detections.
num_detections = int(detections.pop('num_detections'))
detections = {key: value[0, :num_detections].numpy()
               for key, value in detections.items()}
detections['num_detections'] = num_detections

# detection_classes should be ints.
detections['detection_classes'] = detections['detection_classes'].astype(np.int64)

image_with_detections = image.copy()

# SET MIN_SCORE_THRESH BASED ON YOU MINIMUM THRESHOLD FOR DETECTIONS
viz_utils.visualize_boxes_and_labels_on_image_array(
      image_with_detections,
      detections['detection_boxes'],
      detections['detection_classes'],
      detections['detection_scores'],
      category_index,
      use_normalized_coordinates=True,
      max_boxes_to_draw=200,
      min_score_thresh=0.6,
      agnostic_mode=False)

print('Done')
# DISPLAYS OUTPUT IMAGE
cv2_imshow(image_with_detections)
# CLOSES WINDOW ONCE KEY IS PRESSED
```
<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196520945-4978e9a3-b512-44a1-a8ef-aad0a11fa4b7.png">
<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196521072-a133a268-62ad-418a-8372-93456b26da4d.png">

- Pretrain-model SSD_resnet101_v1_fpn without training

```python
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'    # Suppress TensorFlow logging (1)
import pathlib
import tensorflow as tf
import cv2
import argparse
from google.colab.patches import cv2_imshow

# Enable GPU dynamic memory allocation
gpus = tf.config.experimental.list_physical_devices('GPU')
for gpu in gpus:
    tf.config.experimental.set_memory_growth(gpu, True)

# PROVIDE PATH TO IMAGE DIRECTORY
IMAGE_PATHS = '/content/drive/MyDrive/DADS7202/workspace/training_demo/images/test/1_jpg.rf.24fda645c9751b1f97ca006a4c164020.jpg'

# PROVIDE PATH TO MODEL DIRECTORY
PATH_TO_MODEL_DIR = '/content/drive/MyDrive/DADS7202/workspace/training_demo/exported-models/my_model'

# PROVIDE PATH TO LABEL MAP
PATH_TO_LABELS = '/content/drive/MyDrive/DADS7202/workspace/training_demo/annotations/label_map.pbtxt'

# PROVIDE THE MINIMUM CONFIDENCE THRESHOLD
MIN_CONF_THRESH = float(0.60)

# LOAD THE MODEL

import time
from object_detection.utils import label_map_util
from object_detection.utils import visualization_utils as viz_utils

PATH_TO_SAVED_MODEL = PATH_TO_MODEL_DIR + "/saved_model"

print('Loading model...', end='')
start_time = time.time()

# LOAD SAVED MODEL AND BUILD DETECTION FUNCTION
detect_fn = tf.saved_model.load(PATH_TO_SAVED_MODEL)

end_time = time.time()
elapsed_time = end_time - start_time
print('Done! Took {} seconds'.format(elapsed_time))

# LOAD LABEL MAP DATA FOR PLOTTING

category_index = label_map_util.create_category_index_from_labelmap(PATH_TO_LABELS,
                                                                    use_display_name=True)

import numpy as np
from PIL import Image
import matplotlib.pyplot as plt
import warnings
warnings.filterwarnings('ignore')   # Suppress Matplotlib warnings

def load_image_into_numpy_array(path):
    """Load an image from file into a numpy array.
    Puts image into numpy array to feed into tensorflow graph.
    Note that by convention we put it into a numpy array with shape
    (height, width, channels), where channels=3 for RGB.
    Args:
      path: the file path to the image
    Returns:
      uint8 numpy array with shape (img_height, img_width, 3)
    """
    return np.array(Image.open(path))




print('Running inference for {}... '.format(IMAGE_PATHS), end='')

image = cv2.imread(IMAGE_PATHS)
image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
image_expanded = np.expand_dims(image_rgb, axis=0)

# The input needs to be a tensor, convert it using `tf.convert_to_tensor`.
input_tensor = tf.convert_to_tensor(image)
# The model expects a batch of images, so add an axis with `tf.newaxis`.
input_tensor = input_tensor[tf.newaxis, ...]

# input_tensor = np.expand_dims(image_np, 0)
detections = detect_fn(input_tensor)

# All outputs are batches tensors.
# Convert to numpy arrays, and take index [0] to remove the batch dimension.
# We're only interested in the first num_detections.
num_detections = int(detections.pop('num_detections'))
detections = {key: value[0, :num_detections].numpy()
               for key, value in detections.items()}
detections['num_detections'] = num_detections

# detection_classes should be ints.
detections['detection_classes'] = detections['detection_classes'].astype(np.int64)

image_with_detections = image.copy()

# SET MIN_SCORE_THRESH BASED ON YOU MINIMUM THRESHOLD FOR DETECTIONS
viz_utils.visualize_boxes_and_labels_on_image_array(
      image_with_detections,
      detections['detection_boxes'],
      detections['detection_classes'],
      detections['detection_scores'],
      category_index,
      use_normalized_coordinates=True,
      max_boxes_to_draw=200,
      min_score_thresh=0.6,
      agnostic_mode=False)

print('Done')
# DISPLAYS OUTPUT IMAGE
cv2_imshow(image_with_detections)
# CLOSES WINDOW ONCE KEY IS PRESSED
```

<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196521457-630ee530-8717-4304-a68a-de7adad358c4.png">
<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196521564-44488043-7a83-4159-b702-4e7aa03443a4.png">

- With 5,000 steps of training, the results are still unsatisfactory. After this, try 10,000 training steps.

```python
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'    # Suppress TensorFlow logging (1)
import pathlib
import tensorflow as tf
import cv2
import argparse
from google.colab.patches import cv2_imshow

# Enable GPU dynamic memory allocation
gpus = tf.config.experimental.list_physical_devices('GPU')
for gpu in gpus:
    tf.config.experimental.set_memory_growth(gpu, True)

# PROVIDE PATH TO IMAGE DIRECTORY
IMAGE_PATHS = '/content/drive/MyDrive/DADS7202/workspace/training_demo/images/test/1_jpg.rf.24fda645c9751b1f97ca006a4c164020.jpg'

# PROVIDE PATH TO MODEL DIRECTORY
PATH_TO_MODEL_DIR = '/content/drive/MyDrive/DADS7202/workspace/training_demo/exported-models/my_model2'

# PROVIDE PATH TO LABEL MAP
PATH_TO_LABELS = '/content/drive/MyDrive/DADS7202/workspace/training_demo/annotations/label_map.pbtxt'

# PROVIDE THE MINIMUM CONFIDENCE THRESHOLD
MIN_CONF_THRESH = float(0.60)

# LOAD THE MODEL

import time
from object_detection.utils import label_map_util
from object_detection.utils import visualization_utils as viz_utils

PATH_TO_SAVED_MODEL = PATH_TO_MODEL_DIR + "/saved_model"

print('Loading model...', end='')
start_time = time.time()

# LOAD SAVED MODEL AND BUILD DETECTION FUNCTION
detect_fn = tf.saved_model.load(PATH_TO_SAVED_MODEL)

end_time = time.time()
elapsed_time = end_time - start_time
print('Done! Took {} seconds'.format(elapsed_time))

# LOAD LABEL MAP DATA FOR PLOTTING

category_index = label_map_util.create_category_index_from_labelmap(PATH_TO_LABELS,
                                                                    use_display_name=True)

import numpy as np
from PIL import Image
import matplotlib.pyplot as plt
import warnings
warnings.filterwarnings('ignore')   # Suppress Matplotlib warnings

def load_image_into_numpy_array(path):
    """Load an image from file into a numpy array.
    Puts image into numpy array to feed into tensorflow graph.
    Note that by convention we put it into a numpy array with shape
    (height, width, channels), where channels=3 for RGB.
    Args:
      path: the file path to the image
    Returns:
      uint8 numpy array with shape (img_height, img_width, 3)
    """
    return np.array(Image.open(path))




print('Running inference for {}... '.format(IMAGE_PATHS), end='')

image = cv2.imread(IMAGE_PATHS)
image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
image_expanded = np.expand_dims(image_rgb, axis=0)

# The input needs to be a tensor, convert it using `tf.convert_to_tensor`.
input_tensor = tf.convert_to_tensor(image)
# The model expects a batch of images, so add an axis with `tf.newaxis`.
input_tensor = input_tensor[tf.newaxis, ...]

# input_tensor = np.expand_dims(image_np, 0)
detections = detect_fn(input_tensor)

# All outputs are batches tensors.
# Convert to numpy arrays, and take index [0] to remove the batch dimension.
# We're only interested in the first num_detections.
num_detections = int(detections.pop('num_detections'))
detections = {key: value[0, :num_detections].numpy()
               for key, value in detections.items()}
detections['num_detections'] = num_detections

# detection_classes should be ints.
detections['detection_classes'] = detections['detection_classes'].astype(np.int64)

image_with_detections = image.copy()

# SET MIN_SCORE_THRESH BASED ON YOU MINIMUM THRESHOLD FOR DETECTIONS
viz_utils.visualize_boxes_and_labels_on_image_array(
      image_with_detections,
      detections['detection_boxes'],
      detections['detection_classes'],
      detections['detection_scores'],
      category_index,
      use_normalized_coordinates=True,
      max_boxes_to_draw=200,
      min_score_thresh=0.6,
      agnostic_mode=False)

print('Done')
# DISPLAYS OUTPUT IMAGE
cv2_imshow(image_with_detections)
# CLOSES WINDOW ONCE KEY IS PRESSED
```
Loading model...Done! Took 21.366928339004517 seconds
Running inference for /content/drive/MyDrive/DADS7202/workspace/training_demo/images/test/1_jpg.rf.24fda645c9751b1f97ca006a4c164020.jpg... Done
<img width="500" alt="image" src="https://user-images.githubusercontent.com/97492504/196521952-64671dc6-717c-4ef9-a38a-93bed9b177e5.png">

- By increasing the steps to 10000, the results look better.
   
</details>

### *Performance Tuning*
