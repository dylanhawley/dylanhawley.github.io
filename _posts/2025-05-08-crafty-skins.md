---
layout: post
title: "Crafty Skins AI"
subtitle: "Diffusion-Generated Minecraft Skins"
category: Web & Apps
src: "assets/2025-05-08-crafty-skins/thumbnail.webp"
---

> Note: This project is a work in progress.

The goal of this project is to leverage today's powerful image generation models to create a *usable* minecraft player skin. I emphasize *usable* to show this is not a problem than can be trivially solved with a commercial off-the-shelf image generation models, such as Midjourney or OpenAI's DALL-E. Asking one of these models for a Minecraft skin, no matter how well you engineer the prompt, will at best return a render of a semi-realistic minecraft skin. To be able to load a skin in game, the skin file must precisely follow a 64 x 64 px or 64 x 32 px *uv map* layout. See the [technical specification for Minecraft skins](https://github.com/minotar/skin-spec).

<!-- toc -->
 - [Strategy](#strategy)
 - [Dataset Generation](#dataset-generation)
   - [Scraping Skins from the Web](#scraping-skins-from-the-web)
   - [Generating the Photorealistic Images](#generating-the-photorealistic-images)
 - [Experiments](#experiments)
   - [SDXL IP Adapter Scale Experiment (30 steps)](#sdxl-ip-adapter-scale-experiment-100-steps)
   - [SDXL IP Adapter Scale Experiment (100 steps)](#sdxl-ip-adapter-scale-experiment-100-steps)
 - [Building a Fast Inference Server](#building-a-fast-inference-server)

## Strategy
When I first set out on this project, I wanted to fine-tune the **Img2Img** pipeline of Stable Diffusion to preserve the likeness of the person or character in the input image through the model to the output skin. Most of the subsequent work details my *many* attempts at this problem. After a lot of learning, I decided to shift the design to fine-tune the **Text2Img** pipeline instead, but most of this article will be written about my work before the pivot.

## Dataset Generation

There does not exist a dataset which precisely maps photorealistic images of a subject to their Minecraft skin counterpart, so we will have to make one.

### Scraping Skins from the Web

I scraped over 1000 skins and their descriptions from [MinecraftSkins.net](https://www.minecraftskins.net/), and subsequently trimmed off a few bad examples to settle on a modest dataset of 962 samples.

### Generating the Photorealistic Images

We can query the OpenAI API with a rendered image of our skin file and a textual description of the skin. With our multimodal prompt, we ask `gpt-4o` to use **DALL-E** to return a photorealistic render of what it intuits to be the subject.

I wrote a script to repeat this process for every sample in the dataset, then used the Huggingface SDK to convert the dataset to parquet format, and [uploaded it to my Huggingface](https://huggingface.co/datasets/deepwaterhorizon/minecraft-skins-legacy). See the interactive preview below.

<iframe
  src="https://huggingface.co/datasets/deepwaterhorizon/minecraft-skins-legacy/embed/viewer/default/train"
  frameborder="0"
  width="100%"
  height="560px"
></iframe>

## Experiments

### SDXL IP Adapter Scale Experiment (30 steps)

**Base Model**: [monadical-labs/minecraft-skin-generator-sdxl](https://huggingface.co/monadical-labs/minecraft-skin-generator-sdxl)  
**IP-Adapter**: [h94/IP-Adapter/sdxl_models/ip-adapter_sdxl.bin](https://huggingface.co/h94/IP-Adapter/tree/main/sdxl_models)  
**Prompt**: ""  

<details>
<summary markdown="span">Evaluation Image Names</summary>
- data/raw/input/13thdoctor.png
- data/raw/input/3dglasses.png
- data/raw/input/alberteinstein.png
- data/raw/input/balloonboy.png
- data/raw/input/animalcrossingvillager.png
- data/raw/input/arabman.png
- data/raw/input/berlioz.png
- data/raw/input/countrykitty.png
- data/raw/input/creeperboy.png
- data/raw/input/daughterofevil.png
- data/raw/input/dawnpokemon.png
- data/raw/input/demonicmutation.png
- data/raw/input/doctorstrange.png
- data/raw/input/donaldsuit.png
- data/raw/input/doratheexplorer.png
- data/raw/input/frontman.png
</details>

![Composite Figure]({{ site.baseurl }}/assets/2025-05-08-crafty-skins/ip_adapter_scale_comparison_30_steps.webp)

### SDXL IP Adapter Scale Experiment (100 steps)

**Base Model**: [monadical-labs/minecraft-skin-generator-sdxl](https://huggingface.co/monadical-labs/minecraft-skin-generator-sdxl)  
**IP-Adapter**: [h94/IP-Adapter/sdxl_models/ip-adapter_sdxl.bin](https://huggingface.co/h94/IP-Adapter/tree/main/sdxl_models)  
**Prompt**: ""  

<details>
<summary markdown="span">Evaluation Image Names</summary>
- data/raw/input/13thdoctor.png
- data/raw/input/3dglasses.png
- data/raw/input/alberteinstein.png
- data/raw/input/balloonboy.png
- data/raw/input/animalcrossingvillager.png
- data/raw/input/arabman.png
- data/raw/input/berlioz.png
- data/raw/input/countrykitty.png
- data/raw/input/creeperboy.png
- data/raw/input/daughterofevil.png
- data/raw/input/dawnpokemon.png
- data/raw/input/demonicmutation.png
- data/raw/input/doctorstrange.png
- data/raw/input/donaldsuit.png
- data/raw/input/doratheexplorer.png
- data/raw/input/frontman.png
</details>

![Composite Figure]({{ site.baseurl }}/assets/2025-05-08-crafty-skins/ip_adapter_scale_comparison_100_steps.webp)

## Building a Fast Inference Server

While not at all necessary, I think it would be interesting to try and make inference run as fast as possible. This is a fun side project, after all.
A few ways to speed up inference:
 - Distillation (lossy)
   - Adversarial Diffustion Distillation. Implement teacher-student pipeline to teach the distilled model to generate images with very few denoising steps.
   - LCM-LoRA is a distillation LoRA which can be dropped in on top of base diffusion model.
 - Hardware optimization
   - NVIDIA TensorRT

Optimizing Stable Diffusion for TensorRT has proven to be difficult enough to shelve this part of the project for now. I ran into issue - some hardware dependent (limited support for the RTX 5090's `sm_90` module right now), and some software dependent (running nvidia docker in WSL2 sometimes does share CUDA `.so` files with the running container). The resources refenced for TensorRT optimizations and my notes are stored in the expanble section below.

<details>
<summary markdown="span" style="font-size: 1.5em">Resources for future self</summary>

 - [NVIDIA Triton Huggingface Tutorial (NVIDIA Triton GitHub)](https://github.com/triton-inference-server/tutorials/tree/main/HuggingFace): Described the two methods of deploying a model pipline. Single pipeline, or break apart and use TensorRT. Example uses an LLM - not Diffusion model.  
   - [Part 4 - Inference Accceleration](https://github.com/triton-inference-server/tutorials/blob/main/Conceptual_Guide/Part_4-inference_acceleration/README.md): Has useful flowchart and describes using TensorRT's integration with PyTorch/TensorFlow  
   - [HuggingFace ONNX](https://huggingface.co/docs/transformers/en/serialization)
   - [HuggingFace Export to ONNX](https://huggingface.co/docs/transformers/v4.43.2/en/serialization): Not sure if only for transformers.
   - [HuggingFace Diffusers Export to ONNX](https://huggingface.co/docs/diffusers/v0.35.1/en/optimization/onnx#onnx-runtime): To export the pipeline in the ONNX format offline and use it later for inference, use the optimum-cli export command:
     ```sh
     optimum-cli export onnx --model stable-diffusion-v1-5/stable-diffusion-v1-5 sd_v15_onnx/
     ```
     
     Then to perform inference (you don't have to specify export=True again):
     
     ```python
     from optimum.onnxruntime import ORTStableDiffusionPipeline

     model_id = "sd_v15_onnx"
     pipeline = ORTStableDiffusionPipeline.from_pretrained(model_id)
     prompt = "sailing ship in storm by Leonardo da Vinci"
     image = pipeline(prompt).images[0]
     ```
 - [Speed-up Stable Diffusion with TensorRT](https://www.photoroom.com/inside-photoroom/stable-diffusion-25-percent-faster-and-save-seconds)  
 - [AWS Fast Model Loader in Sagemake Inference](https://aws.amazon.com/blogs/machine-learning/introducing-fast-model-loader-in-sagemaker-inference-accelerate-autoscaling-for-your-large-language-models-llms-part-1/): Seems proprietary. Load model weights from s3 instead.  
 - [TensorRT Docs Capabilities](https://docs.nvidia.com/deeplearning/tensorrt/latest/architecture/capabilities.html): The TensorRT Builder optimizes a model and produces and Engine (offline). The `NetworkDefinition` interface defines the model. The most common path to transfer a model to TensorRT is to export it from a framework in ONNX format and use TensorRT’s ONNX parser to populate the network definition. The `BuilderConfig` interface is used to specify how TensorRT should optimize the model. Allows you to reduce precision, tradeoff memory for speed, etc. With `NetworkDefinition` and `BuilderConfig`, `Builder` can now create the engine. The Builder creates the Engine in a serialized form called a `Plan`. Engines are specific to the TensorRT version and the GPU which they were created. During runtime execution, we deserialize a plan to create and engine and create an execution context from the engine. `ExecutionContext`, created from the engine, is the main interface for invoking inference. Contains all states accosiated with an inference. `BuilderFlag` allow TensorRT to select lower-precision implementations.
  - [Deploy TensorRT to Triton](https://github.com/NVIDIA/TensorRT/tree/main/quickstart/deploy_to_triton)
  - [40% Faster Stable Diffusion XL with TensorRT](https://www.baseten.co/blog/40-faster-stable-diffusion-xl-inference-with-nvidia-tensorrt/#sdxl-with-tensorrt-in-production): Baseten has some good articles.  
 - [Deploying Stable Diffusion Models with Triton and TensorRT](https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/tutorials/Popular_Models_Guide/StableDiffusion/README.html)
 - [Stable Diffusion Triton Server (kamalkraj GitHub)](https://github.com/kamalkraj/stable-diffusion-tritonserver/tree/master): Seems quite useful. Small repository and may be incomplete.  
 - <del>[TensorRT Diffusion Demo (NVIDIA TensorRT GitHub)](https://github.com/NVIDIA/TensorRT/tree/release/10.13.3/demo/Diffusion): Has issue running on the RTX 5090?</del>

    ```sh
    # Running on deepblue with RTX 5090
    $ python3 demo_txt2img_xl.py "man wearing a hat" --version xl-1.0
    [I] Optimizing ONNX model: onnx/clip2.opt/model.onnx
    [I] Folding Constants | Pass 1
    ['\x1b[38;5;11m'][W] Inference failed. You may want to try enabling partitioning to see better results. Note: Error was:
    [ONNXRuntimeError] : 1 : FAIL : /onnxruntime_src/onnxruntime/core/graph/model.cc:181 onnxruntime::Model::Model(onnx::ModelProto&&, const onnxruntime::PathString&, const onnxruntime::IOnnxRuntimeOpSchemaRegistryList*, const onnxruntime::logging::Logger&, const onnxruntime::ModelOptions&) Unsupported model IR version: 12, max supported IR version: 10


    # Running on Runpod with A6000 48GB GPU
    [I] Loading bytes from engine/vae.trt10.13.3.9.plan
    Loading TensorRT engine from bytes: engine/vae.trt10.13.3.9.plan
    [E] [defaultAllocator.cpp::allocate::57] Error Code 1: Cuda Runtime (In allocate at common/dispatch/defaultAllocator.cpp:57)
    [W] Requested amount of GPU memory (20132809216 bytes) could not be allocated. There may not be enough free memory for allocation to succeed.
    [E] [executionContext.cpp::initializeExecutionContext::625] Error Code 2: OutOfMemory (Requested size was 20132809216 bytes.)
    [W]: engine/unetxl.trt10.13.3.9.plan: Could not find 'timestep' in shape dict {'sample': (2, 4, 128, 128), 'encoder_hidden_states': (2, 77, 2048), 'latent': (2, 4, 128, 128), 'text_embeds': (2, 1280), 'time_ids': (2, 6)}.  Using shape (1,) inferred from the engine.
    Traceback (most recent call last):
      File "/workspace/TensorRT/demo/Diffusion/demo_txt2img_xl.py", line 150, in <module>
        demo.loadResources(args.height, args.width, args.batch_size, args.seed)
      File "/workspace/TensorRT/demo/Diffusion/demo_txt2img_xl.py", line 73, in loadResources
        self.base.loadResources(image_height, image_width, batch_size, seed)
      File "/workspace/TensorRT/demo/Diffusion/demo_diffusion/pipeline/stable_diffusion_pipeline.py", line 275, in loadResources
        self.engine[model_name].allocate_buffers(shape_dict=obj.get_shape_dict(batch_size, image_height, image_width), device=self.device)
      File "/workspace/TensorRT/demo/Diffusion/demo_diffusion/engine.py", line 301, in allocate_buffers
        self.context.set_input_shape(name, shape)
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    AttributeError: 'NoneType' object has no attribute 'set_input_shape'
    ```

### Exporting model to ONNX
```sh
pip install optimum[onnx] onnx onnxruntime
optimum-cli export onnx --model deepwaterhorizon/minecraft-skin-model sd_mc_onnx/
```
</details>