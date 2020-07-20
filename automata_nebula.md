---
math: true
permalink: automata-nebula
title: Abstract Astronomy - Novel Classification of CA
layout: page
---

![Large Megallanic Cloud](/blog/images/automata_nebula/expository/large_megallanic_cloud.jpg)

In my [last post](/blog/turing-basins){:target=”blank”}, I shared my journey through understanding the link between entropy, thermodynamics, evolution, computation, and mathematics. At the end, I shared some preliminary research on using entropy/complexity to classify the behavior of Cellular Automata (CA) and perhaps pave a road to finding more universal CA (those capable of computation). At that time, I only had a handful of samples which, albeit showing promise, fell short of demonstrating concrete results.

I am incredibly excited to share that I have now run my simulations on _every single possible_ Life-Like Cellular Automaton rule (a total of 262,144 rules), and it shows some great potential in classifying every rule based on its emergent behavior. Not only that, but this method establishes what appears to be a strong metric for finding "islands" of rules that have similar behavior.

This is exciting news, because past classifications of even elementary CA such as the semi-totalistic Von-Neumann neighborhood variety (called the Life-Like CA) have either required generalizations that are computationally intractable to ascertain, or required a great deal of manual filtering and edge-case handling in order to separate sets of rules into classes.

## Abstract

In his paper "University and Complexity in Cellular Automata", Stephen Wolfram proposed a four-level classification scheme for one dimensional cellular automata. He later extended these definitions to include two-dimensional cellular automata like the Life-Like CA we are looking at here. The classifications are:

1. _Evolution leads to a homogeneous state._
2. _Evolution leads to a set of separated simple stable or periodic structures._
3. _Evolution leads to a chaotic pattern._
4. _Evolution leads to complex localized structures, sometimes long-lived._

But, as mentioned in [this post](https://www.ics.uci.edu/~eppstein/ca/wolfram.html){:target="blank"} regarding some caveats about these classifications, gliders have been found in all four of these classes. Even worse, [it has been shown](https://wpmedia.wolfram.com/uploads/sites/13/2018/02/02-2-2.pdf){:target="blank"} that, given a rule, finding which class a CA belongs to is an undecidable problem (for one-dimensional CA at least, but I would imagine the argument abstracts well to any Cartesian dimension).

My goal here was to focus on dynamic classification of the emergent properties of any given CA given its rules. By not subscribing to manually generated labels on classification, we can instead focus on developing a metric of similarity. In this sense, each rule becomes its own "class" and you can find rules that are sufficiently close in behavior to be considered the same class. Geometrically, this would be an analysis of CA by way of clustering.

The difficult part, of course, is developing a representation of a given rule that would allow for clustering. I settled on producing a curve of the Kolmogorov complexity across the generations of the automaton's universe. My inspiration for this approach came from a few core concepts. First, that entropy and complexity looked like valid metrics to measure the emergent behavior of a system and its potential for self-organized criticality. In a addition to the convenient dualistic simplicity of studying life-or-death CA, the grid of an automaton can be interpreted as an image. Image compression is (unsurprisingly) adept at finding something close to the smallest possible representation of an image, and PNG compression does it without loss of information. Image compression asymptotically (up to \\(\mathcal{O}(1)\\)) approximates Kolmogorov Complexity. Therefore, we have a viable pipeline for estimating the complexity of each state of the CA universes we encounter. If we wish to relate all of this back to entropy, we can do so. Entropy is the expected value of Kolmogorov Complexity in this context, so this data will be useful for that as well.

In order to get an amortized generalization of each rule, I started from random initial states of the universe with each cell having an equal probability of starting in any of the possible states. I then ran simulations for hundreds of generations with multiple random initial conditions and found the average complexity at each step. Other researchers looking into the general behavior of CA have taken this approach of random initial state and it seems to be a valid way to capture their behavior.

Lastly, I chose to study only Life-Like CA. These are the totalistic CA rules that only depend on the Von-Neumann neighborhood of each cell. This made the search space something that I could simulate in reasonable time, given that it only had around a quarter million possible rules (even though it still took two weeks to generate all of the data).

The result of these simulations were 262,144 records of the average complexity in bytes of the board of all possible Life-Like CA. Each record had 256 samples, each record was averaged from 10 runs, and each rule was run with a board size of 100x100 cells.

## Using UMAP, a Digital Telescope

Obviously, no one has the time to go through the graphs of over a quarter million samples, so I needed to find a way to classify the results. Recently I have been infatuated with the UMAP algorithm. It has the ability to compress data with thousands of dimensions into a lower dimensional space (in this case 2D or 3D) while still preserving structures/features in the data. It is a remarkable feat of algebraic topology that deserves more awareness of in the scientific community.

When first learning about dimensionality reduction algorithms such as UMAP or tSNE, I was extremely skeptical of their efficacy. It seemed impossible to retain structure when losing that many dimensions. What made their usage click for me was the knowledge that, even if your data lives in a space that has thousands of dimensions (called the ambient space), there is a very good chance that the _local dimension_ of real-world data is of much lower dimension than this. For further understanding on this topic, check out the [presentation](https://www.youtube.com/watch?v=nq6iPZVUxZU){:target="blank"} that Leland McInnes (the creator of UMAP) gave on his algorithm.

In a sense, UMAP is a digital telescope that lets us look at constellations of high-dimensional data that we have never had the ability to visualize before. Algorithms like tSNE have worked in similar ways in the past, but UMAP is the first algorithm to be efficient enough to run on data with thousands of dimensions using something as prosaic as a laptop and a dream. This is to say that UMAP scales incredibly well, especially when compared to what is already out there.

Armed with UMAP, I fed the algorithm all 262,144 vectors (each with 256 dimensions, one for each complexity snapshot) and patiently waited for the embedding to complete. After fifteen minutes of my laptop revving up my fans, I had my first snapshot of the overarching structure of the Life-Like CA:

![Large Megallanic Cloud](/blog/images/automata_nebula/plots/selected_run/UMAP_CA_Full.png)

There it was, the massive serpent hiding in the structure of emergent complexity in automata. It is important to note that compressing dimensions can make parts of the data look separate in the embedding, even though they are connected in the ambient space they came from. It would be reasonable to assume that the serpent is one continuous entity, and the "jump" in the center was a result of the embedding.

While beautiful, this representation would not mean much if it did not accomplish the goal we set out to achieve: a metric for classification of rules that behave in similar ways to a given starting rule. Starting from the Game of Life, I began examining nearby rules and found that the metric did indeed yield other rules that produced uncanny behavior.

*Rules Close to the Game of Life (B3/S23)*

|B3/S23|B3/S013|B38/S013|B38/S238|
|-|-|-|-|
| <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/gol_like/6152.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/gol_like/5640.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/gol_like/5896.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/gol_like/137480.gif" width="200px" /> |

*Rules Close to Day and Night (B3678/S34678)*

|B3678/S34678|B36/S01456|B3678/S01456|B3567-S01478|
|-|-|-|-|
| <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/dan_like/242120.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/dan_like/58952.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/dan_like/59336.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/dan_like/206568.gif" width="200px" /> |

*Rules Close to Anneal (B4678/S35678)*

|B4678/S35678|B468/S035678|B0123578/S0124|B46/S035678|
|-|-|-|-|
| <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/anneal_like/250320.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/anneal_like/250704.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/anneal_like/12207.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/anneal_like/250448.gif" width="200px" /> |

*Rules Close to Maze-Finder (B138/S12357)*

|B138/S12357|B124/S123467|B0124/S0123467|B038/S012358|
|-|-|-|-|
| <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/maze_like/89354.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/maze_like/113686.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/maze_like/114199.gif" width="200px" /> | <img style="max-width:initial;" src="/blog/images/automata_nebula/animations/similar/maze_like/155401.gif" width="200px" /> |

## Filtering The Rules

While there is beauty in showing the structure of every possible rule, it is important to note that some classes of rules will not produce emergent behavior capable of universality. This is covered in more detail [in other research](https://www.ics.uci.edu/~eppstein/ca/wolfram.html){:target="blank"}. Essentially, rules that contain `B1` in their makeup will expand to infinity in all directions. Likewise, rules that contain `S01234` or `B23/S0` will do the same. Inversely, if a rule does not include `B2` or `B3`, any pattern will remain within its initial bounding box. This would make the formation of gliders impossible, which would prevent the formation of gliders. While it still may be possible for computation to occur in one-dimensional CA that exist on the boundaries of structures in both the first and second cases, I would like to examine only the types of CA that support universal structures that use gliders for the transport of information. This is how universality was proven in the Game of Life, after all.

In order to filter the rules so that we are only examining systems that are capable of both expansion and contraction, we just need to look at rules that don't contain `B1` and contain either `B2` or `B3`. This reduces our search space by at least a factor of four.
