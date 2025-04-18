---
layout: post
title: "Co-Localization for Planetary Rover Teams"
subtitle: "Astrobotic Technology"
category: Web & Apps
src: "assets/2021-05-10-rover-teams/astrobotic_logo.webp"
---
<figure>
  <img src="/assets/2021-05-10-rover-teams/feature_matching_crop.webp" alt="ORB Features">
  <figcaption>Image features detected with ORB on Astrobotic's Peregrine Lander</figcaption>
</figure>

### The Promise of Cooperative Exploration

Cooperative robots can explore the surface of planets with higher efficiency and lower mission risk, perform novel and precise resource and science surveys, and gather and share resources and information with other assets to bring planetary exploration. In order to work together more efficiently and effectively, robots must understand their location relative to their peers.

### The Localization Challenge

This is challenged in planetary exploration by the fact that these environments lack global positioning systems to enable a robot to understand its absolute location in space. State-of-the-art simultaneous localization and mapping (SLAM) techniques can accurately localize without explicit pose sensing, but also require high-end range sensors, high-fidelity vision, and powerful onboard computing.

Adding these computing and sensor demands on paired and multi-agent systems begins to defeat the purpose - paired exploration is advantageous precisely because it can be used to field more minimalist robots that can devote energy to rapid traverse, multi-angle inspections, or specific scientific instruments.

### Proposed Work

The proposed work will develop two key techniques to improve the foundation for cooperative planetary robotic missions:

1.  **Relative Co-localization:** Novel methods for co-localizing multiple robots using relative observations.
2.  **Uncertainty-Reducing Path Planning:** Methods for planning multi-robot paths that reduce localization uncertainty and improve positioning accuracy of robot teams.

### Benefits

This research will enable more accurate localization of multiple planetary exploration robots without requiring high-fidelity sensing and powerful compute.