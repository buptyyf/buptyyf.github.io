# TrackBallControls学习札记

一. TrackBallControls简介

TrackBallControls其实是一个Three.js中的一个小类库，主要用于
依据三种操作的而进行不同的视觉变化(`Rotate,Zoom,Pan`)，鼠标左键移动表示旋转移动，
鼠标中间的滑轮滚动表示前后伸缩移动，鼠标右键移动表示在所在平面
进行二维移动。

二. 主要属性介绍

在使用TrackBallControls之前需要先用下面这句代码来创建一个控制对象
```javascript
let controls = new THREE.TrackballControls( camera );
```

**rotateSpeed**：顾名思义，旋转移动的速度，默认为1.0，数值越大旋转的越快

**zoomSpeed**：顾名思义，前后伸缩的速度，默认为1.2，数值越大伸缩的越快，但是实验中效果不是很理想，
总是向前到一定距离就会不动弹，向后的话没有什么影响，具体还看源码深究

**panSpeed**：顾名思义，平面移动的速度，默认为0.3，数值越大平面移动的越快

**noRotate**：顾名思义，是否禁止旋转，默认为false

**noZoom**：顾名思义，是否禁止前后伸缩，默认为false

**noPan**：顾名思义，是否禁止平面移动，默认为false

**staticMoving**：是否纯静态移动，鼠标停就停，没有缓动。默认为false，有缓动

**dynamicDampingFactor**：和三中变化都有关
> 对rotate的影响：
> ```javascript
> else if ( !_this.staticMoving && _lastAngle ) {
>    _lastAngle *= Math.sqrt( 1.0 - _this.dynamicDampingFactor );
>    _eye.copy( _this.object.position ).sub( _this.target );
>    quaternion.setFromAxisAngle( _lastAxis, _lastAngle );
>    _eye.applyQuaternion( quaternion );
>    _this.object.up.applyQuaternion( quaternion );
> }
> ```
> 对zoom的影响：
> ```javascript
> if ( !_this.staticMoving ) {
>    _zoomStart.y += ( _zoomEnd.y - _zoomStart.y ) * this.dynamicDampingFactor;
> }
> ```
> 对pan的影响：
> ```javascript
> if ( !_this.staticMoving ) {
>    _panStart.add( mouseChange.subVectors( _panEnd, _panStart ).multiplyScalar( _this.dynamicDampingFactor ) );
> }
> ```

三. 主要方法介绍

**update()**：用于页面在三种操作后的更新。

**handleResize()**：用于在浏览器窗口大小变化后进行重新的调整。

**dispose()**：用于页面卸载后解除监听的操作。
