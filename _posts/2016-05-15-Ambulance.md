---
layout: post
title: Ambulance Response Time Optimization
published: true
project: true
---

This was a project I did in my junior year, with my friends [Palash Mittal](https://www.linkedin.com/in/palashmittal) and [Aakash Anuj](https://www.linkedin.com/in/aakash-anuj-4870b345). This was for the Xerox Research Innovation Challenege competetion conducted as a part of XRCI Open 2015.

After barinstorming possible innovations in the domain of healthcare and transportation, we decided to work on the problem of Ambulance Facility Location. The goal was to reduce the response time of ambulances by positioning them at stretegic locations in a city.

## Data Collection

We targeted the city of Bangalore as 1)XRCI is located here and 2)It is infamous for its traffic. After digging through a few websites, we found a survey listing the number of accidents per location for a few locations in Bangalore. We used this data for the next phase.

## Modelling - Facility Location on Roads

Facility Location is a problem commonly studied in Computational Geometry and Operations Research. There exits many varients of this problem. We used the P-median version of this problem. What it basically says is:

You are given a graph of $$I$$ nodes, indexed by $$i$$. Each node has a demand value $a_i$. $$d_{ij}$$ is the distance between node $i$ and $j$.

Here is the presentation we gave at XRCI 2015:

<p align="center"><iframe id="iframe_container" frameborder="0" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen="" width="550" height="400" src="https://prezi.com/embed/yh8vzwj0mmav/?bgcolor=ffffff&amp;lock_to_path=0&amp;autoplay=0&amp;autohide_ctrls=0&amp;landing_data=bHVZZmNaNDBIWnNjdEVENDRhZDFNZGNIUE43MHdLNWpsdFJLb2ZHanI5KzdQNnNzTUhxWVhBZlc4dXRUWjQrRXhnPT0&amp;landing_sign=we7s1YuLpdjkKWx0fe-NQplgR8ibWKoZoZOdvaiKfds"></iframe></p>
