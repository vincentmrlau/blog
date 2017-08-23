---
layout: post
title:  "vue-slide-options document"
date:   2017-08-18 00:46:18
categories:
- front-end
- vue
- components
tags:
- front-end
- vue
- component
excerpt: document of an component for vue.js
published: true
---
## vue-slide-options
> dependence: webpack, vue-loader

![](https://raw.githubusercontent.com/vincentmrlau/remote-image-store/master/vue-slide-options.gif)

## START UP
### INSTALL
`npm i vue-slide-options`

### BASIC USAGE

```HTML
<vue-slide-options>
    <div slot="cover">an item 一个列表</div>
    <div slot="options">an options 一个选项</div>
</vue-slide-options>
```

### SLOT
![](https://raw.githubusercontent.com/vincentmrlau/remote-image-store/master/vue-slide-options-slots.png)
* cover: NEEDED
    * the element seen as the cover,like the chat list in the example image
* options:
    * the element hiding under the cover laying at the right side

### PROPS

|props|needed|type|default|description|
|:---|:---|:---|:---|:---|
|threshold|no|Number|0.5|0.5 means when touch move distance >= 0.5* options.width, when finger leave it will auto-slide to show all options, IF < ,auto-slide to hide them all|
|speed|no|Number|0.2|0.2 means finish auto-slide in 0.2s|
