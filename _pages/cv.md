---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Education
======
**KTH Royal Institute of Technology** — *MSc in Autonomous Systems and Intelligent Robots (Double Degree)*  
Sep 2025 – Jul 2026  

**Aalto University** — *MSc in Autonomous Systems and Intelligent Robots (Double Degree)*  
Sep 2024 – Jul 2026 · GPA 4.31/5  
Relevant coursework: Reinforcement Learning, Digital & Optimal Control, Robotic Manipulation, Probabilistic Machine Learning, Dynamic System Modeling & Analysis, Robotics, Supervised Machine Learning.

**Jiangnan University (Project 211)** — *BEng in Computer Science and Technology*  
Sep 2019 – Jul 2023 · GPA 3.54/4 · Rank 17/216 (Top 10%)  
Selected coursework: Advanced Mathematics (95), Probability & Statistics (99), Machine Learning (A), Algorithm Analysis & Design (97), Image Processing Techniques (A).

Honors & Awards
======
- Erasmus Scholarship, 2025  
- Master’s Scholarship, 50% tuition waiver, 2024  
- Excellent Bachelor Thesis, Jiangnan University, 2023  
- Mathematical Contest in Modeling (MCM), Finalist, 2022  
- Second-Class Scholarship, Jiangnan University, 2022  
- Lanqiao Cup, Third Prize (C/C++ Programming), 2021  
- Certified Software Engineer (Top 7.88%), 2020  

Internships
======
**Astribot (Stardust Intelligence)** — Robotics Learning Algorithm Engineer  
May 2025 – Present  
- Investigate RSSM-based world models (DreamerV2/V3, TD-MPC2) combined with imitation learning (ACT, Diffusion Policy) and VLA models (OpenPi, OpenVLA).  
- Research multi-task policies, proposing solutions for state confusion stemming from insufficient long-horizon memory; explore object-centric latent spaces for improved multi-object reasoning.  

**Zhejiang Lab** — Research Assistant  
2024  
- Researched 3D Gaussian Splatting for high-quality scene reconstruction with reflective surfaces such as automotive glass.  
- Collaborated with interdisciplinary teams to design scalable and performant reconstruction pipelines.  

Research Experience
======
**Object-Centric Learning Research** — First author NeurIPS 2025 poster  
Dec 2024 – May 2025 · Advisor: [Joni Pajarinen](https://rl.aalto.fi/)  
- Reviewed ~70 papers on object-centric learning across CV, RL, and robotics to identify integration gaps.  
- Reproduced key Slot Attention variants to validate baselines before proposing a new method.  
- Proposed **MetaSlot**, a Slot Attention variant that adapts to varying object counts using a VQ-prototype codebook, pruning redundant slots and enabling semantically meaningful initialization.  
- First author of [MetaSlot: Break Through the Fixed Number of Slots in Object-Centric Learning](https://arxiv.org/abs/2505.20772), accepted to **NeurIPS 2025**.  

**Gaze Estimation Research**  
Sep 2024 – May 2025 · Advisor: [Shiyong Lan](https://cs.scu.edu.cn/info/1359/16712.htm)  
- Developed **DMAGaze**, a gaze estimation framework that blends feature disentanglement with multi-scale attention for accurate direction prediction.  
- Second author of [DMAGaze: Gaze Estimation Based on Feature Disentanglement and Multi-Scale Attention](https://arxiv.org/abs/2504.11160), submitted to **Pattern Recognition Letters**.  
- Proposed **DCDNet** (Differential Capsule Disentanglement Network) with structural constraints and differential operations to suppress noise and retain gaze-relevant cues, submitted to **IEEE Transactions on Instrumentation & Measurement**.  
- Ran extensive benchmark evaluations, achieving state-of-the-art performance on public datasets.  

**Fast Style Transfer Based on AdaIN**  
2023 · Advisor: [Hui Li](https://hli1221.github.io/)  
- Proposed GuideAST, a fast arbitrary style transfer model that couples sketch and correction networks.  
- Enhanced global style transfer on super-resolution images through multi-layer AdaIN skip connections and Gaussian noise injection.  
- Incorporated guided filtering to preserve high-frequency details in the correction stage.  

Projects
======
**Quantitative Investment Strategies for Gold and Bitcoin** · 2022  
- Designed a hybrid Random Forest + BiLSTM model for purchase-pattern and price-trend prediction.  
- Built a daily trading ratio strategy grounded in portfolio-theory constraints and sensitivity analysis.  

**Mainboard Quality Inspection System (Faster R-CNN)** · 2021  
- Developed a production-line defect inspection system for a local enterprise.  
- Led dataset annotation and Faster R-CNN training/tuning to reach reliable detection accuracy.  

**Organizer, ACM Algorithm Design Club / Hengwei Cup Programming Competition** · 2020  
- Co-led daily training and competition reviews, emphasizing advanced algorithmic problem solving.  
- Authored contest problems on dynamic programming, topological sorting, and union-find to sharpen coding fluency.  

Skills
======
- **Languages:** Excellent English (IELTS 6.5); native Mandarin Chinese.  
- **Programming:** Python, C/C++, Matlab, LaTeX, SQL, Java, R, C#.  
- **Deep Learning & ML:** PyTorch, PyTorch Lightning, GPU training/inference workflows for CV and NLP.  
- **Robotics:** ROS, Linux, reinforcement learning, optimal control.  
- **Database:** OLTP/OLAP data modeling and schema design.  
- **Software Engineering:** Git-based collaboration, DevSecOps pipelines, agile practices.  
- **Other:** Unity development, OOP, mathematical modeling, design patterns.  

Publications
======
<ul>{% for post in site.publications reversed %}
  {% include archive-single-cv.html %}
{% endfor %}</ul>

Talks
======
<ul>{% for post in site.talks reversed %}
  {% include archive-single-talk-cv.html  %}
{% endfor %}</ul>

Teaching
======
<ul>{% for post in site.teaching reversed %}
  {% include archive-single-cv.html %}
{% endfor %}</ul>
