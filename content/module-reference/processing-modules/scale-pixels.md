---
title: scale pixels
id: scale-pixels
weight: 10
applicable-version: 3.2.1
tags: 
working-color-space: RGB 
view: darkroom
masking: false
---

Some cameras (such as the Nikon D1X) have rectangular instead of the usual square sensor cells. Without correction this would lead to distorted images. This module applies the required scaling.

darktable detects images that need correction using their Exif data and automatically activates this module where required. 

The module's only control is **pixel aspect ratio**. For cameras that need it, it will be set automatically. It may also be set manually for transformations like anamorphic desqueeze.
