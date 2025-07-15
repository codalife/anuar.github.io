---
title: Building Computer Vision System. Lessons in Technical Leadership.
description: This is a post on My Blog about what I've learned building an Machine Learning project from scratch.
date: 2025-07-15
tags:
  - machine-learning
  - techinal-leadership
  - software-engineering
layout: layouts/post.njk
---

**Bottom Line Up Front:** Built an AI-powered shooting target analysis system from scratch with zero computer vision background. The technical challenge taught me more about engineering leadership than any web project I'd shipped at scale companies.

## The Challenge: Operating in Uncharted Territory

As a web engineer with 7 years at Uber, Headspace, and Aurora, I could have easily dismissed computer vision as "not my domain." Instead, I saw an interesting problem: automatically detecting new bullet holes on shooting targets in real-time. 

The constraints were real—this needed to work with actual hardware in field conditions, not just clean datasets in Jupyter notebooks. I had to build everything: the data collection pipeline, annotation workflow, model training, and edge deployment on a Raspberry Pi 5.

## Initial Approach: The Elegant Theory Trap

My first instinct was to frame this as a "before/after comparison" problem. Siamese neural networks seemed perfect—feed the model two images and let it learn to spot differences. The approach felt mathematically elegant.

But after weeks of planning and initial development, I realized I was solving the wrong problem. Real shooting scenarios don't give you perfect "before" shots. Lighting changes, paper moves, and you need to work with whatever image you can capture.

**Leadership Lesson #1:** Resist the urge to force-fit elegant solutions. Match your technical approach to real-world constraints, not theoretical ideals.

## The Pivot: Strategic Resource Allocation Over Pure ML

Here's where engineering leadership intersected with technical choices. I faced a classic fork-in-the-road decision:

**Path 1:** Collect significantly more training data and make the Siamese CNN approach work  
**Path 2:** Pivot to a hybrid solution that leveraged existing data more efficiently

Both were technically viable. But Path 1 required weeks of additional data collection with no guarantee of success—Siamese networks are notoriously data-hungry. Path 2 let me combine techniques intelligently and ship something that worked.

I chose the hybrid approach:

1. **YOLO for target localization** - Detect target positions to remove background noise and focus on the relevant area
2. **LightGlue for local feature matching** - Align before/after images by finding corresponding keypoints, handling camera movement and paper shifts
3. **Image comparison** - Detect changes within the aligned, cropped target area

**Leadership Lesson #2:** When facing multiple viable technical paths, evaluate them on resource efficiency and risk, not just theoretical elegance. Sometimes the "less pure" ML solution is the better engineering solution.

**Leadership Lesson #3:** Sunk cost bias kills projects. When performance metrics show you're on the wrong path, pivot fast and learn faster.

## The 80/20 Reality Check

Here's what actually consumed my time over those couple of months:

- **80% Data Engineering:** Setting up hardware (Raspberry Pi + 64MP Arducam), building annotation pipelines with CVAT, handling coordinate system bugs, image resizing, format conversions
- **20% Model Architecture:** The actual YOLO training and tuning

This mirrors every complex system I've built in web development—the infrastructure and data architecture matter far more than the "sexy" algorithmic work.

I spent days just getting the annotation coordinate system right. Original images were 2MB+ and crashed the training pipeline due to memory constraints. Had to resize everything to 640x640 and debug why my y-axis was inverted.

**Leadership Lesson #4:** Production systems are 80% plumbing, 20% algorithms. Plan accordingly.

## Modern Learning Velocity

What struck me most was how different learning looks in 2025. I used LLMs to sanity-check architectural decisions, consumed Andrew Ng's courses for fundamentals, and leveraged papers/podcasts for cutting-edge techniques. The feedback loop between "I have a question" and "I understand the solution space" has compressed from weeks to hours.

When I hit the coordinate system bug, I could iterate through solutions in real-time rather than waiting for Stack Overflow responses or diving through documentation.

**Leadership Lesson #5:** The tools for continuous learning have never been better. Use them as force multipliers, not crutches.

## Systems Thinking: Orchestrating Components

The final solution wasn't a single model—it was an orchestrated pipeline:

- **YOLO for target detection and background removal** - Precisely crop to the target area, eliminating environmental noise
- **LightGlue for local feature matching** - Find corresponding keypoints between images to achieve robust alignment despite camera movement and paper positioning variations
- **Change detection** - Compare aligned target areas to identify new holes

This reinforced something I've seen across domains: production systems are about intelligent component orchestration, not single-technique optimization. The breakthrough came from combining multiple deep learning approaches (YOLO + LightGlue) with traditional change detection rather than forcing one technique.

**Leadership Lesson #6:** Success comes from systems thinking, not component optimization.

## The "It WORKS!!!!" Moment

April 25th, 11 AM: After days of debugging annotation formats and coordinate systems, the model finally detected targets correctly. That moment of breakthrough—seeing the bounding boxes appear around bullet holes in test images—reminded me why I love building things that work in the real world.

But more importantly, it validated the broader approach: when facing unfamiliar technical domains, the same engineering principles apply. Break down the problem, validate assumptions early, iterate based on data, and don't be afraid to pivot when the evidence points in a new direction.

## Broader Implications

This project reinforced three patterns I've seen across every successful technical initiative:

1. **Front-load research before committing to implementation.** The weeks I spent understanding the problem space saved months of building the wrong solution.

2. **Data quality often trumps algorithmic sophistication.** Clean, well-annotated datasets with proper coordinate systems matter more than hyperparameter tuning.

3. **Real-world constraints drive architecture decisions more than theoretical elegance.** Hardware limitations, memory constraints, and field conditions shaped every major technical choice.

These lessons apply whether you're building computer vision systems, distributed web services, or autonomous vehicle software. The domain changes, but the engineering leadership principles remain constant.

## Looking Forward

This project scratched a personal itch while teaching me fundamentals that apply to any technical domain. It's a reminder that the best way to learn isn't always through courses or tutorials—sometimes you need to build something real, with real constraints, and real stakes.

The spotter app now accurately detects new bullet holes in real-time. But the bigger win was proving to myself that with the right approach to problem-solving and continuous learning, any technical domain is learnable.

---

*Built over a couple of months while studying ML fundamentals. Sometimes the best classroom is your own curiosity plus a problem worth solving.*
