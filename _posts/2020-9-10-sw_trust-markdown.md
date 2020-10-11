---
layout: post
title: Trust Evaluation in OSNs
subtitle: Implementing SWTrust framework for online social networks based on [this paper]({% link https://ieeexplore.ieee.org/abstract/document/6120835 %})
tags: [Project]
comments: true
---

### Brief introduction

{: .box-note}
**I**n this project we implemented SWTrust framework to generate trusted graphs for trust evaluation in online social networks. The algorithm is based on graph theory and weak ties. The results are tested for Epinions dataset.


### SWTrust
SWTrust is a framework that addresses the issue of \"Can Alice trust Bob on a service?\" in
large online social networks (OSNs). Many models have been proposed for constructing and calculating
trust. However, two common shortcomings make them less practical, especially in large OSNs: the
information used to construct trust is usually too complicated to get or maintain, that is, it is
resource consuming; and usually subjective and changeable, which makes it vulnerable to vicious
nodes. With those problems in mind, SWTrust is focused on generating small trusted graphs for large OSNs, which
can be used to make previous trust evaluation algorithms more efficient and practical.
At first we preprocess a social network (PSN) by developing a simple and practical user-domain-based trusted
acquaintance chain discovery algorithm through using the small-world network characteristics of online
social networks and taking advantage of ‘‘weak ties’’. Then, a trust network (BTN) is built 
and a trusted graph (GTG) is generated with the adjustable width breadth-first search algorithms.

![BobAlice](/assets/img/sw_trust_bob_alice.jpg){: .mx-auto.d-block :}

