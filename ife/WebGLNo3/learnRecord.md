# 材质和纹理

## 一. 材质

### 1. THREE.MeshBasicMaterial()

基本材质，无立体感，下面介绍几个常用参数
> visible = true

是否可见，默认为可见，一般不更改

> wireframe

是否渲染线而非面，默认为 false

> color

十六进制 RGB 颜色，如红色表示为 0xff0000

> map 

使用纹理贴图，见下文

### 2. THREE.MeshLambertMaterial()

只考虑漫反射，不考虑镜面反射，不适用于金属等有较强反射性质的材质。

> emissive 

是材质的自发光颜色，可以用来表现光源的颜色。

### 3. THREE.MeshPhongMaterial()

Phong 模型考虑了镜面反射的效果，因此对于金属、镜面的表现尤为适合。由于漫反射部分与
 Lambert 模型是一致的，因此，如果不指定镜面反射系数，而只设定漫
反射，其效果与 Lambert 是相同的。  
可以指定 emissive 值。

> specular = 0x111111 （灰色）

高光的颜色，即光照射在物体上，物体上所显示的光斑的颜色。

> shininess = 30

光照射在物体上，光斑的大小，数值越大，光斑越小。

### 4. THREE.MultiMaterial(Materials)

对物体每个面的材料进行设定，需在构造的时候传入一存有正确数量Material的数组，个下面以立方体为例
```javascript
var materials = [

    new THREE.MeshBasicMaterial( { color: 0xff0000 } ), // right
    new THREE.MeshBasicMaterial( { color: 0x0000ff } ), // left
    new THREE.MeshBasicMaterial( { color: 0x00ff00 } ), // top
    new THREE.MeshBasicMaterial( { color: 0xffff00 } ), // bottom
    new THREE.MeshBasicMaterial( { color: 0x00ffff } ), // back
    new THREE.MeshBasicMaterial( { color: 0xff00ff } )  // front

];

var cubeSidesMaterial = new THREE.MultiMaterial( materials );
```

## 二. 纹理

纹理其实就是给物体的表面贴图。主要会用到Material中的map属性。  
下面给出两个典型的例子：

1. 给六个面加同一张图
```javascript
let loader = new THREE.TextureLoader();
let texture = loader.load("./assets/1.jpg",function(){
    renderer.render( scene, camera );})
console.log(texture)
let material = new THREE.MeshLambertMaterial({
    map: texture,
});
console.log(material)
let cube = new THREE.Mesh(new THREE.CubeGeometry(3, 3, 3),material);
console.log(cube)
cube.position.set(4,3,0);
scene.add(cube)
```

2. 给六个面加六张图
```javascript
let loader = new THREE.TextureLoader()
let materials = [];
for (var i = 0; i < 6; ++i) {
    materials.push(new THREE.MeshPhongMaterial({
        map: loader.load('./assets/' + i + '.jpg', function(){
            renderer.render( scene, camera );
        }),
        specular: 0xff0000,
        shininess: 1000,
    }));
}
let cube = new THREE.Mesh(new THREE.CubeGeometry(3, 3, 3),
    new THREE.MultiMaterial(materials));
cube.position.set(4,3,0);
console.log(cube)
cube.castShadow = true;
scene.add(cube);
```

因为要进行贴图，这两个例子都用到了**TextureLoader()**，这是纹理中一个重要的类，可用于加载图片并在加载完成后渲染。
这个类有个重要的方法

> load(url, onLoad, onProgress, onError)

**url**是资源的地址，**onload**是在加载完成后的回调函数，一般renderer.render写在这个回调中，否则，加载的资源不
会渲染在scene中。其他两个也都是相应状态的回调函数，一个是在加载过程中，另一个是加载失败后。

**note：**  
*在加载图片的时候，如果直接打开文件夹中的html文件进行浏览，会出现跨域的问题，那要怎么解决呢？*

最快捷方法就是使用一个简单的web server。通过网上了解了以下几个，搬运过来：

1. python -m SimpleHTTPServer  

将在你所在目录下，建立一个http server，之后访问http://localhost:8000 就可以轻松解决跨域问题啦~

2. python3 -m http.server

如果是python3的话，可以安装上面这个http server，同样访问http://localhost:8000 即可。

3. npm install -g http-server

如果你是node用户的话，试试这个把，轻松加愉快，安装到html所在目录下，输入http-server即可，还有很多参数，去
github上看吧，开启之后访问http://localhost:8080吧。

4. ruby -run -e httpd . -p 8080

ruby用户看上面。。。

5. php -S localhost:8000

怎么能少了世界上最好的语言php呢，话不多说，执行就好。