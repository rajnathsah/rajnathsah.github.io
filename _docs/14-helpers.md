---
title: "Helpers"
permalink: /docs/helpers/
excerpt: "Jekyll `_includes` and other helpers to use as shortcuts for creating archives, galleries, table of contents, and more."
gallery:
  - url: /assets/images/unsplash-gallery-image-1.jpg
    image_path: /assets/images/unsplash-gallery-image-1-th.jpg
    alt: "placeholder image 1"
    title: "Image 1 title caption"
  - url: /assets/images/unsplash-gallery-image-2.jpg
    image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "Image 2 title caption"
  - url: /assets/images/unsplash-gallery-image-3.jpg
    image_path: /assets/images/unsplash-gallery-image-3-th.jpg
    alt: "placeholder image 3"
    title: "Image 3 title caption"
feature_row:
  - image_path: /assets/images/unsplash-gallery-image-1-th.jpg
    alt: "placeholder image 1"
    title: "Placeholder 1"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
  - image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "Placeholder 2"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--inverse"
  - image_path: /assets/images/unsplash-gallery-image-3-th.jpg
    title: "Placeholder 3"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
last_modified_at: 2020-05-01T10:22:56-04:00
toc: true
toc_label: "Helpers"
toc_icon: "cogs"
---

You can think of these Jekyll helpers as little shortcuts. Since GitHub Pages doesn't allow most plugins --- [custom tags](https://jekyllrb.com/docs/plugins/#tags) are out. Instead the theme leverages [**includes**](https://jekyllrb.com/docs/templates/#includes) to do something similar.

## Group by array

[Jekyll Group-By-Array](https://github.com/mushishi78/jekyll-group-by-array) by Max White.

A liquid include file for Jekyll that allows an object to be grouped by an array.

## Figure

Generate a `<figure>` element with a single image and caption.

| Include Parameter | Required     | Description                                                                                          |
| ----------------- | ------------ | ---------------------------------------------------------------------------------------------------- |
| **image_path**    | **Required** | Full path to image eg: `/assets/images/filename.jpg`. Use absolute URLS for those hosted externally. |
| **alt**           | Optional     | Alternate text for image.                                                                            |
| **caption**       | Optional     | Figure caption text. Markdown is allowed.                                                            |

Using the `figure` include like so:

