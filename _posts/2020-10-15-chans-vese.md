---
layout: post
title: "Modified Chans-Vese Active Contours"
subtitle: "Image Segmentation Algorithm"
category: Web & Apps
src: "assets/images/active_contour.png"
---
![Chans-Vese](/assets/images/chans_vese.png)
<p>A popular method to represent a textured region is via the use of what is known as a structure tensor descriptor. Given an image, I(x,y), the structure tensor at each pixel is a (2,2) matrix formed by, W(x,y) ∗ ∇I(x,y)∇I(x,y)t, where W(x,y) is a known Kernel for e.g., a Gaussian Kernel. The structure tensor captures neighborhood information at each pxiel for instance, the dominant directional characteristics within the neighborhood. Our goal then is to segment this tensor field using the Chan-Vesse active contour model given by the following variational principle.</p>
![Chans-Vese](/assets/images/variational_principal.png)