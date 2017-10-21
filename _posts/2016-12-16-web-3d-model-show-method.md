---
layout: post
title:  "web使用three.js展示&操作外部的3D模型"
date:   2016-12-16 00:46:18
tags:
- front-end
- three.js
- components
- canvas
excerpt: three.js的官方文档和例程给出的功能很多，但是很多web前端搬砖工的需求可能是展示建模师或者美术姐姐做出来的模型，或者对这些模型进行简单的操作（换纹理，换颜色，换模型），如果你的需求是这样，那么，小弟这里有个简单的小程序，各位大神可以看一下
published: true
---
## 写在前面
关于three.js的基础知识和参考资料，我推荐两份文档

1. 	[http://davidscottlyons.com/threejs/presentations/frontporch14/#slide-0](http://davidscottlyons.com/threejs/presentations/frontporch14/#slide-0)
2. 	[http://www.ituring.com.cn/minibook/792 ](http://www.ituring.com.cn/minibook/792 )

第二份不知道是哪个大神写的，写得很好，看起来一点都不费劲，看完之后就知道了很多基础的知识（webGL,renderer,scene,camera,光线，模型，网格，纹理等等）。

three.js的官方文档和例程给出的功能很多，但是很多web前端搬砖工的需求可能是展示建模师或者美术姐姐做出来的模型，或者对这些模型进行简单的操作（换纹理，换颜色，换模型），如果你的需求是这样，那么，小弟这里有个简单的小程序，各位大神可以看一下。

## 先说功能

*	简单粗暴，一个方法构建一个简单的3D展示舞台（方法名：base3DshowStateInitial），默认带白色环境灯和两盏白色点光源，同时支持360度无死角观察模型，支持手势操作,支持自动旋转，（其实就是[OrbitControls.js](https://github.com/mrdoob/three.js/blob/dev/examples/js/controls/OrbitControls.js)），另外，也把renderer,scene,camera返还出来，以供各位大神作另外的操作了。
*	加载OBJ+MTL格式的外部模型（方法名：MTLOBJLoaderFromPath）。这里多说一句，threeJS有个FBXLoader的，FBX据建模届的同事说是比较通用的格式，但是这里的FBXLoader只支持ASCII格式的FBX文件，一般的建模软件导出来的是二进制文件（Binary format），需要使用的同学记得在导出模型的时候选好导出的格式了。
*	使用以上两个方法就可以实现在web里面展示一个外部的3d模型了，是不是很爽呢。下面就是我的源代码了啊

##	应用
###	应用步骤说明
1.	HTML文件设置一个canvas ,id = "modelcanvas"
2.	调用"base3DshowStateInitial"方法生成一个3D模型的展示舞台，参数设置以及参数作用如下：
	*	canvasid:"modelcanvas"//用于选中HTML的canvas
	*	canvasheight:'height'//选中的HTML的canvas的height
	*	canvaswidth:'width'//选中的HTML的canvas的width，和height配合决定摄像机的长宽比，使模型展示出来不会变形
	*	modelboxsize:'modelboxsize'//能包围模型的最小的正方体的边长，以保证模型能在舞台中不会过大也不会过小的显示出来
	*	autoRotate:'false'//我不要摄像头自动旋转
	*	backgroud:"#B8B8B8"//设置舞台背景颜色
3.	调用"MTLOBJLoaderFromPath"方法，加载模型，参数设置及原因如下:
	*	modelpath//模型的路径
	*	mtlfilename//模型mtl文件文件名，由modelpath+mtlfilename决定加载mtl文件的路径
	*	objfilename//模型mtl文件文件名，由modelpath+mtlfilename决定加载mtl文件的路径
4.	在"MTLOBJLoaderFromPath"方法中的加载成功回调函数中调节模型位置'object.position.set(0,-('modelboxheight'/2),0);',**把模型加载到场景中‘scene.add(object);’**

```js
var state = vt.base3DshowStateInitial({
			canvasid:"modelcanvas"
			,canvasheight:'height'
			,canvaswidth:'width'
			,modelboxsize:'modelboxsize'
			,autoRotate:'false'
			,backgroud:"#B8B8B8"
		});
var scene = state.scene;
var camera = state.camera;
var renderer = state.renderer;

//load model

vt.MTLOBJLoaderFromPath(
	'modelpath'
	,'mtlfilename'
	, 'objfilename'
	, function(object){
		//设置居中
		object.position.set(0,-('modelboxheight'/2),0);
		object.scale.set(1,1,1);
		scene.add(object);

	}, function(){
		//onError
	}, function(xhr){
		if ( xhr.lengthComputable ) {
			var percentComplete = xhr.loaded / xhr.total * 100;
			console.log( Math.round(percentComplete, 2) + '% downloaded' );
		}

})
```


## 源代码&&注释


### rely on
*	[three.js](https://threejs.org/)
*	[MTLLoader.js](https://github.com/mrdoob/three.js/tree/dev/examples/js/loaders)
*	[OBJLoader.js](https://github.com/mrdoob/three.js/tree/dev/examples/js/loaders)
*	[OrbitControls.js](https://github.com/mrdoob/three.js/blob/dev/examples/js/controls/OrbitControls.js)

* data: Thu Oct 20 2016 14:54:31 GMT+0800

### author info
*	`Vincent Lau`
* 	`mrliuyiman@foxmail.com`
*	`Guangzhou,China`

### functions
#### vt.base3DshowStateInitial(options)
initialize a base 3D model show state,by initialing
a base scene with PerspectiveCamera,5 width PointLights,
camera controls,background and WebGLRenderer.
##### parameters
*	`options` valid keys:(+ means necessary)
	*	`canvasid` {string} +
	*	`canvaswidth` {num} +
	*	`canvasheight` {num} +
	*	`modelboxsize` {num} + the max length,(minimeter)
	*	`backgroud` {string} default "#FFFFFF"
	*	`axisHelper` {boolean} default: false
	*	`dampingFactor` {num} defaule: 0.7
	*	`enableZoom` {boolean} defaule: true
	*	`autoRotate` {boolean} defaule: true
	*	`enableDamping` {boolean} defaule: true
	*	`lightOn` {boolean} defaule: true
##### return
	*	```
		{
			scene:scene
			,camera:camera
			,renderer:renderer
		}```

#### vt.MTLOBJLoaderFromPath(path,mtlFileName,objFileName,success,error,progress)
load an 3D model , .mtl & .obj
#####	paramerters
*	`path` {string}
*	`mtlFileName` {string}
*	`objFileName` {string}
*	`success` {function} call:success(object)
*	`error` {function} call:error(xhr)
*	`progress` {function} call:progrees(xhr)


### code
```js
(function(window){
	"use strict";

	window.vt = {};//vt means vincent-threejs

	/*
	## vt.base3DshowStateInitial(options)
	initialize a base 3D model show state,by initialing
	a base scene with PerspectiveCamera,5 width PointLights,
	camera controls,background and WebGLRenderer.
	## parameters
	*	`options` valid keys:(+ means necessary)
		*	`canvasid` {string} +
		*	`canvaswidth` {num} +
		*	`canvasheight` {num} +
		*	`modelboxsize` {num} + the max length,(minimeter)
		*	`backgroud` {string} default "#FFFFFF"
		*	`axisHelper` {boolean} default: false
		*	`dampingFactor` {num} defaule: 0.7
		*	`enableZoom` {boolean} defaule: true
		*	`autoRotate` {boolean} defaule: true
		*	`enableDamping` {boolean} defaule: true
		*	`lightOn` {boolean} defaule: true
	## return
	*	{
			scene:scene
			,camera:camera
			,renderer:renderer
		}
	*/
	vt.base3DshowStateInitial = function(options)
	{
		var renderer = new THREE.WebGLRenderer({
			canvas:document.getElementById(options.canvasid)
			,antialias: true
		});
		var background = options.backgroud||"#FFFFFF";
		renderer.setClearColor(background,1);
		var scene = new THREE.Scene();
		var viwerate = options.canvaswidth/options.canvasheight;
		var viwefar = options.modelboxsize*10
		var camera = new THREE.PerspectiveCamera(60,viwerate,0.1,viwefar)
		camera.position.set(options.modelboxsize*1
			,options.modelboxsize*1
			,options.modelboxsize*1);
		//set poitlight
		var lightOn = options.lightOn||true;
		if(lightOn){
			var distance = options.modelboxsize*2;
			var x = distance;
			var y = distance;
			var z = distance;



			var abienLight = new THREE.AmbientLight( 0x404040,3 );
			scene.add(abienLight);

			var pointLight1 = new THREE.PointLight( 0x404040 ,2);
			scene.add(pointLight1);
			pointLight1.position.set(-distance*2,-distance*2,-distance*2);

			var pointLight = new THREE.PointLight( 0x404040 ,2);
			scene.add(pointLight);
			pointLight.position.set(2*distance,2*distance,2*distance);
		}
		//set camera control
		var controls = new THREE.OrbitControls(camera,renderer.domElement);
		controls.dampingFactor = options.dampingFactor||0.7;
		controls.enableZoom = true;
		controls.autoRotate = true;
		controls.enableDamping = true;
		if (options.enableZoom === false) {
			controls.enableZoom = options.enableZoom;
		}
		if (options.autoRotate === false) {
			controls.autoRotate = options.autoRotate;
		}
		if (options.enableDamping === false) {
			controls.enableDamping = options.enableDamping;
		}

		//set axisHelper
		if(options.axisHelper){
			var axisHelper = new THREE.AxisHelper( options.modelboxsize*10 );
			scene.add( axisHelper );
		}

		//set animationframe
		 window.requestAnimationFrame = window.requestAnimationFrame
		         || window.mozRequestAnimationFrame
		         || window.webkitRequestAnimationFrame
		         || window.msRequestAnimationFrame;
		 function render()
		 {

		 		controls.update();
		 		renderer.render(scene, camera);//rendering

				requestAnimationFrame(render);
		 }
		 render();


		return {
			scene:scene
			,camera:camera
			,renderer:renderer
		}
	}

	/*
	##	vt.MTLOBJLoaderFromPath(path,mtlFileName,objFileName,success,error,progress)
	load an 3D model , .mtl & .obj
	###	paramerters
	*	`path` {string}
	*	`mtlFileName` {string}
	*	`objFileName` {string}
	*	`success` {function} call:success(object)
	*	`error` {function} call:error(xhr)
	*	`progress` {function} call:progrees(xhr)
	*/
	vt.MTLOBJLoaderFromPath = function(path,mtlFileName,objFileName,success,error,progress)
	{
		var onError = function(xhr){
			if(typeof error == "function"){
				error(xhr);
			}
		}
		var onProgress = function(xhr){
			if(typeof progress == "function"){
				progress(xhr);
			}
		}
		var mtlLoader = new THREE.MTLLoader();
		mtlLoader.setPath(path);
		mtlLoader.load(mtlFileName,function(materials){
			var objLoader = new THREE.OBJLoader();
			objLoader.setPath(path);
			objLoader.setMaterials(materials);
			objLoader.load(objFileName,function(object){
				if(typeof success == "function"){
					success(object);
				}
			},onProgress,onError);
		},onProgress,onError);
	}


})(window);

```