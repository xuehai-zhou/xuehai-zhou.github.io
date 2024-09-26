---
layout: page
is_project: true
title: Occlusion-resilient fruit counting
description: Accurate yield estimation is critical for supply chain management. Traditional fruit detection methods, especially those using stitched images, often fail to address leaf occlusion problems. This project utilizes lightweight object detection neural networks and real-time tracking algorithms to count fruits in video footage, enabling multi-angle observation that significantly reduces occlusion issues. Furthermore, this method can be extended to incorporate features such as disease detection. Deploying this technology with an RGB camera on unmanned aerial vehicles (UAVs) or ground vehicles (UGVs) significantly enhances the efficiency and effectiveness of agricultural surveillance and management.
img: assets/gif/strawberry_demo.gif
importance: 3
category: work
---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/gif/strawberry_demo-uncrop.gif" title="strawberry_counting" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Counting ripe and unripe strawberries.
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/gif/tomato_demo.gif" title="tomato_counting" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Counting ripe tomatos.
        </div>
    </div>
</div>


To enable real-time counting functionality in video streams, we initially train object detection neural networks, such as [YOLO](https://github.com/ultralytics/ultralytics), enhanced with modules like [Swin-Transformer](https://github.com/microsoft/Swin-Transformer) and [CBAM](https://arxiv.org/abs/1807.06521). Subsequently, [tracking techniques](https://paperswithcode.com/task/multi-object-tracking) are employed to eliminate duplicates of identical targets across consecutive frames. We have developed a customized module named MultiMap that assigns Track IDs to counting numbers, which are then displayed adjacent to the tracked objects in the video.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/fruit_count/network.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Fruit detection neural network architecture.
        </div>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/fruit_count/integration.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
        <div class="caption">
            Integration of the MultiMap module for enhancing tracking algorithms into counting systems.
        </div>
    </div>
</div>

To accurately count objects in real-time video streams, we first detect bounding boxes and associate identical objects across frames using tracking algorithms like [DeepSORT](https://github.com/nwojke/deep_sort), [ByteTrack](https://github.com/ifzhang/ByteTrack), and [StrongSORT](https://github.com/dyhBUPT/StrongSORT). However, directly applying these tracking methods to counting tasks can lead to inaccuracies due to issues like ID jumps—where Track IDs are assigned before an object's presence is confirmed, causing discontinuities and overcounting. To solve this, we introduce the MultiMap scheme. MultiMap focuses on confirmed tracks, mapping each one to a unique counting number only after verification. It maintains a reference that records all confirmed tracks and assigns counting numbers based on object classes. This approach ensures accurate counting by effectively filtering out false positives and addressing ID jumps. By concentrating on confirmed tracks and providing a clear mapping to counting numbers, MultiMap offers a simple yet robust solution for counting tasks in large-scale applications.

<div class="row mt-4 d-flex align-items-center">
    <!-- Left Column: Algorithm Image (1/3 of the row) -->
    <div class="col-md-5">
        {% include figure.liquid path="assets/img/fruit_count/alg.jpg" title="Algorithm 1: MultiMap Procedure" class="img-fluid rounded z-depth-1" %}
    </div>
    <!-- Right Column: Description Text (2/3 of the row) -->
    <div class="col-md-7">
        <div>
            <h5>Parameters involved</h5>
            <p>
                <i>S<sub>C</sub></i> : The set of confirmed tracks, representing objects that have been consistently detected and tracked.
            </p>
            <p>
                <i>L<sub>1</sub></i> : A mapping from each object class to its assigned counting numbers, effectively keeping track of counts per class.
            </p>
            <p>
                <i>t</i> : Each individual track within <i>S<sub>C</sub></i>, containing attributes like <i>t.id</i> (the unique Track ID) and <i>t.class</i> (the object class).
            </p>
            <p>
                <i>L<sub>2</sub></i> : A mapping specific to an object class that assigns counting numbers to Track IDs within that class.
            </p>
            <p>
                <i>prev</i> : A variable holding the current highest counting number for a particular class.
            </p>
        </div>
    </div>
</div>

## Cite our works
If you find this work helpful for your research, we kindly request that you cite the following papers:

{% raw %}

```
@article{zhou2024advancing,
  title={Advancing tracking-by-detection with MultiMap: Towards occlusion-resilient online multiclass strawberry counting},
  author={Zhou, Xuehai and Zhang, Yuyang and Jiang, Xintong and Riaz, Kashif and Rosenbaum, Phil and Lefsrud, Mark and Sun, Shangpeng},
  journal={Expert Systems with Applications},
  volume={255},
  pages={124587},
  year={2024},
  publisher={Elsevier}
}

@inproceedings{zhou2023dynamic,
  title={A Dynamic Object Counting Method for Strawberry Fruits using Vision Transformer Networks and Kalman Filter Tracking},
  author={Zhou, Xuehai and Zhang, Yuyang and Sun, Shangpeng Sun and Rosenbaum, Phil},
  booktitle={2023 ASABE Annual International Meeting},
  pages={1},
  year={2023},
  organization={American Society of Agricultural and Biological Engineers}
}

@article{kang2024toward,
  title={Toward Real Scenery: A Lightweight Tomato Growth Inspection Algorithm for Leaf Disease Detection and Fruit Counting},
  author={Kang, Rui and Huang, Jiaxin and Zhou, Xuehai and Ren, Ni and Sun, Shangpeng},
  journal={Plant Phenomics},
  volume={6},
  pages={0174},
  year={2024},
  publisher={AAAS}
}
```

{% endraw %}
