---
layout: post
title: "LiCS: Navigation using Learned-imitation on Cluttered Space" 
categories: [paper]
---

<style>
/* .post h1, h2, .center {
    text-align: center;
} */
 .full-width-viewport-image {
    width: 100%;
    height: auto;
    display: block;
    margin-left: auto;
    margin-right: auto;
}

@media (min-width: 900px) {
    .full-width-viewport-image {
        position: relative;
        left: 50%;
        right: 50%;
        margin-left: -40vw;
        margin-right: -40vw;
        width: 80vw;
        max-width: 80vw;
        height: auto;
    }
}

</style>

<div class="center"><a href="https://scholar.google.com/citations?user=eyQAHtwAAAAJ&hl=en">Joshua Julian Damanik <i class="fa fa-graduation-cap" aria-hidden="true"></i></a>,
 Jae-Won Jung, Chala Adane Deresa, <a href="https://scholar.google.com/citations?user=v5hGAWMAAAAJ&hl=en">Han-Lim Choi <i class="fa fa-graduation-cap" aria-hidden="true"></i></a></div>
---
[[Preprint](https://arxiv.org/abs/2406.14947)] [[Code](https://github.com/damanikjosh/the-barn-challenge)]


## Video

<div class="center">
<iframe class="center" width="100%" height="405" src="https://www.youtube.com/embed/b2xXbj4M8x8?si=Z-2YlGnDh-D70DmZ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

## Abstract

In this letter, we propose a robust and fast navigation system in a narrow indoor environment for UGV (Unmanned Ground Vehicle) using 2D LiDAR and odometry. We used behavior cloning with Transformer neural network to learn the optimization-based baseline algorithm. We inject Gaussian noise during expert demonstration to increase the robustness of learned policy. We evaluate the performance of LiCS using both simulation and hardware experiments. It outperforms all other baselines in terms of navigation performance and can maintain its robust performance even on highly cluttered environments. During the hardware experiments, LiCS can maintain safe navigation at maximum speed of 1.5 m/s.

## Highlights

<!-- ![LiCS architecture](/assets/image/posts/lics_diagram.jpg) -->
<img src="/assets/image/posts/lics_diagram.jpg" alt="LiCS architecture" class="full-width-viewport-image">
<br />
**Figure 1**: Training pipeline diagram of the Learned-imitation on Cluttered Space model.
<br /><br />

<img src="/assets/image/posts/lics_result.png" alt="LiCS result" class="full-width-viewport-image">
<br />
**Figure 2**: Training pipeline diagram of the Learned-imitation on Cluttered Space model.