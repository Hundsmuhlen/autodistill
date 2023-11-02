<div align="center">
  <p>
    <a align="center" href="" target="_blank">
      <img
        width="850"
        src="https://media.roboflow.com/open-source/autodistill/autodistill-banner.jpg?2"
      >
    </a>
  </p>
  
[notebooks](https://github.com/roboflow/notebooks) | [inference](https://github.com/roboflow/inference) | [autodistill](https://github.com/autodistill/autodistill) | [collect](https://github.com/roboflow/roboflow-collect)

[![version](https://badge.fury.io/py/autodistill.svg?)](https://badge.fury.io/py/autodistill)
[![downloads](https://img.shields.io/pypi/dm/autodistill)](https://pypistats.org/packages/autodistill)
[![license](https://img.shields.io/pypi/l/autodistill?)](https://github.com/autodistill/autodistill/blob/main/LICENSE)
[![python-version](https://img.shields.io/pypi/pyversions/autodistill)](https://badge.fury.io/py/autodistill)
[![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/roboflow-ai/notebooks/blob/main/notebooks/how-to-auto-train-yolov8-model-with-autodistill.ipynb)
</div>

Autodistill uses big, slower foundation models to train small, faster supervised models. Using `autodistill`, you can go from unlabeled images to inference on a custom model running at the edge with no human intervention in between.

<div align="center">
  <p>
    <a align="center" href="" target="_blank">
      <img
        width="850"
        src="https://media.roboflow.com/open-source/autodistill/steps.jpg"
      >
    </a>
  </p>
</div>

Currently, `autodistill` supports vision tasks like object detection and instance segmentation, but in the future it can be expanded to support language (and other) models.

## 🔗 Quicklinks

| [Tutorial](https://blog.roboflow.com/autodistill)| [Docs](https://docs.autodistill.com)| [Supported Models](#-available-models)  | [Contribute](CONTRIBUTING.md)
|:---:|:---:|:---:|:---:|

## 👀 Example Output

Here are example predictions of a Target Model detecting milk bottles and bottlecaps after being trained on an auto-labeled dataset using Autodistill (see [the Autodistill YouTube video](https://www.youtube.com/watch?v=gKTYMfwPo4M) for a full walkthrough):

<div align="center">
  <p>
    <a align="center" href="https://www.youtube.com/watch?v=gKTYMfwPo4M" target="_blank">
      <img
        width="850"
        src="https://media.roboflow.com/open-source/autodistill/milk-480.gif"
      >
    </a>
  </p>
</div>

## 🚀 Features

* 🔌 Pluggable interface to connect models together
* 🤖 Automatically label datasets
* 🐰 Train fast supervised models
* 🔒 Own your model
* 🚀 Deploy distilled models to the cloud or the edge

## 📚 Basic Concepts

To use `autodistill`, you input unlabeled data into a Base Model which uses an Ontology to label a Dataset that is used to train a Target Model which outputs a Distilled Model fine-tuned to perform a specific Task.

<div align="center">
  <p>
    <a align="center" href="" target="_blank">
      <img
        width="850"
        src="https://media.roboflow.com/open-source/autodistill/overview.jpg"
      >
    </a>
  </p>
</div>

Autodistill defines several basic primitives:

* **Task** - A Task defines what a Target Model will predict. The Task for each component (Base Model, Ontology, and Target Model) of an `autodistill` pipeline must match for them to be compatible with each other. Object Detection and Instance Segmentation are currently supported through the `detection` task. `classification` support will be added soon.
* **Base Model** - A Base Model is a large foundation model that knows a lot about a lot. Base models are often multimodal and can perform many tasks. They're large, slow, and expensive. Examples of Base Models are GroundedSAM and GPT-4's upcoming multimodal variant. We use a Base Model (along with unlabeled input data and an Ontology) to create a Dataset.
* **Ontology** - an Ontology defines how your Base Model is prompted, what your Dataset will describe, and what your Target Model will predict. A simple Ontology is the `CaptionOntology` which prompts a Base Model with text captions and maps them to class names. Other Ontologies may, for instance, use a CLIP vector or example images instead of a text caption.
* **Dataset** - a Dataset is a set of auto-labeled data that can be used to train a Target Model. It is the output generated by a Base Model.
* **Target Model** - a Target Model is a supervised model that consumes a Dataset and outputs a distilled model that is ready for deployment. Target Models are usually small, fast, and fine-tuned to perform a specific task very well (but they don't generalize well beyond the information described in their Dataset). Examples of Target Models are YOLOv8 and DETR.
* **Distilled Model** - a Distilled Model is the final output of the `autodistill` process; it's a set of weights fine-tuned for your task that can be deployed to get predictions.

## 💡 Theory and Limitations

Human labeling is one of the biggest barriers to broad adoption of computer vision. It can take thousands of hours to craft a dataset suitable for training a production model. The process of distillation for training supervised models is not new, in fact, traditional human labeling is just another form of distillation from an extremely capable Base Model (the human brain 🧠).


Foundation models know a lot about a lot, but for production we need models that know a lot about a little.

As foundation models get better and better they will increasingly be able to augment or replace humans in the labeling process. We need tools for steering, utilizing, and comparing these models. Additionally, these foundation models are big, expensive, and often gated behind private APIs. For many production use-cases, we need models that can run cheaply and in realtime at the edge.

<div align="center">
  <p>
    <a align="center" href="" target="_blank">
      <img
        width="850"
        src="https://media.roboflow.com/open-source/autodistill/connections.jpg"
      >
    </a>
  </p>
</div>

Autodistill's Base Models can already create datasets for many common use-cases (and through creative prompting and few-shotting we can expand their utility to many more), but they're not perfect yet. There's still a lot of work to do; this is just the beginning and we'd love your help testing and expanding the capabilities of the system!

## 💿 Installation

Autodistill is modular. You'll need to install the `autodistill` package (which defines the interfaces for the above concepts) along with [Base Model and Target Model plugins](#-available-models) (which implement specific models).

By packaging these separately as plugins, dependency and licensing incompatibilities are minimized and new models can be implemented and maintained by anyone.

Example: 
```bash
pip install autodistill autodistill-grounded-sam autodistill-yolov8
```

<details close>
<summary>Install from source</summary>

You can also clone the project from GitHub for local development:

```bash
git clone https://github.com/roboflow/autodistill
cd autodistill
pip install -e .
```
</details>

Additional Base and Target models are [enumerated below](#-available-models).

## 🚀 Quickstart

See the [demo Notebook](https://colab.research.google.com/github/roboflow-ai/notebooks/blob/main/notebooks/how-to-auto-train-yolov8-model-with-autodistill.ipynb) for a quick introduction to `autodistill`. This notebook walks through building a milk container detection model with no labeling.

Below, we have condensed key parts of the notebook for a quick introduction to `autodistill`.

You can also run Autodistill in one command. First, install `autodistill`:

```bash
pip install autodistill
```

Then, run:

```bash
autodistill images --base="grounding_dino" --target="yolov8" --ontology '{"prompt": "label"}' --output="./dataset"
```

This command will label all images in a directory called `images` with Grounding DINO and use the labeled images to train a YOLOv8 model. Grounding DINO will label all images with the "prompt" and save the label as the "label". You can specify as many prompts and labels as you want. The resulting dataset will be saved in a folder called `dataset`.

### Install Packages

For this example, we'll show how to distill [GroundedSAM](https://github.com/IDEA-Research/Grounded-Segment-Anything) into a small [YOLOv8](https://github.com/ultralytics/ultralytics) model using [autodistill-grounded-sam](https://github.com/autodistill/autodistill-grounded-sam) and [autodistill-yolov8](https://github.com/autodistill/autodistill-yolov8).

```
pip install autodistill autodistill-grounded-sam autodistill-yolov8
```

### Distill a Model

```python
from autodistill_grounded_sam import GroundedSAM
from autodistill.detection import CaptionOntology
from autodistill_yolov8 import YOLOv8

# define an ontology to map class names to our GroundingDINO prompt
# the ontology dictionary has the format {caption: class}
# where caption is the prompt sent to the base model, and class is the label that will
# be saved for that caption in the generated annotations
base_model = GroundedSAM(ontology=CaptionOntology({"shipping container": "container"}))

# label all images in a folder called `context_images`
base_model.label(
  input_folder="./images",
  output_folder="./dataset"
)

target_model = YOLOv8("yolov8n.pt")
target_model.train("./dataset/data.yaml", epochs=200)

# run inference on the new model
pred = target_model.predict("./dataset/valid/your-image.jpg", confidence=0.5)
print(pred)

# optional: upload your model to Roboflow for deployment
from roboflow import Roboflow

rf = Roboflow(api_key="API_KEY")
project = rf.workspace().project("PROJECT_ID")
project.version(DATASET_VERSION).deploy(model_type="yolov8", model_path=f"./runs/detect/train/")
```

<details close>
<summary>Visualize Predictions</summary>

To plot the annotations for a single image using `autodistill`, you can use the code below. This code is helpful to visualize the annotations generated by your base model (i.e. GroundedSAM) and the results from your target model (i.e. YOLOv8).

```python
import supervision as sv
import cv2

img_path = "./images/your-image.jpeg"

image = cv2.imread(img_path)

detections = base_model.predict(img_path)
# annotate image with detections
box_annotator = sv.BoxAnnotator()

labels = [
    f"{base_model.ontology.classes()[class_id]} {confidence:0.2f}"
    for _, _, confidence, class_id, _ in detections
]

annotated_frame = box_annotator.annotate(
    scene=image.copy(), detections=detections, labels=labels
)

sv.plot_image(annotated_frame, (16, 16))
```
</details>

## 📍 Available Models

Our goal is for `autodistill` to support using all foundation models as Base Models and most SOTA supervised models as Target Models. We focused on object detection and segmentation
tasks first but plan to launch classification support soon! In the future, we hope `autodistill` will also be used for models beyond computer vision.

* ✅ - complete (click row/column header to go to repo)
* 🚧 - work in progress

### object detection

| base / target | [YOLOv8](https://github.com/autodistill/autodistill-yolov8) | [YOLO-NAS](https://github.com/autodistill/autodistill-yolonas) | [YOLOv5](https://github.com/autodistill/autodistill-yolov5) | [DETR](https://github.com/autodistill/autodistill-detr) | YOLOv6 | YOLOv7 | MT-YOLOv6 |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| [DETIC](https://github.com/autodistill/autodistill-detic) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [GroundedSAM](https://github.com/autodistill/autodistill-grounded-sam) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [GroundingDINO](https://github.com/autodistill/autodistill-grounding-dino) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [OWL-ViT](https://github.com/autodistill/autodistill-owl-vit) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [SAM-CLIP](https://github.com/autodistill/autodistill-sam-clip) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [LLaVA-1.5](https://github.com/autodistill/autodistill-llava) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [Kosmos-2](https://github.com/autodistill/autodistill-kosmos-2) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [OWLv2](https://github.com/autodistill/autodistill-owlv2) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [Roboflow Universe Models (50k+ pre-trained models)](https://github.com/autodistill/autodistill-roboflow-universe) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [CoDet](https://github.com/autodistill/autodistill-codet) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [Azure Custom Vision](https://github.com/autodistill/autodistill-azure-vision) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [AWS Rekognition](https://github.com/autodistill/autodistill-rekognition) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |
| [Google Vision](https://github.com/autodistill/autodistill-gcp-vision) | ✅ | ✅ | ✅ | ✅ | 🚧 |  |  |


### instance segmentation

| base / target | [YOLOv8](https://github.com/autodistill/autodistill-yolov8) | [YOLO-NAS](https://github.com/autodistill/autodistill-yolonas) | [YOLOv5](https://github.com/autodistill/autodistill-yolov5) | YOLOv7 | Segformer |
|:---:|:---:|:---:|:---:|:---:|:---:|
| [GroundedSAM](https://github.com/autodistill/autodistill-grounded-sam) | ✅ | 🚧 | 🚧 |  |  |
| SAM-CLIP | ✅ | 🚧 | 🚧 |  |  |
| SegGPT | ✅ | 🚧 | 🚧 |  |  |
| FastSAM | 🚧 | 🚧 | 🚧 |  |  |


### classification

| base / target | [ViT](https://github.com/autodistill/autodistill-vit) | [YOLOv8](https://github.com/autodistill/autodistill-yolov8) | [YOLOv5](https://github.com/autodistill/autodistill-yolov5) |
|:---:|:---:|:---:|:---:|
| [CLIP](https://github.com/autodistill/autodistill-clip) | ✅ | ✅ | 🚧 |
| [MetaCLIP](https://github.com/autodistill/autodistill-metaclip) | ✅ | ✅ | 🚧 |
| [DINOv2](https://github.com/autodistill/autodistill-dinov2) | ✅ | ✅ | 🚧 |
| [BLIP](https://github.com/autodistill/autodistill-blip) | ✅ | ✅ | 🚧 |
| [ALBEF](https://github.com/autodistill/autodistill-albef) | ✅ | ✅ | 🚧 |
| [FastViT](https://github.com/autodistill/autodistill-fastvit) | ✅ | ✅ | 🚧 |
| [AltCLIP](https://github.com/autodistill/autodistill-altcip) | ✅ | ✅ | 🚧 |
| Fuyu | 🚧 | 🚧 | 🚧 |
| Open Flamingo | 🚧 | 🚧 | 🚧 |
| GPT-4 |  |  |  |
| PaLM-2 |  |  |  |


## Roboflow Model Deployment Support

You can optionally deploy some Target Models trained using Autodistill on Roboflow. Deploying on Roboflow allows you to use a range of concise SDKs for using your model on the edge, from [roboflow.js](https://docs.roboflow.com/inference/web-browser) for web deployment to [NVIDIA Jetson](https://docs.roboflow.com/inference/nvidia-jetson) devices.

The following Autodistill Target Models are supported by Roboflow for deployment:

| model name | Supported? |
|:---:|:---:|
| YOLOv8 Object Detection | ✅ |
| YOLOv8 Instance Segmentation | ✅ |
| YOLOv5 Object Detection | ✅ |
| YOLOv5 Instance Segmentation | ✅ |
| YOLOv8 Classification |  |

## 🎬 Video Guides

<p align="left">
<a href="https://www.youtube.com/watch?v=gKTYMfwPo4M" title="Autodistill: Train YOLOv8 with ZERO Annotations"><img src="https://i.ytimg.com/vi/gKTYMfwPo4M/maxresdefault.jpg" alt="Autodistill: Train YOLOv8 with ZERO Annotations" width="300px" align="left" /></a>
<a href="https://youtu.be/oEQYStnF2l8"><strong>Autodistill: Train YOLOv8 with ZERO Annotations</strong></a>
<div><strong>Published: 8 June 2023</strong></div>
<br/>In this video, we will show you how to use a new library to train a YOLOv8 model to detect bottles moving on a conveyor line. Yes, that's right - zero annotation hours are required! We dive deep into Autodistill's functionality, covering topics from setting up your Python environment and preparing your images, to the thrilling automatic annotation of images. </p> 

## 💡 Community Resources

- [Distill Large Vision Models into Smaller, Efficient Models with Autodistill](https://blog.roboflow.com/autodistill/): Announcement post with written guide on how to use Autodistill
- [Comparing AI-Labeled Data to Human-Labeled Data](https://blog.roboflow.com/ai-vs-human-labeled-data/): A qualitative evaluation of Grounding DINO used with Autodistill across various tasks and domains.
- [How to Evaluate Autodistill Prompts with CVevals](https://blog.roboflow.com/autodistill-prompt-evaluation/): Evaluate Autodistill prompts.
- [Autodistill: Label and Train a Computer Vision Model in Under 20 Minutes](https://www.youtube.com/watch?v=M_QZ_Q0zT0k): Building a model to detect planes in under 20 minutes.
- [Comparing AI-Labeled Data to Human-Labeled Data](https://blog.roboflow.com/ai-vs-human-labeled-data/): Explore the strengths and limitations of a base model used with Autoditsill.
- [Train an Image Classification Model with No Labeling](https://blog.roboflow.com/train-classification-model-no-labeling/): Use Grounded SAM to automatically label images for training an Ultralytics YOLOv8 classification model.
- [Train a Segmentation Model with No Labeling](https://blog.roboflow.com/train-a-segmentation-model-no-labeling/): Use CLIP to automatically label images for training an Ultralytics YOLOv8 segmentation model.
- File a PR to add your own resources here!

## 🗺️ Roadmap

Apart from adding new models, there are several areas we plan to explore with `autodistill` including:

* 💡 Ontology creation & prompt engineering
* 👩‍💻 Human in the loop support
* 🤔 Model evaluation
* 🔄 Active learning
* 💬 Language tasks

## 🏆 Contributing

We love your input! Please see our [contributing guide](CONTRIBUTING.md) to get started. Thank you 🙏 to all our contributors!

## 👩‍⚖️ License

The `autodistill` package is licensed under an [Apache 2.0](LICENSE). Each Base or Target model plugin may use its own license corresponding with the license of its underlying model. Please refer to the license in each plugin repo for more information.

## Frequently Asked Questions ❓

### What causes the `PytorchStreamReader failed reading zip archive: failed finding central directory` error?

This error is caused when PyTorch cannot load the model weights for a model. Go into the `~/.cache/autodistill` directory and delete the folder associated with the model you are trying to load. Then, run your code again. The model weights will be downloaded from scratch. Leave the installation process uninterrupted.

## 💻 explore more Roboflow open source projects

|Project | Description|
|:---|:---|
|[supervision](https://roboflow.com/supervision) | General-purpose utilities for use in computer vision projects, from predictions filtering and display to object tracking to model evaluation.
|[Autodistill](https://github.com/autodistill/autodistill) (this project) | Automatically label images for use in training computer vision models. |
|[Inference](https://github.com/roboflow/inference) | An easy-to-use, production-ready inference server for computer vision supporting deployment of many popular model architectures and fine-tuned models.
|[Notebooks](https://roboflow.com/notebooks) | Tutorials for computer vision tasks, from training state-of-the-art models to tracking objects to counting objects in a zone.
|[Collect](https://github.com/roboflow/roboflow-collect) | Automated, intelligent data collection powered by CLIP.

<br>

<div align="center">

  <div align="center">
      <a href="https://youtube.com/roboflow">
          <img
            src="https://media.roboflow.com/notebooks/template/icons/purple/youtube.png?ik-sdk-version=javascript-1.4.3&updatedAt=1672949634652"
            width="3%"
          />
      </a>
      <img src="https://raw.githubusercontent.com/ultralytics/assets/main/social/logo-transparent.png" width="3%"/>
      <a href="https://roboflow.com">
          <img
            src="https://media.roboflow.com/notebooks/template/icons/purple/roboflow-app.png?ik-sdk-version=javascript-1.4.3&updatedAt=1672949746649"
            width="3%"
          />
      </a>
      <img src="https://raw.githubusercontent.com/ultralytics/assets/main/social/logo-transparent.png" width="3%"/>
      <a href="https://www.linkedin.com/company/roboflow-ai/">
          <img
            src="https://media.roboflow.com/notebooks/template/icons/purple/linkedin.png?ik-sdk-version=javascript-1.4.3&updatedAt=1672949633691"
            width="3%"
          />
      </a>
      <img src="https://raw.githubusercontent.com/ultralytics/assets/main/social/logo-transparent.png" width="3%"/>
      <a href="https://docs.roboflow.com">
          <img
            src="https://media.roboflow.com/notebooks/template/icons/purple/knowledge.png?ik-sdk-version=javascript-1.4.3&updatedAt=1672949634511"
            width="3%"
          />
      </a>
      <img src="https://raw.githubusercontent.com/ultralytics/assets/main/social/logo-transparent.png" width="3%"/>
      <a href="https://disuss.roboflow.com">
          <img
            src="https://media.roboflow.com/notebooks/template/icons/purple/forum.png?ik-sdk-version=javascript-1.4.3&updatedAt=1672949633584"
            width="3%"
          />
      <img src="https://raw.githubusercontent.com/ultralytics/assets/main/social/logo-transparent.png" width="3%"/>
      <a href="https://blog.roboflow.com">
          <img
            src="https://media.roboflow.com/notebooks/template/icons/purple/blog.png?ik-sdk-version=javascript-1.4.3&updatedAt=1672949633605"
            width="3%"
          />
      </a>
      </a>
  </div>

</div>
