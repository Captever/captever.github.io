---
title: "Layout: Header Image Overlay"
lang: en
header:
  overlay_image: /assets/images/unsplash-image-1.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "Call to action 1"
      url: "https://github.com"
    - label: "Call to action 2"
      url: "https://mademistakes.com"
categories:
  - Layout
  - Uncategorized
tags:
  - edge case
  - image
  - layout
last_modified_at: 2016-05-02T11:39:01-04:00
---

This post should display a **header with an overlay image**, if the theme supports it.

Non-square images can provide some unique styling issues.

This post tests overlay header images.

## Overlay filter

You can use it by specifying the opacity (between 0 and 1) of a black overlay like so:

![transparent black overlay]({{ '/assets/images/mm-header-overlay-black-filter.jpg' | relative_url }})

```yaml
excerpt: "This post should [...]"
header:
  overlay_image: /assets/images/unsplash-image-1.jpg
  overlay_filter: 0.5 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "More Info"
      url: "https://unsplash.com"
```

Or if you want to do more fancy things, go full rgba:

![transparent red overlay]({{ '/assets/images/mm-header-overlay-red-filter.jpg' | relative_url }})

```yaml
excerpt: "This post should [...]"
header:
  overlay_image: /assets/images/unsplash-image-1.jpg
  overlay_filter: rgba(255, 0, 0, 0.5)
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
  actions:
    - label: "More Info"
      url: "https://unsplash.com"
```