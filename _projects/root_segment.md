---
layout: page
is_project: true
title: 3D root system segmentation and skeletonization
description: This project develops 3D methods for root system architecture analysis, including primary root segmentation, lateral root separation, skeleton extraction, and downstream trait computation for phenotyping and genotyping studies.
img: assets/gif/skeleton_rotation.gif
importance: 2
category: work
related_publications: false
---

## Overview

Root system architecture (RSA) contains rich structural information that is highly relevant to plant phenotyping and genotyping. To quantify biologically meaningful traits such as root length, branching angle, surface area, and volume, the root system first needs to be decomposed into analyzable units. This is why **3D segmentation** and **3D skeletonization** are critical: they provide the structural basis for downstream trait extraction.

This project presents two connected stages of work toward that goal.

- In the first stage, we developed a pipeline for **primary root segmentation, lateral root separation, and lateral root skeletonization** in relatively simple root systems.
- In the second stage, we extended this idea to **complex soybean root architectures**, where simple segmentation-plus-clustering strategies are no longer sufficient, and introduced a more advanced **bio-inspired tracking framework** for robust skeletonization and phenotyping.

Together, these works move from structural decomposition of root systems to downstream trait analysis for plant phenotyping and genotyping studies.

---

## Segmentation and skeletonization in simple root systems

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/gif/root_input.gif" title="root_input" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Input root system point cloud.
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/gif/root_primary_removed.gif" title="primary_root" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Primary root segmentation and removal.
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/gif/individual_segmented.gif" title="secondary_root" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Individual lateral root segmentation by clustering.
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/gif/secondary_skeleton.gif" title="skeletonization" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Skeletonization of segmented lateral roots.
        </div>
    </div>
</div>


---

## Arabidopsis root system segmentation

<!-- Include three.js and PLYLoader via CDN -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/loaders/PLYLoader.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>

<div class="row mt-4">
    <div class="col-md-5">
        <div id="threejs-container" style="width: 100%; height: 400px; margin-bottom: 2rem;"></div>
    </div>
    <div class="col-md-7">
        <h5>Interactive Arabidopsis root system viewer</h5>
        <p>
            Our earlier work focused on <i>Arabidopsis thaliana</i>, where the root architecture is well suited for developing and validating a structured 3D analysis pipeline.
        </p>
        <p>
            The workflow consists of four main steps:
        </p>
        <ol>
            <li><strong>Segment the primary root from the whole root system</strong></li>
            <li><strong>Remove the primary root from the full point cloud</strong></li>
            <li><strong>Use clustering to separate individual lateral roots</strong></li>
            <li><strong>Apply Laplacian-based skeletonization to each lateral root separately</strong></li>
        </ol>
        <p>
            In this pipeline, the primary root was segmented using <a href="https://github.com/pointcept/pointcept">Point Transformer</a>. Once the primary root was removed, the lateral roots became more spatially separable, making clustering-based individual root segmentation feasible. Each isolated lateral root could then be skeletonized independently, which enabled downstream measurement of traits such as root length and branching angle.
        </p>
        <p>
            This strategy was effective for relatively simple root architectures such as <i>Arabidopsis thaliana</i>, and it established the basis for our later work on more complex soybean root systems.
        </p>
    </div>
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0xffffff);

    const container = document.getElementById('threejs-container');
    const width = container.clientWidth;
    const height = container.clientHeight;

    const camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
    camera.position.set(0, -1.5, 0);

    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(width, height);
    container.appendChild(renderer.domElement);

    const controls = new THREE.OrbitControls(camera, renderer.domElement);
    controls.enableDamping = true;
    controls.dampingFactor = 0.05;

    const ambientLight = new THREE.AmbientLight(0x404040, 2);
    scene.add(ambientLight);

    const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
    directionalLight.position.set(0, 1, 1).normalize();
    scene.add(directionalLight);

    const loader = new THREE.PLYLoader();
    loader.load(
        '{{ "/assets/ply/arabidopsis_root_demo.ply" | relative_url }}',
        function (geometry) {
            geometry.computeVertexNormals();
            geometry.center();

            const material = new THREE.PointsMaterial({
                size: 0.001,
                color: 0xffffff,
                vertexColors: THREE.VertexColors,
                transparent: true,
                opacity: 0.8
            });

            const points = new THREE.Points(geometry, material);
            scene.add(points);
        },
        function (xhr) {
            console.log((xhr.loaded / xhr.total * 100) + '% loaded');
        },
        function (error) {
            console.error('An error happened while loading the PLY file:', error);
        }
    );

    window.addEventListener('resize', function() {
        const newWidth = container.clientWidth;
        const newHeight = container.clientHeight;

        renderer.setSize(newWidth, newHeight);
        camera.aspect = newWidth / newHeight;
        camera.updateProjectionMatrix();
    });

    function animate() {
        requestAnimationFrame(animate);
        controls.update();
        renderer.render(scene, camera);
    }

    animate();
});
</script>

---

## Limitation of the early pipeline

Although the pipeline of

**primary root segmentation → lateral root clustering → individual skeletonization**

worked well for relatively simple root systems, it became much more difficult to apply to **soybean root systems**.

Compared with *Arabidopsis thaliana*, soybean RSA is structurally more complex. Root overlap is more frequent, branch organization is denser, and individual lateral roots are harder to separate cleanly after primary root removal. Under these conditions, simple clustering is no longer sufficiently robust, and downstream skeletonization becomes much less reliable.

This limitation motivated the development of a more advanced method.

---

## Bio-inspired skeletonization and phenotyping for soybean RSA

<div style="overflow: auto;">
    <div style="float: right; width: 38%; max-width: 420px; margin: 3.6rem 0.75rem 0.5rem 1rem;">
        {% include figure.liquid loading="eager" path="assets/gif/path_grow.gif" title="bio-inspired tracking" class="img-fluid rounded z-depth-1" %}
        <div class="caption" style="margin-top: 0.5rem; text-align: center; font-size: 0.95rem;">
            Bio-inspired path growing and tracking for complex soybean root systems.
        </div>
    </div>

    <p>
        To address the complexity of soybean root systems, we developed a <strong>bio-inspired tracking method</strong> for 3D skeletonization and phenotyping.
    </p>

    <p>
        Instead of first attempting to fully segment every individual root and then skeletonize them independently, this method directly traces biologically plausible root paths. As a result, the extracted outputs are inherently <strong>segmented skeletons / paths</strong>, which are more suitable for downstream trait analysis.
    </p>

    <p>
        The main workflow is:
    </p>

    <ol>
        <li><strong>Manually identify the two endpoints of the primary root</strong></li>
        <li><strong>Segment the primary root</strong></li>
        <li><strong>Locate primary–lateral joint regions as tracking initiation points</strong></li>
        <li><strong>Perform multiple tracking from these starting points</strong></li>
        <li><strong>Check overlap among candidate paths</strong></li>
        <li><strong>Use combination sort to avoid duplicates and select the optimal path set</strong></li>
        <li><strong>Obtain robust segmented skeletons for downstream trait computation</strong></li>
    </ol>

    <p>
        This approach has several advantages:
    </p>

    <ul>
        <li>it directly produces <strong>individual root skeletons / paths</strong></li>
        <li>it is more robust to <strong>overlapping structures</strong></li>
        <li>it handles <strong>complex root architectures</strong> better than the earlier segmentation-plus-clustering pipeline</li>
        <li>it provides a stronger basis for downstream <strong>trait extraction</strong>, including root length, branching angle, and volume</li>
    </ul>
</div>

<div style="clear: both;"></div>

---

## Representative soybean results

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/gif/root_system_demo_origin.gif" title="soybean_input" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Input soybean root system.
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/gif/root_system_demo.gif" title="soybean_segmentation" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Segmented individual root paths.
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/gif/skeleton_rotation.gif" title="soybean_skeleton" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Bio-inspired skeletonization result.
        </div>
    </div>
</div>

---

## Why segmentation and skeletonization matter

The purpose of segmentation and skeletonization in this project is not only structural visualization. More importantly, they are required for **downstream trait analysis**.

Once individual roots or root paths are identified, we can extract biologically meaningful traits such as:

- primary root length
- lateral root length
- lateral root number
- lateral root angle
- lateral root surface area
- lateral root volume

These traits are important for **phenotyping** and provide quantitative descriptors that can support **genotyping-related studies**.

In this sense, segmentation and skeletonization are enabling steps that connect 3D root geometry to downstream biological analysis.

---

## Key contributions

This project contributes to 3D RSA analysis in three ways:

- **Primary-root-guided lateral root separation**  
  We showed that segmenting the primary root first can simplify the decomposition of relatively simple root systems and make lateral root analysis more tractable.

- **From isolated roots to analyzable skeletons**  
  By combining segmentation and skeletonization, we transformed raw 3D point clouds into structures that support trait extraction.

- **Bio-inspired tracking for complex soybean RSA**  
  We introduced a more advanced path-based framework that is inherently robust to overlap and complexity, and better suited for downstream phenotyping.

---

## Cite our works
If you find this work helpful for your research, we kindly request that you cite the following papers:

```
@inproceedings{zhou20243d,
  title={3D segmentation within the root system architecture using Point Transformer},
  author={Zhou, Xuehai and Bai, Leshang and Xu, Rui and Kang, Rui and Torkamaneh, Davoud and Sun, Shangpeng},
  booktitle={2024 ASABE Annual International Meeting},
  pages={1},
  year={2024},
  organization={American Society of Agricultural and Biological Engineers}
}

@article{zhou20253d,
  title = {3D skeletonization and phenotyping for soybean root system architecture using a bio-inspired algorithm},
  journal = {Computers and Electronics in Agriculture},
  volume = {239},
  pages = {110890},
  year = {2025},
  issn = {0168-1699},
  type = {journal},
  publisher = {Elsevier},
  author = {Zhou, Xuehai and Yang, Tianzi and Xu, Rui and Bucksch, Alexander and Dutilleul, Pierre and Torkamaneh, Davoud and Sun, Shangpeng}
}
```
