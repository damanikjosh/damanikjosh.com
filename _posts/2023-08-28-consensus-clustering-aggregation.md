---
layout: post
title: "Consensus-based clustering and data aggregation in decentralized network of multi-agent systems"
tags: [paper]
---

<style>
/* .post h1, h2, .center {
    text-align: center;
} */
 /* .full-width-viewport-image {
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
} */

</style>
---
<div class="center"><a href="https://scholar.google.com/citations?user=eyQAHtwAAAAJ&hl=en">Joshua Julian Damanik</a>,
 <a href="https://scholar.google.com/citations?user=FDtnVAMAAAAJ&hl=en">Ming Chong Lim</a>, Hyeon-Mun Jeong, Ho-Yeon Kim, <a href="https://scholar.google.com/citations?user=v5hGAWMAAAAJ&hl=en">Han-Lim Choi</a></div>
---

[[Journal Paper](https://peerj.com/articles/cs-1445/?td=tw)] [[Code](https://github.com/damanikjosh/peerj)]



## Abstract

Multi-agent systems are promising for applications in various fields. However, they require optimization algorithms that can handle large number of agents and heterogeneously connected networks in clustered environments. Planning algorithms performed in the decentralized communication model and clustered environment require precise knowledge about cluster information by compensating noise from other clusters. This article proposes a decentralized data aggregation algorithm using consensus method to perform COUNT and SUM aggregation in a clustered environment. The proposed algorithm introduces a trust value to perform accurate aggregation on cluster level. The correction parameter is used to adjust the accuracy of the solution and the computation time. The proposed algorithm is evaluated in simulations with large and sparse networks and small bandwidth. The results show that the proposed algorithm can achieve convergence on the aggregated data with reasonable accuracy and convergence time. In the future, the proposed tools will be useful for developing a robust decentralized task assignment algorithm in a heterogeneous multi-agent multi-task environment.

## Highlights

This paper presents a decentralized data aggregation algorithm using consensus methods tailored for multi-agent systems in clustered environments. It introduces a trust value to accurately aggregate COUNT and SUM metrics across clusters with minimal communication. The algorithm is designed to optimize performance in large, sparsely connected networks with low bandwidth, allowing agents to reach consensus efficiently. The findings indicate promising results for decentralized task assignments in complex, heterogeneous networks.

![network of distributed network](/assets/images/peerj_network.png)
<br />
**Figure 1**: A neetwork consisting of 100 heterogeneous agents with 4 categories (a). The corresponding
adjacency matrix is shown in (b). 
<br /><br />


## Key Contributions

1. Decentralized Aggregation: Novel algorithm for COUNT/SUM data aggregation using trust-based consensus, suitable for clustered and heterogeneous networks.
2. Trust Value Integration: Trust values improve aggregation accuracy and limit the impact of noise and inter-cluster communication.
3. Efficiency in Sparse Networks: Algorithm achieves accurate aggregation with limited bandwidth and quick convergence, even in large multi-agent networks.
4. Future Applications: Potential for developing robust decentralized task assignment and coordination algorithms in complex multi-agent systems.

![data counting problem](/assets/images/peerj_counting.png)
<br />
**Figure 2**: A problem of identifying objects in a decentralized network of multi-agent systems. Data
aggregation enables agent 1 to recognize objects beyond its sensor range.
<br /><br />


![data counting result](/assets/images/peerj_result.png)
<br />
**Figure 3**: Data to be aggregated (a) for each nodes equals to the ID of each agents (yi; i) and the
data aggregation (b) converges to the sum of data to be aggregated of all agents in the same cluster,
<br /><br />
