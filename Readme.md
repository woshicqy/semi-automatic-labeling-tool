# This project aims to develop a segment anything model-based labeling tool for object detection task.
The **Segment Anything Model (SAM)** produces high quality object masks from input prompts such as points or boxes, and it can be used to generate masks for all objects in an image. It has been trained on a [dataset](https://segment-anything.com/dataset/index.html) of 11 million images and 1.1 billion masks, and has strong zero-shot performance on a variety of segmentation tasks. This tool utilized the masks generated from SAM to decrease the human efforts when drawing boundary box for object detection task.

## Environment

Right now, it is only compatible with Windows system, there are some bugs when deploying on MacOS.

## Installation

The code requires `python>=3.8`, as well as `pytorch>=1.7` and `torchvision>=0.8`. Please follow the instructions [here](https://pytorch.org/get-started/locally/) to install both PyTorch and TorchVision dependencies. Installing both PyTorch and TorchVision with CUDA support is strongly recommended.

Install Segment Anything:

```
pip install git+https://github.com/facebookresearch/segment-anything.git
```

or clone the repository locally and install with

```
git clone git@github.com:facebookresearch/segment-anything.git
cd segment-anything; pip install -e .
```

Install other packages:

```
pip install -r .\requirement.txt
```


## Step 1

Before you use the tool to label your images, you need to get all image embeddings (masks) from SAM. To get all embeddings, please run:

```
python helpers\extract_embeddings.py --checkpoint-path sam_vit_h_4b8939.pth --dataset-folder .\example_dataset --device cpu
```


## Step 2

Convert original SAM to onnx format. Please pay attention, this tool doesn't support dynamic image size. It means that all images will be processed into the same size for example, after run the command line below, this tool will only process images with 1080 by 1920.

```
python helpers\generate_onnx.py --checkpoint-path sam_vit_h_4b8939.pth --onnx-model-path ./sam_onnx.onnx --orig-im-size 1080 1920
```

## Step 3

When you begin to label your images, you need to define your categories at first, for example, below defined 4 categories including generator, breaker, transformer and bus.

```
python segment_anything_annotator.py --onnx-model-path sam_onnx.onnx --dataset-path .\example_dataset --categories generator,breaker,tranformer,bus
```

After you runn all things successfully, you will get the below UI in your device.

<p align="center">
  <img src="assets/ui_demo.png?raw=true" width="75%" />
</p>

Then you can enjoy your work!
