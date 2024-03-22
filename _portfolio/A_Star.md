---
title: "A* pathfinding algorithm"
excerpt: "A C++ implementation of the A* algorithm"
header:
#  image: /assets/images/bio-photo.jpg
  teaser: /assets/images/AStar.gif
permalink: /AStar/
sidebar:
  - title: "Role"
    text: "Game Programmer"
  - title: "Technologies used:"
    text: "- C++"
  - title: "Team size: 2"
    text: " "
  - title: "Time frame: One week"
    text: " "
gallery:
  - url: /assets/images/AStar.gif
    image_path: /assets/images/AStar.gif
    alt: "A*"
    title: "A*"
simple:
  - url: /assets/images/SimpleAStar.png
    image_path: /assets/images/SimpleAStar.png
    alt: "Simple A*"
    title: "A* with array of open nodes"
node:
  - url: /assets/images/NodeArrayAStar.png
    image_path: /assets/images/NodeArrayAStar.png
    alt: "Node Array A*"
    title: "A* with array of node records"
priority:
  - url: /assets/images/PriorityQueueAStar.png
    image_path: /assets/images/PriorityQueueAStar.png
    alt: "Priority Queue A*"
    title: "A* with priority queue of open nodes"
comparison:
  - url: /assets/images/AStarComparisons.png
    image_path: /assets/images/AStarComparisons.png
    alt: "Simple A*"
    title: "Comparing the three versions"
---
{% include gallery %}
## Overview

This project is a C++ version of the A* pathfinding algorithm described by Ian Millington in his book <strong>AI for Games Third Edition</strong>.<br>
It was the first more structured project done in C++ and it was the one that got me confident in the language; the majority of the code regarding the pathfinding was written in pair-programming.<br>
[A* on GitHub](https://github.com/IlDirettore95/Pathfinding)

## The algorithm

Pathfinding algorithms solve a common problem of every video game: <strong>which is the best path (if there is one) between two points</strong>, where best means the less expensive one.<br>
However they do not take into consideration nor the why or the how.<br>
To represent the “level” we use a graph which is made of nodes and connections.
More precisely we represent it with a <strong>directed non-negative weighted graph</strong>:
- <strong>directed</strong>: each connection is one-way only (ex. point A to B but NOT point B to A);
- <strong>non-negative weighted</strong>: the cost of a connection can represent how much time the agent spends using that connection or how far the two nodes connected are or also how difficult that path is.

<strong>Node representation:</strong>
```
  struct Node
  {
    struct Point
    {
      float X;
      float Y;

      Point(float x, float y);
      Point(int x, int y);
    };

    int ID;
    Point Position;
    Node(int id, Point position);
  };
```

<strong>Connection representation:</strong>
```
  struct Connection
  {
    int From;
    int To;
    float Cost;

    Connection(int from, int to, float cost);
  };
```

### A*
This algorithm, given a graph and two points (a start node and an end node), returns a list of connections to follow to go from the start node to the end node.<br> 
This path, theoretically, should be the less expensive one but, since this implementation is thought to be used in game development, we stop the algorithm once we reach the end node thus settling for one with a low cost rather than the least expensive one.<br>
Broadly speaking, the algorithm checks all the connections of the current node (starting from the start one) and for each “To” node (the node that connection points to) stores:
- the cost of the path until that moment (cost-so-far);
- the node that connection started from;
- the “estimated-total-cost” which is the sum of the cost-so-far and an estimate of the cost to go from that node to the end node.

#### Version #1: Array of open nodes
{% include gallery id="simple" %}


#### Version #2: Array of node records
{% include gallery id="node" %}


#### Version #3: Heap Node Array of node records
{% include gallery id="priority" %}



### Comparison
{% include gallery id="comparison" %}
