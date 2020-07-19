---
math: true
permalink: automata-nebula
title: Abstract Astronomy - Novel Classification of CA
layout: page
---

![Large Megallanic Cloud](/blog/images/automata_nebula/expository/large_megallanic_cloud.jpg)

In my [last post](/blog/turing-basins){:target=”blank”}, I shared my journey through understanding the link between entropy, thermodynamics, evolution, computation, and mathematics. At the end, I shared some preliminary research on using entropy/complexity to classify Cellular Automata objectively. At that time, I only had a handful of samples which, albeit showing promise, fell short of demonstrating concrete results.

I am incredibly excited to share that I have now run my simulations on _every single possible_ Life-Like Cellular Automaton rule (a total of 262,144 rules), and it shows some potential in classifying every rule based on its emergent behavior. Not only that, but this method establishes what appears to be a strong metric for finding "islands" of rules that have similar behavior.

## Recap

My inspiration for this approach came from a few core concepts. First, that entropy and complexity looked like valid metrics to measure the emergent behavior of a system and its potential for self-organized criticality. Second, that Cellular Automata are simple systems and are easy to study due to their oftentimes literal black-and-white interpretation (cells are either alive or dead). In a addition to the simplicity of the systems involved, the grid of a Cellular Automaton can be interpreted as an image. Image compression is (unsurprisingly) adept at finding something close to the smallest possible representation of an image, and PNG compression does it without loss of information. Rationalized by the knowledge that image compression then asymptotically (up to \\(\mathcal{O}(1)\\)) approximates Kolomogorov Complexity, and that entropy is the expected value of Kolomogorov Complexity in this context, we have a viable pipeline for estimating the complexity of each state of the CA universes we encounter.

While powerful, measuring the complexity of each generation of a CA is not enough to categorize its behavior. In order to do this, I started from random initial states of the universe with each cell having an equal probability of starting in any of the possible states. I then ran simulations for hundreds of generations with multiple random initial conditions and found the average complexity at each step. Other researchers looking into the general behavior of CA have taken this approach, so I felt that it was a valid (and computationally affordable) move to take.

Lastly, I chose to study only Life-Like CA. These are the totalistic CA rules that only depend on the Von-Neumann neighborhood of each cell. This made the search space something that I could simulate in reasonable time, given that it only had around a quarter million possible rules (even though it still took two weeks).

The result of these simulations were 262,144 records of the average complexity in bytes of the board of a Life-Like CA. Each record had 256 samples, each rule had 10 runs, and each rule was run with a board size of 100x100 cells.

## Results - An Automata Nebula

Obviously, no one has the time to go through the graphs of over a quarter million samples, so I needed to find a way to classify the results. Recently I have been infatuated with the UMAP algorithm. It has the ability to compress data with thousands of dimensions into a lower dimensional space (in this case 2D or 3D), while still preserving structures/features in the data. It is a remarkable feat of algebraic topology that deserves more awareness of in the scientific community.

When viewed as vectors, each averaged sample of complexity history for a given rule is a 256 dimensional vector. Given that I don't have 256-dimensional vision, or a screen to render a quarter million points in 256 dimensions, I needed UMAP to project this structure I had created onto a lower dimensional space in order to view it.

The results, while not what I expected, were beautiful:

![Large Megallanic Cloud](/blog/images/automata_nebula/plots/selected_run/UMAP_CA_Full.png)

| 236670|5640|5896|6152|137480|
|-------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
|B123456/S123678|B3/S013|B38/S013|B3/S23|B38/S238|
| ![B123456/S123678](/blog/images/automata_nebula/animations/similar/gol_like/236670-B123456-S123678.gif) | ![B3/S013](/blog/images/automata_nebula/animations/similar/gol_like/5640-B3-S013.gif) | ![B38/S013](/blog/images/automata_nebula/animations/similar/gol_like/5896-B38-S013.gif) | ![B3/S23](/blog/images/automata_nebula/animations/similar/gol_like/6152-B3-S23.gif) | ![B38/S238](/blog/images/automata_nebula/animations/similar/gol_like/137480-B38-S238.gif) |

## Filtering The Rules

While there is beauty in showing the structure of every possible rule, it is important to note that some classes of rules will not produce emergent behavior capable of universality. This is covered in more detail [in other research](https://www.ics.uci.edu/~eppstein/ca/wolfram.html){:target="blank"}. Essentially, rules that contain `B1` in their makeup will expand to infinity in all directions. Likewise, rules that contain `S01234` or `B23/S0` will do the same. Inversely, if a rule does not include `B2` or `B3`, any pattern will remain within its initial bounding box. This would make the formation of gliders impossible, which would prevent the formation of gliders. While it still may be possible for computation to occur in one-dimensional CA that exist on the boundaries of structures in both the first and second cases, I would like to examine only the types of CA that support universal structures that use gliders for the transport of information. This is how universality was proven in the Game of Life, after all.

In order to filter the rules so that we are only examining systems that are capable of both expansion and contraction, we just need to look at rules that don't contain `B1` and contain either `B2` or `B3`. This reduces our search space by at least a factor of four.
