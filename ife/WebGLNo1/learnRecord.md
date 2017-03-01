
# 初学 three.js 的一点记录和理解

一. 有三个关键概念：
* scene 

> 使用 new THREE.Scene() 进行创建，相当于一个大容器，一般情况下场景没有很复杂的操作，最开始实例化，之后把物体添加进场景

* camera

> 使用 new THREE.PerspectiveCamera(FeildView, width/height, near, far) 用于创建一个透明的右手坐标系，
有camera.position.set(x, y, z)方法来实现位置的移动，如果只针对某一坐标系来移动，可以使用position.x()
或postion.y()或position.z()来实现

* renderer

> 使用 new THREE.WebGLRenderer() 进行创建一个canvas，有setSize(width, height)方法用于控制画布的大小

---

二. 创建一个立方体：

创建一个立方体需要三个重要的部件：

* geometry

> 可使用 new THREE.BoxGeometry( 1, 1, 1 ) 来创建一个边长为1的正方体，其实这个方法可以控制立方体所有的
点和面，还有更多用法还待研究

* material

> 使用 new THREE.MeshBasicMaterial( style: object )， three.js还有许多的material，我们现在先介绍MeshBasicMaterial

* mesh

> 使用 new THREE.Mesh(geometry, material) 来创建一个mesh，需要一个几何体(geometry)和覆盖它的材料(material)

到这里我们创建完成了一个简单的几何体，但还没有呈现的地方，此时需要下面这个操作
```javascript 
scene.add(mesh) 
```
的方法来把这个几何体放在scene上，其默认的
在camrea上的位置为(0, 0, 0)。

这个时候我们把几何体加入了场景中，但是还是没有展示出来啊，毕竟还没有渲染嘛。那就进行渲染吧！
```javascript
renderer.render(scene, camera)
```
好啦，已经可以在页面上看见一个方块了。

啊？我创建的可是正方体啊，为啥只是个正方形出来了。怎么才能让他显得有立体感啊，那就把它转一转吧！
```javascript
mesh.rotation.x += 1;
mesh.rotation.y += 1;
```
嗯，看起来是有点立体感了。。那是让它旋转起来多好看啊。好吧，就依你说，让他旋转！

如果要让立方体连续并且丝滑的转动起来，就得requestAnimationFrame()出场了
```javascript
var render = function () {
    cube.rotation.x += .1;
    cube.rotation.y += .1;
    cube.rotation.z += .1;

    renderer.render(scene, camera);
    requestAnimationFrame( render );
};

render();
```