+++
title = "Lightgallery in Markdown"
date = "2021-03-11"
description = "How to get lightgallery working for images in Markdown"
tags = ["markdown", "html", "javascript"]
showToc = true
+++

Many people use mkdocs for their documentation needs. Implementing a working lightbox for images does seem to be hard for some people to do! Getting a working lightbox js extension for all images in your Mardown docs is actually fairly simple, with the [lightgallery-markdown](https://github.com/g-provost/lightgallery-markdown) package on GitHub.
<!--more-->
The lightgallery-markdown extension is made to work with [lightgallery.js](https://github.com/sachinchoolur/lightgallery.js), a full featured JavaScript lightgallery/lightbox with no dependencies.

## Installation
You will need [pip](https://pip.pypa.io/en/stable/installing/) to easily install the lightgallery-markdown package.

To install the lightgallery-markdown package, run the following code in your terminal:
```
pip install lightgallery
```

## Making the Extension Work with MKDocs

I personally use [Material for MKDocs](https://squidfunk.github.io/mkdocs-material/) for my [documentation site](https://docs.nicklyss.com) and always noticed that it missed a lightbox feature for images. The lightgallery-markdown extension can work directly with Material for MKDocs and other MKDocs themes!

The following instructions will need to be done within your theme folder, either found in the MKDocs root folder in your libs packages or in the separate theme folder. You will need to add the following folders to your theme directory:
```
theme/
|_ css/
|_ fonts/
|_ img/
|_ js/
```
Next, we will download the [lightgallery.js](https://github.com/sachinchoolur/lightgallery.js) package from GitHub and move the following files into the theme sub-folders created above:

- dist/js/lightgallery.min.js -> theme/js/  

- dist/css/lightgallery.min.css -> theme/css/  

- dist/fonts/lg.* -> theme/fonts/  

- dist/img/loading.gif -> theme/img/  

It should look like this:

![Folder Layout](https://i.imgur.com/FwnLfYM.png)  

Most MKDocs themes will include a `main.html` file in the root theme folder, but if there isn’t one, create it. Add the following to your `main.html` file:
```html
{% extends "base.html" %}

{% block styles %}
    {{ super() }}
    <link rel="stylesheet" href="{{ base_url }}/css/lightgallery.min.css">
{% endblock styles %}

{% block libs %}
    {{ super() }}
    <script src="{{ base_url }}/js/lightgallery.min.js"></script>
{% endblock libs %}

{% block scripts %}
    {{ super() }}
    <script>
    var elements = document.getElementsByClassName("lightgallery");
    for(var i=0; i<elements.length; i++) {
       lightGallery(elements[i]);
    }
    </script>
{% endblock scripts %}
```
Finally, check your `mkdocs.yml` file and make sure the theme name from your theme folder is:
```
# Documentation and theme
theme: material

# Or if you used a custom theme
theme:
  custom_dir: 'theme'
```
and `lightgallery` must be added to your extensions:
```
# Extensions
markdown_extensions:
  - lightgallery
```

### Additional Settings Available
All settings of the extension are optional and can be omitted.


| Setting                            | Description                                                                                                     | Default Value |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------|---------------|
| show_description_in_lightgallery   | Adds the description as caption in lightgallery dialog.                                                         | `true`        |
| show_description_as_inline_caption | Adds the description as inline caption below the image.                                                         | `false`       |
| custom_inline_caption_css_class    | Custom CSS classes which are applied to the inline caption paragraph. Multiple classes are separated via space. | Empty         |  

## Writing Correct Image Markdown
The lightgallery-markdown extension will only wrap images with an added “!” right after the opening “[” bracket of the image. For example:
```
![!Description](/img/pic1.png)
```
This should give you the following HTML output when you run mkdocs build:
```html
<p>
  <div class="lightgallery">
    <a href="../img/pic1.png" data-sub-html="Description">
      <img alt="Description" src="../img/pic1.png" />
    </a>
  </div>
</p>
```