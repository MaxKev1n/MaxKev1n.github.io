---
title: 'VESTA: A Secure and Efficient FHE-based Three-Party Vectorized Evaluation System for Tree Aggregation Models'

# Authors
# If you created a profile for a user (e.g. the default `me` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - Haosong Zhao
  - JunHao Huang
  - me
  - Kunxiong Zhu
  - Donglong Chen
  - Zhuoran Ji
  - Hongyuan Liu

# Author notes (optional)
# author_notes:
#   - 'Equal contribution'
#   - 'Equal contribution'

date: '2025-03-10T00:00:00Z'

# Schedule page publish date (NOT publication's date).
publishDate: '2017-01-01T00:00:00Z'

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ['paper-conference']

# Publication name and optional abbreviated publication name.
publication: In *Proceedings of the ACM on Measurement and Analysis of Computing Systems, Volume 9, Issue 1*
publication_short: In *Sigmetrics 2025*

abstract: Machine Learning as a Service (MLaaS) platforms simplify the development of machine learning applications across multiple parties. However, the model owner, compute server, and client user may not trust each other, creating a need for privacy-preserving approaches that allow applications to run without revealing proprietary data. In this work, we focus on a widely used classical machine learning model -- tree ensembles. While previous efforts have applied Fully Homomorphic Encryption (FHE) to this model, these solutions suffer from slow inference speeds and excessive memory consumption. To address these issues, we propose VESTA, which includes a compiler and a runtime to reduce tree evaluation time and memory usage. VESTA includes two key techniques, First, VESTA precomputes a portion of the expensive FHE operations at compile-time, improving inference speed. Second, VESTA uses a partitioning pass in its compiler to divide the ensemble model into sub-models, enabling task-level parallelism. Comprehensive evaluation shows that VESTA achieves a 2.1× speedup and reduces memory consumption by 59.4% compared to the state-of-the-art.

# Summary. An optional shortened abstract.
# summary: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis posuere tellus ac convallis placerat. Proin tincidunt magna sed ex sollicitudin condimentum.

tags:
  - Security

# Display this page in the Featured widget?
featured: true

# Standard identifiers for auto-linking
hugoblox:
  ids:
    doi: 10.1145/3711707

# Custom links
links:
  - type: pdf
    url: "3711707.pdf"
  # - type: code
  #   url: https://github.com/HugoBlox/kit
  # - type: dataset
  #   url: https://github.com/HugoBlox/kit
  # - type: slides
  #   url: https://www.slideshare.net/
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
