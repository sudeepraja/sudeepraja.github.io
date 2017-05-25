---
layout: post
title: Ambulance Response Time Optimization
published: true
project: true
---

This was a project I did in my junior year, with my friends [Palash Mittal](https://www.linkedin.com/in/palashmittal) and [Aakash Anuj](https://www.linkedin.com/in/aakash-anuj-4870b345). This was for the Xerox Research Innovation Challenge competition conducted as a part of XRCI Open 2015.

After brainstorming possible innovations in the domain of healthcare and transportation, we decided to work on the problem of Ambulance Facility Location. The goal was to reduce the response time of ambulances by positioning them at strategic locations in a city.

## Data Collection

We targeted the city of Bangalore as 1)XRCI is located here and 2)It is infamous for its traffic. After digging through a few websites, we found a survey listing the number of accidents per location for a few locations in Bangalore. We used this data for the next phase.

## Modelling - Facility Location on Roads

Facility Location is a problem commonly studied in Computational Geometry and Operations Research. There exits many variants of this problem. We used the $$p$$-Median version of this problem. 

You are given a directed graph with vertex set $$V$$ and edge set $$E$$. Each vertex has a demand value $$a_i$$. The weight of the edge between two vertices is given by $$w_{ij}$$, which represents the cost of travelling from $$i$$ to $$j.$$ The objective of the $$p$$-median problem is to find the set of vertices of size $$p$$, called facilities such that the demand weighted total cost between each vertex and its nearest facility is minimized.

We assume that each vertex in this graph is reachable from every other vertex. Let $$d_{ij}$$ be the cost of travelling from $$i$$ to $$j$$ on the minimum cost path. These $$d_{ij}$$s can be calculated by running the Floyd-Warshall Algorithm on the graph. 

Define the indicator variables $$X_{i}$$ and $$Y_{ij}$$ as:

$$
X_{i}= 
\begin{cases}
    1 & \text{if } i \text{ is a facility }\\
    0              & \text{otherwise}
\end{cases}
$$

$$
Y_{ij}= 
\begin{cases}
    1 & \text{if demand at node } i \text{ is assigned to facility }j\\
    0              & \text{otherwise}
\end{cases}
$$


This problem can be stated as a Binary Integer Program as follows:

$$
\begin{array}{ll@{}ll}
\text{minimize}  & \sum_{i}\sum_{j}a_i d_{ij} Y_{ij}\\
\text{subject to}& \sum_{j}Y_{ij} = 1  & \forall i \in V  & \text{Each vertex is assigned to exactly 1 facility}\\
				 & \sum_{i}X_{i} = p & & \text{There can be exactly } p \text{ facilities}\\
                 & Y_{ij} \le X_j & \forall i \in V,\forall j \in V & \text{Vertex } i \text{ can be assigned to } j \text{, only if }j \text{ is a facility}\\
                 & Y_{ij} \in \{0,1\} & \forall i \in V,\forall j \in V \\
                 & X_i \in \{0,1\} & \forall i \in V
\end{array}
$$

We created a graph on the road network of Bangalore with various locations of the city as vertices. The demand of a vertex is the number of accidents which occur in that area. The cost of travelling between two vertices is the time it takes to reach one vertex from the other, by following the shortest time route. All that is left to be done is to feed the variables into a solver and get the Xs and Ys. We used this software: [FLP Spreadsheet Solver](https://verolog.deis.unibo.it/flp-spreadsheet-solver).

The p-Median problem is NP-hard on general network. Although it can be solved exactly using a Branch and Bound or a Branch and Cut method, it may take a very long time to solve even small instances. This software uses a Tabu search heuristic and provides a good enough sub-optimal solution.  

We went on to win the Xerox Research Innovation Challenge that year. Here is the presentation we gave at XRCI 2015:
<p align="center"><iframe id="iframe_container" frameborder="0" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen="" width="550" height="400" src="https://prezi.com/embed/yh8vzwj0mmav/?bgcolor=ffffff&amp;lock_to_path=0&amp;autoplay=0&amp;autohide_ctrls=0&amp;landing_data=bHVZZmNaNDBIWnNjdEVENDRhZDFNZGNIUE43MHdLNWpsdFJLb2ZHanI5KzdQNnNzTUhxWVhBZlc4dXRUWjQrRXhnPT0&amp;landing_sign=we7s1YuLpdjkKWx0fe-NQplgR8ibWKoZoZOdvaiKfds"></iframe></p>
