---
layout: page
is_project: true
title: Root system phenotyping using CT scan
description: Phenotyping root systems is challenging due to their hidden nature in soil. Traditional [shovelomics](https://link.springer.com/article/10.1007/s11104-010-0623-8) are destructive and do not preserve true root morphology or facilitate root-soil interaction studies. To address these limitations, we employ CT scanning for 3D root reconstruction. However, the resolution of modern CT scanners is often inadequate for reconstructing fine root systems within the soil. Alternatively, multiview stereo techniques post-shovelomics can reconstruct detailed root systems. We hypothesize that CT-reconstructed roots maintain authentic morphologies, while extracted reconstructions offer detailed phenomics. By aligning these reconstructions, we aim to depict detailed root systems with near-authentic morphologies, enhancing our understanding of root dynamics and environmental interactions.
img: assets/gif/ct_scan_slices.gif
importance: 1
category: work
related_publications: false
---

<style>
.same-height {
    height: 250px; /* Set your desired uniform height */
    width: auto;   /* Allows width to adjust to maintain aspect ratio */
    max-width: 100%; /* Ensures images don't exceed their container's width */
}
</style>


<div class="row">
    <!-- First Column -->
    <div class="col-sm mt-3 mt-md-0">
        <!-- Image Container -->
        <div class="image-container d-flex justify-content-center">
            {% include figure.liquid 
                loading="eager" 
                path="assets/img/ct/ct_scanner.jpg" 
                title="strawberry counting" 
                class="img-fluid rounded z-depth-1 same-height" 
            %}
        </div>
        <!-- Caption -->
        <div class="caption text-center mt-2">
            CT scanning.
        </div>
    </div>

    <!-- Second Column -->
    <div class="col-sm mt-3 mt-md-0">
        <!-- Image Container -->
        <div class="image-container d-flex justify-content-center">
            {% include figure.liquid 
                loading="eager" 
                path="assets/gif/ct_scan_slices.gif" 
                title="tomato counting" 
                class="img-fluid rounded z-depth-1 same-height" 
            %}
        </div>
        <!-- Caption -->
        <div class="caption text-center mt-2">
            CT slice data.
        </div>
    </div>

    <!-- Third Column -->
    <div class="col-sm mt-3 mt-md-0">
        <!-- Image Container -->
        <div class="image-container d-flex justify-content-center">
            {% include figure.liquid 
                loading="eager" 
                path="assets/gif/ct_rotation_demo.gif" 
                title="tomato counting" 
                class="img-fluid rounded z-depth-1 same-height" 
            %}
        </div>
        <!-- Caption -->
        <div class="caption text-center mt-2">
            Traced root system architecture within soil.
        </div>
    </div>
</div>

We are continuously improving our algorithms to achieve more detailed 3D reconstructions of root system architectures from CT slice data. This work is ongoing, and we encourage you to stay informed by following our future developments.


