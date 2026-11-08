---
title: 'NB-Walker: Non-blocking Page Table Walker to Enhance Address Translation in NUMA GPUs'

# Authors
# If you created a profile for a user (e.g. the default `me` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - me
  - Lieven Eeckhout
  - Hongyuan Liu
  - Tianao Ge
  - Xinkai Wang
  - Jiayi Huang

# Author notes (optional)
# author_notes:
#   - 'Equal contribution'
#   - 'Equal contribution'

date: '2026-10-31T00:00:00Z'

# Schedule page publish date (NOT publication's date).
publishDate: '2017-01-01T00:00:00Z'

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ['paper-conference']

# Publication name and optional abbreviated publication name.
publication: 
publication_short: In *MICRO 2026*

abstract: Modern Graphics Processing Unit (GPU) architectures have transitioned toward multi-partitioned or chiplet-based designs to scale compute throughput and memory capacity. However, this evolution introduces significant internal Non-Uniform Memory Access (NUMA) effects, where cross-partition memory latencies diverge significantly. As address translation requires sequential, multi-level probes, these NUMA-induced latencies are amplified, placing unprecedented pressure on the memory management unit. This paper identifies a critical bottleneck: traditional page table walkers employ a rigid, blocking design, where walker hardware resources are held by in-flight requests until the entire multi-level walk completes. Under NUMA conditions, long-latency remote memory probes monopolize these resources, causing translation requests to queue, which eventually stalls the entire GPU pipeline. To mitigate these translation-induced stalls, we propose NB-WALKER, a high-throughput, non-blocking address translation architecture. NB-WALKER decouples translation issuance from limited page walk buffers by repurposing underutilized L1 cache MSHR entries to buffer walk metadata and track outstanding page table entry fetch requests. This transformation allows the page table walkers to sustain multiple outstanding translation requests concurrently, significantly enhancing memory-level parallelism during the walk process. Furthermore, our design enables opportunistic coalescing of concurrent page table probes, reducing redundant memory traffic to the last-level cache. Experimental results demonstrate that NB-WALKER significantly alleviates translation pressure, achieving an average 5.56× increase in page table walk throughput. This enhanced concurrency translates into an average speedup of 2.78× (up to 5.73×) across translation-intensive workloads with negligible hardware overhead.

# Summary. An optional shortened abstract.
# summary: Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis posuere tellus ac convallis placerat. Proin tincidunt magna sed ex sollicitudin condimentum.

tags:
  - GPU Archiecture

# Display this page in the Featured widget?
featured: true

# Standard identifiers for auto-linking
# hugoblox:
#   ids:
#     doi: 10.1145/3805633

# Custom links
links:
  - type: pdf
    url: "paper.pdf"
  # - type: code
  #   url: https://github.com/HugoBlox/kit
  # - type: dataset
  #   url: https://github.com/HugoBlox/kit
  - type: slides
    url: "slides.pdf"
  - type: source
    url: https://github.com/redbird-arch/micro2026-nbwalker-artifact
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
