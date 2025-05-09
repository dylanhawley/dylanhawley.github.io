---
layout: post
title: "Crafty Skins AI"
subtitle: "Diffusion-Generated Minecraft Skins"
category: Web & Apps
src: "assets/2025-05-08-crafty-skins/thumbnail.webp"
---
<!-- toc -->
 - [Dataset Generation](#dataset-generation)
   - [Scraping Skins from the Web](#scraping-skins-from-the-web)
   - [Generating the Photorealistic Images](#generating-the-photorealistic-images)
 - [Experiments](#experiments)
   - [SDXL IP Adapter Scale Experiment (100 steps)](#sdxl-ip-adapter-scale-experiment-100-steps)
> Note: This page is a work in progress.

## Dataset Generation

There does not exist a dataset which precisely maps photorealistic images of a subject to their Minecraft skin counterpart, so we will have to make one.

### Scraping Skins from the Web

TODO: descrip web scraping process

### Generating the Photorealistic Images

We can query the OpenAI API with a rendered image of our skin file and a textual description of the skin which we scraped earlier. With our enhanced prompt, we ask `gpt-4o` to use **DALL-E** to return a photorealistic render of what it intuits to be the subject.

## Experiments

### SDXL IP Adapter Scale Experiment (100 steps)

#### Models Used

- **SDXL Model**: [monadical-labs/minecraft-skin-generator-sdxl](https://huggingface.co/monadical-labs/minecraft-skin-generator-sdxl)
- **IP-Adapter**: [h94/IP-Adapter/sdxl_models/ip-adapter_sdxl.bin](https://huggingface.co/h94/IP-Adapter/tree/main/sdxl_models)

#### Parameters

- **IP Scales Tested**: [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0]
- **Prompt**: ""
- **Number of Inference Steps**: 100

#### Results

![Composite Figure]({{ site.baseurl }}/assets/2025-05-08-crafty-skins/ip_adapter_scale_comparison.webp)

#### Evaluation Image Names

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
