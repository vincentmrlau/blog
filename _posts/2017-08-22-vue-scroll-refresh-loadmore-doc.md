---
layout: post
title:  "vue-scroll-refresh-loadmore document"
date:   2017-08-22 00:46:18
tags:
- front-end
- vue
- components

excerpt: document of an component for vue.js
published: true
---
## vue-scroll-refresh-loadmore
> scroll for refresh an load more

![](https://raw.githubusercontent.com/vincentmrlau/remote-image-store/master/vue-scroll-pulldown.gif)

![](https://raw.githubusercontent.com/vincentmrlau/remote-image-store/master/vue-scroll-pullup.gif)

## START UP
### INSTALL

```bash
npm install vue-scroll-refresh-loadmore --save
```

### BASIC USE

```html
<scroller
      @on-refresh="refresh"
      @on-loadmore="loadmore"
      ref="chatScroller">
   <item />
   <item />
   <item />
</scroller>

```

methods

```javascript
methods: {
      refresh () {
        console.log('refresh')
        window.setTimeout(() => {
          this.$refs.chatScroller.refreshDone()
        }, 3000)
      },
      loadmore () {
        console.log('load more')
        window.setTimeout(() => {
          this.$refs.chatScroller.loadmoreDone()
        }, 3000)
      }
    },
```

### props

|props|type|required|description|default|
|:---|:---|:---|:---|:---|:---|
|pulldownText|String|N|text show when pulling done|Pull down to refresh|
|onPulldownText||||Release to refresh|
|pulldownRefreshingText||||loading...|
|scrollRefreshingText|||footer loading more text|loading...|
|headerMaxHeight|Number||in px|50|
|footerMaxHeight|||in px|30|
|refreshThreshold|Number||0.5 means when pulldown distance >= 0.5 * headerMaxHeight ,that is enough for release refreshing|0.5|
|loadMoreThreshold| Number ||the distance (px) from bottom for loading more|10|
|headerRefreshDisabled|Boolean||when set true, `on-refresh` will not emit|false|
|footerLoadDisabled|Boolean||when set true, `on-loadmore` will not emit|false|

### events

* on-refresh:
	* emit when pulldown a enough distance
	* will not emit when refreshing, or `headerRefreshDisabled === true` ,must call the method `refreshDone` to reset

* on-loadmore:
	* emit when pullup to the bottom(or the loadMoreThreshold set)
	* will not emit when refreshing, or `footerLoadDisabled === true` ,must call the method `footerLoadDisabled` to reset

### methods

* refreshDone:
	* hide header loading
	* reset header refreshing statu
	* scrollTop = 0
* loadmoreDone:
	* reset footer loading