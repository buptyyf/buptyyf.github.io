# 关于光与影

在 Three.js 中，能形成阴影的光源只有  与 THREE.SpotLight ；而
相对地，能表现阴影效果的材质只有 THREE.LambertMaterial 与 THREE.PhongMaterial 。

---

## 一. 加光

### 1. THREE.DirectionalLight(color = "#fff", intensity = 1 )

用于正交照相机计算阴影

> .castShadow = false

设为true表示可以形成阴影。

> .position = (0, 0, 0)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.target = (0, 0, 0)  

光源的位置，光照射的方向为position -> target的矢量方向。如果改变了target的值，则需要把
target加入到scene中：**scene.add(light.target)**

> .shadow&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*所有可以形成阴影的light都会用到这个属性*

- .matrix
- .camera  
- .bais
- .map  
以上几个还没用过，对于功能看文档也没看懂。。。  
- .mapSize(height = 512, width = 512)  
拥有.height和.width两个属性，数值越大表示阴影的精度越高，即阴影的边界约清晰。
- .radius=1  
如果值大于1则会使边界变得模糊，但是值太大就会使边界很难看，可以配合mapSize来使用，这两个属性
成正比。如果 WebGLRenderer.shadowMap.type设为BasicShadowMap，radius属性就没用了。

### 2. THREE.PointLight(color = '#fff', intensity = 1, distance = 0, decay = 1)

decay表示光随着距离的衰退程度，distance设为0表示光永远不衰减

### 3. THREE.SpotLight(color, intensity, distance, angle = Math.PI / 3, penumbra = 0, decay)

angle最大的角度为PI / 2，penumbra什么半影衰减百分比。。没懂，可取值0到1。

## 二. 加接收阴影的平面

其实就是增加一个平面几何体之后，再把这个平面几何的.receiveShadow属性设为true就可以啦

## 三. 使物体成为可呈阴影

对物体设置.castShadow为true即可