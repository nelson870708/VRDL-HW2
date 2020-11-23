# VRDL-HW2
NCTU Selected Topics in Visual Recognition using Deep Learning Homework 2

## Hardware
The following specs were used to create the original solution.
- Ubuntu 18.04 LTS
- Intel(R) Core(TM) i7-6700 CPU @ 3.40 GHz
- NVIDIA GeForce GTX TITAN X

## Installation
1. Using Anaconda is strongly recommended. {envs_name} is the new environment name which you should assign.
    ```shell
    conda create -n {envs_name} python=3.7
    conda activate {envs_name}
    ```
2. Install PyTorch and torchvision following the [official instructions](https://pytorch.org/), e.g.,
    ```
    conda install pytorch torchvision -c pytorch
    ```
    Note: Make sure that your compilation CUDA version and runtime CUDA version match.
    You can check the supported CUDA version for precompiled packages on the [PyTorch website](https://pytorch.org/).

    `E.g` If you have CUDA 10.2 installed under `/usr/local/cuda` and would like to install
    PyTorch 1.7.0, you need to install the prebuilt PyTorch with CUDA 10.2.
    
    ```shell
    conda install pytorch torchvision torchaudio cudatoolkit=10.2 -c pytorch
    ```    
3. Install mmcv-full, I recommend you to install the pre-build package as below.

    ```
    pip install mmcv-full==latest+torch1.7.0+cu102 -f https://download.openmmlab.com/mmcv/dist/index.html
    ```
    See [here](https://github.com/open-mmlab/mmcv#install-with-pip) for different versions of MMCV compatible to different PyTorch and CUDA versions.
    Optionally you can choose to compile mmcv from source by the following command

    ```shell
    pip install mmcv-full
    ```
4. Clone the MMDetection repository.
    ```shell
    git clone https://github.com/open-mmlab/mmdetection.git
    cd mmdetection
    ```

5. Install build requirements and then install MMDetection.
    ```shell
    pip install -r requirements/build.txt
    pip install -v -e .  # or "python setup.py develop"
    ```
### Verification

To verify whether MMDetection and the required environment are installed correctly, we can run sample python codes to initialize a detector and inference a demo image:
```python
from mmdet.apis import init_detector, inference_detector

config_file = 'configs/faster_rcnn/faster_rcnn_r50_fpn_1x_coco.py'
device = 'cuda:0'
# init a detector
model = init_detector(config_file, device=device)
# inference the demo image
inference_detector(model, 'demo/demo.jpg')
```
Note: You should cd to mmdetection dirtectory.

The above code is supposed to run successfully upon you finish the installation.

## Dataset Preparation

You can download the data [here](https://drive.google.com/drive/u/1/folders/1Ob5oT9Lcmz7g5mVOcYH3QugA7tV3WsSl). The label is at the train directory, called "digitStruct.mat".

### Prepare Data and Code

After downloading, the data directory is structured as:
```
+- data
  +- train
    +- 1.png
    +- 2.png
    ...
  +- test
    +- 1.png
    +- 2.png
    ...
+- mmdetection
  ...
training_data_annotation.py
testing_data_annotation.py
faster_rcnn_r50_digit_number.py
```
### Data Preprocessing
It is going to split the training data randomly to mark training data and validation data in the json file, called "train.json". The ratio of the training data and valid data is 8 : 2

```shell
python3 training_data_annotations.py
```

## Training
You can train the model by following:

```shell
python3 mmdetection/tools/train.py faster_rcnn_r50_digit_number.py
```

## Testing
You can test the model and make a json submission file as following:

```shell
python3 mmdetection/tools/test.py faster_rcnn_r50_digit_number.py
```





