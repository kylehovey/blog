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

## Results

Obviously, no one has the time to go through the graphs of over a quarter million samples, so I needed to find a way to classify the results. Recently I have been infatuated with the UMAP algorithm. It has the ability to compress data with thousands of dimensions into a lower dimensional space (in this case 2D or 3D), while still preserving structures/features in the data. It is a remarkable feat of algebraic topology that deserves more awareness of in the scientific community.

When viewed as vectors, each averaged sample of complexity history for a given rule is a 256 dimensional vector. Given that I don't have 256-dimensional vision, or a screen to render a quarter million points in 256 dimensions, I needed UMAP to project this structure I had created onto a lower dimensional space in order to view it.

The results, while not what I expected, were beautiful:

![Large Megallanic Cloud](/blog/images/automata_nebula/plots/selected_run/UMAP_CA_Full.png)
