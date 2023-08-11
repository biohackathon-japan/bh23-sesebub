---
title: 'Introducing the Pangenome Variation Structure Tree (PVST) Model for Comprehensive Variant Detection and Graph Decomposition'
title_short: 'Pangenome Variation Structure Tree (PVST)'
tags:
  - Pangenome Variation Structure Tree
  - Bubbles
  - Graphs
authors:
  - name: Moses Njagi Mwaniki
    orcid: 0000-0002-4858-2375
    affiliation: 1
  - name: Nadia Pisanti
    orcid: 0000-0003-3915-7665
    affiliation: 1
  - name: Michael R. Crusoe
    orcid: 0000-0002-2961-9670
    affiliation: 3
  - name: Erik Garrison
    orcid: 0000-0003-3821-631X
    affiliation: 2
affiliations:
  - name: Department of Computer Science, University of Pisa, Italy
    index: 1
  - name: University of Tennessee Health Science Center, USA
    index: 2
  - name: ELIXIR-Germany (via FZ Jülich); VU Amsterdam
    index: 3
date: 30 June 2023
cito-bibliography: paper.bib
event: BH23JP
biohackathon_name: "BioHackathon Japan 2023"
biohackathon_url:   "https://2023.biohackathon.org/"
biohackathon_location: "Kagawa, Japan, 2023"
group: R4
# URL to project git repo --- should contain the actual paper.md:
git_url: https://github.com/biohackathon-japan/bh23-sesebub
# This is the short authors description that is used at the
# bottom of the generated paper (typically the first two authors):
authors_short: Moses Njagi Mwaniki \emph{et al.}
---

# Background


Pangenomics, a vital field in genomics, seeks to analyze and understand the complete set of genes in a species. The significance of this approach lies in its broad perspective. Instead of focusing solely on a single reference genome, pangenomics encompasses the entire spectrum of genetic variation present within a species. This consideration of comprehensive genetic diversity has profound implications for studies in evolution, disease genetics, and more.



A key player in the field of pangenomics is the pangenome graph. This data structure is instrumental in representing the genetic diversity of a species. The graph outlines sequences as nodes and maps the genetic variants as paths running through these nodes. 

The term "bubble" has various is not consisitent and changes based on complexity. The initial definition and there are also ultra bubbles superbubbles and snarls [1,2].
Therefore it is necessary for us to provide a formal definition of a bubble.
A specific region in this pangenome graph with a single entry and exit point, housing variation within, is referred to as a "bubble". These bubbles are analogous to genetic sites, housing multiple potential sequences, hence they can be interpreted as genetic variants. Identifying these bubbles within the pangenome graph, thus, becomes pivotal to unravelling the rich tapestry of genetic diversity in a species.

However, detecting these bubbles, which represent genetic variants, has proven to be a challenging task due to the limitations of existing bubble finding algorithms. Most of these algorithms are either bound to Directed Acyclic Graphs (DAGs), making them ill-equipped to handle realistic pangenome graphs that incorporate cycles and inversions, or are burdened by hefty computational bounds such as O(N^2), limiting the size of detectable bubbles. Furthermore, they exhibit a peculiar flaw where they overlook specific forms of nested variation, such as Single Nucleotide Polymorphisms (SNPs) found at the beginning or end of indels.

Adding to the complexity is the fact that these algorithms have proven to be remarkably complicated to implement. This has primarily stemmed from these methods growing organically as an offshoot of previous work, without a clear, streamlined approach. This complexity not only presents a formidable hurdle for researchers and developers but also limits the applicability and reusability of these algorithms. A notable example is the multi-phase algorithm used in the VG toolkit, which, despite its effectiveness in finding variants in arbitrary pangenome graphs, has only seen a single implementation due to its intricacy.

Addressing these challenges and the need for a simpler, more versatile solution, we turned our focus to a concept from a different domain that seemed to offer promising insights. This was the Program Structure Tree (PST) concept, which was initially introduced by Johnson in 1994. The PST concept, applied to cyclic flow graphs, was attractive because it seemed to provide a direct, isomorphic mapping onto our problem domain. This compatibility arises due to the formal equivalence between the variation graph model, which we are working on, and a flow graph. Notably, the PST concept only requires a single depth-first traversal of the flow graph, making it a potential candidate for a more straightforward approach to detecting bubbles in pangenome graphs.

# Genomic Variation and the Pangenome Variation Structure Tree

Genomic variation is a fundamental concept in biology and genetics that explains the differences between individual genomes. For clarity, we define a genomic variant as a site in homologous regions of two or more genomes, where the sequences within these regions differ in one or more ways. These differences are bracketed by similar sequences that flank them, which suggests a common genetic ancestor. The recognition of this variation is critical for comparative genomics, phylogenetics, and evolutionary studies, as it helps us understand the trajectory and scope of sequence evolution.

Conceptualizing genomic variation, particularly in the context of a pangenome, necessitates a versatile and robust model. In this regard, we propose a Pangenome Variation Structure Tree (PVST), from the detection of cycle equivalent Single Entry Single Exit regions.
This is based on work from the Program Structure Tree (PST)  traditionally used in compilers to break a program into isolatable regions or Single Entry Single Exit (SESE) regions, proves advantageous when applied to pangenomic research. It allows us to decompose the pangenome into analogous isolatable regions, each of which can be managed independently. This facilitates various processes like local compression, paging out to disk, or dynamic network access, thereby circumventing the need for loading the entire pangenome graph simultaneously in memory.

However, unlike program flow, which is linear and one-directional, DNA is bidirectional. It consists of two strands and can be traversed in either direction. This duality of DNA led us to define PVST on a bi-edged graph, where two types of edges are identified—gray and black. The black edges correspond to sequences of DNA, while the gray ones connect these sequences. Any genome must be spelled as a series of alternating black and gray edge traversals. The bi-edged graph allows us to define SESE regions and consequently isolate genomic variants.

The PVST, when applied to a pangenome graph, allows for comprehensive detection and characterization of genomic variants, which we define as bubbles. These bubbles, SESE regions on a pangenome graph, represent variable sites that are supported by the sequence. Notably, our model's utility extends to the ends of sequences where the flanking homology can be perceived as a virtual start or ending node, as seen in a flow graph.

Existing literature has primarily characterized genetic variation using concepts like bubbles, superbubbles, ultrabubbles, and snarls. However, the PVST model, with its emphasis on isolating SESE regions, offers a more comprehensive and nuanced approach to identifying and understanding genomic variation. This allows us to avoid the issues faced with previous models, such as the snarl decomposition model, which consistently missed 1-2% of small variants due to mathematical limitations. We anticipate that the PVST model's successful application will significantly enhance our understanding of genomic variation and provide a robust platform for future research in the field.


# Outcomes

The central achievement of our work in this hackathon has been the successful development and implementation of the Program Structure Tree (PST) algorithm to detect genetic variants in pangenome graphs. The PST algorithm's application in this domain has given rise to the Pangenome Variation Structure Tree (PVST) model, a novel tool that can effectively isolate single-entry single-exit (SESE) regions in pangenome graphs. By doing so, it has the potential to radically enhance our understanding and utilization of pangenomic data.

Given that pangenome graphs have a formal equivalence with cyclic flow graphs to which the PST concept applies, we were able to develop and debug the core cycle equivalency algorithm in the programming languages C++ and Rust. We prioritized ensuring the correctness of this algorithm in our implementations, as understanding it completely is crucial for any potential applications in pangenomics.

Our PVST model allows for a detailed examination of the interplay between large structural variations (SVs) and smaller variations like SNPs and indels. This unique perspective, along with the tree topology of the PVST model, can provide valuable insights into evolutionary processes that are difficult to explore when using a single reference genome as a guide.

Furthermore, the PVST model can serve as the basis for enhancing a set of algorithms that rely on a model of the topology of the pangenome graph. This set includes algorithms for sequence alignment to the graph, graph sorting, 2D layout visualization, and graph decomposition or "untangling," where we extract pairwise relationships over the embedded paths, or genomes, in the graph.

As part of our algorithm's development process, we created numerous debugging tools and visualizations to aid our understanding of the algorithm's flow and progression. This intensive, hands-on approach not only allowed us to comprehend the algorithm fully but also helped us guarantee its accuracy in our implementations. Additionally, our collaboration during the hackathon allowed us to exchange ideas and debug each other's work and conceptual understanding of the problem in real-time. This dynamic exchange and collaboration were instrumental in leading us to the successful independent implementation of the PVST.

Finally, we took significant measures to ensure that our implementations are accessible and reusable. We've painstakingly documented our implementations inline, following the pseudocode outlined in the original PST paper. Our commitment to maintaining detailed documentation is intended to benefit the broader scientific community, as it will enable other researchers to build upon our work more effectively. We've shared our work openly in the BioHackrXiv markdown repository on Github, encouraging other scientists to contribute, refine, and expand on our initial developments. The development of these multiple implementations and the associated broad knowledge dissemination is an essential step towards democratizing the application of the PST algorithm in pangenomics.

## Supporting outcome
We also [wrote a tool](https://github.com/ekg/subarch-select) to support picking the best optimized binary for x86-64 systems; useful for packaging (Debian, Conda, et cetera) and software containers (Docker, Singularity/Apptainer, et cetera). The tool was then [packaged](https://salsa.debian.org/med-team/subarch-select/) and [submitted to Debian](https://alioth-lists.debian.net/pipermail/debian-med-packaging/2023-June/108645.html) and will be useful in improving the execution speed of many bioinformatics tools already in Debian.

# Future work

Our hackathon success sets a strong foundation for an exciting future in advancing pangenomic research. The application of the Pangenome Variation Structure Tree (PVST) model promises to open new avenues in pangenome compression, representation, and understanding of evolutionary processes.

Our immediate plans include applying the PVST model to detect variation in the Human Pangenome Project, in which we are both participants. This venture will resolve a key problem that the project encountered in its past work, specifically the inability of the snarl decomposition model to detect all small variants. Notably, this was a consistent limitation of previous models, consistently missing 1-2% due to mathematical restrictions inherent in these models. We believe the PVST will not suffer from this limitation, providing a more comprehensive and accurate variant extraction.

We also plan to explore the implications of making the pangenome graphs "bi-edged." This transformation process involves converting each node into two nodes connected by a new kind of edge (a "black" edge), while retaining the original edges as "grey" edges between the new nodes. SESE regions bounded by black edges in these bi-edged graphs can be seen as "bubbles" or traditional genetic sites, while those bounded by grey edges can be seen as alleles within the bubbles. Completing this decomposition will enable us to find internal alleles missed by previous bubble-finding methods. This development will enhance the applicability of the PVST model and address observed problems with previous bubble-finding methods.

In addition to these advancements, we anticipate that the PVST model will play a significant role in breaking down the pangenome into isolatable regions. Similar to how the PST is used by compilers to separate a program into isolatable regions (the SESE regions), the PVST can help us partition the pangenome into discrete sections. Each region can then be individually managed, potentially allowing for local compression, or for parts of the pangenome graph to be stored on disk or accessed over the network dynamically. This could greatly reduce the need for large amounts of memory to simultaneously load all parts of the pangenome graph.

In our continued work, we will focus on hammering out the precise mapping of PST concepts onto variation in pangenomes. While we have made a good start with the development of the PVST, there's much more to be done. We need to confirm that the bi-edged transformation of the input pangenome variation graph is sufficient to resolve the observed issues with previous bubble-finding methods. Beyond this, there will be significant work in developing the system to emit a variant call format (VCF) file, facilitating downstream analysis in standard bioinformatic processes.

Our goal for the future is to continue to develop and expand our implementations, leading to libraries in key systems languages (targets are C, C++, and Rust). We are committed to facilitating the broader application and understanding of the PST algorithm in the pangenomic community

## Acknowledgements

We would like to thank the fellow participants at BioHackathon 2023 for their collaboration and constructive advice, which greatly influenced our project. We are grateful to the organizers for providing this platform and the developers of open source language models. Special thanks to our mentors, advisors, and colleagues for their guidance and support. Without their contributions, our project in linked data standardization with LLMs in bioinformatics would not have been possible.

Further, we would also like to acknowledge that this preprint is an outcome of a project that has received funding from the European Union’s Horizon 2020 research and innovation programme under the Marie Skłodowska-Curie grant agreement No 956229. Co-financed by the Connecting Europe Facility of the European Union.

The resources and backing from these institutions have been integral in allowing us to explore and innovate in the field of bioinformatics, and we are truly appreciative of their contributions to our work.

## References

1. Taku Onodera, Kunihiko Sadakane, and Tetsuo Shibuya. Detecting Superbubbles in Assembly Graphs. In Aaron Darling and Jens Stoye, editors, Algorithms in Bioinformatics, Lecture Notes in Computer Science, pages 338–348, Berlin, Heidelberg, 2013. Springer. doi:10.1007/978-3-642-40453-5_26

2. Benedict Paten, Adam M Novak, Erik Garrison, and Glenn Hickey. Superbubbles, Ultrabubbles and Cacti. bioRxiv, page 101493, January 2017. doi:10.1fcite101/101493
