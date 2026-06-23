---
title: 'cuPTW: Leveraging Idle Compute Units for Massively Parallel GPU Page Table Walks'

# Authors
# If you created a profile for a user (e.g. the default `me` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - me
  - Tianao Ge
  - Lieven Eeckhout
  - Hongyuan Liu
  - Jiayi Huang

# Author notes (optional)
# author_notes:
#   - 'Equal contribution'
#   - 'Equal contribution'

date: '2026-06-10T00:00:00Z'

# Schedule page publish date (NOT publication's date).
publishDate: '2017-01-01T00:00:00Z'

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ['paper-conference']

# Publication name and optional abbreviated publication name.
publication: In *Proceedings of the ACM on Measurement and Analysis of Computing Systems, Volume 10, Issue 2*
publication_short: In *Sigmetrics 2026*

abstract: Virtual memory has become a cornerstone of modern GPUs, enabling unified address spaces and advanced memory management techniques. However, the performance of address translation has emerged as a critical bottleneck, particularly under irregular workloads with massive memory footprints, where frequent TLB misses and costly page table walks dominate total memory access latency. Prior work has primarily focused on improving TLB effectiveness or optimizing page table walks through batching and coalescing, but these approaches remain limited by the lack of locality and memory bandwidth constraints. In this work, we propose Compute Unit Page Table Walk (cuPTW), a novel address translation architecture that repurposes idle GPU compute resources to accelerate page table walks. Specifically, we introduce (1) a single-threaded synchronous cuPTW, which offloads translation requests to idle functional units within compute units, and (2) two optimizations that further reduce latency and improve throughput by caching page table walks in local data store memory (cuPTW-SW) and parallelizing them across multiple SIMD lanes (cuPTW-MT). When combined, cuPTW-Full transforms low-parallelism page table walks into a massively parallel computation task. Our evaluation across 15 representative GPU workloads, including deep learning, graph analytics and scientific simulations, demonstrates that cuPTW-Full achieves a performance speedup of 4.43× on average (up to 76.09×) by improving page table walk throughput by 9.92× on average over our baseline GPU architecture. Compared to state-of-the-art GPU address translation proposals, cuPTW-Full achieves a 1.97× to 2.08× average speedup.

# Summary. An optional shortened abstract.
# summary: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis posuere tellus ac convallis placerat. Proin tincidunt magna sed ex sollicitudin condimentum.

tags:
  - GPU Archiecture

# Display this page in the Featured widget?
featured: true

# Standard identifiers for auto-linking
hugoblox:
  ids:
    doi: 10.1145/3805633

# Custom links
links:
  - type: pdf
    url: "3805633.pdf"
  # - type: code
  #   url: https://github.com/HugoBlox/kit
  # - type: dataset
  #   url: https://github.com/HugoBlox/kit
  - type: slides
    url: "slides.pdf"
  # - type: source
  #   url: https://github.com/HugoBlox/kit
  # - type: video
  #   url: https://youtube.com

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# image:
#   caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/pLCdAaMFLTE)'
#   focal_point: ''
#   preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
# projects:
#   - example

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: ""
---

<!-- > [!NOTE]
> Click the _Cite_ button above to demo the feature to enable visitors to import publication metadata into their reference management software.

> [!NOTE]
> Create your slides in Markdown - click the _Slides_ button to check out the example.

Add the publication's **full text** or **supplementary notes** here. You can use rich formatting such as including [code, math, and images](https://docs.hugoblox.com/content/writing-markdown-latex/). -->
