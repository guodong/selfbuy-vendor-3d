<template>
  <div id="app" class="main">
    <div class="info">
      <div class="page-header">
        <h3>可比克薯片 <small>原味</small></h3>
      </div>
      <div class="field">
        <label>品牌：</label>
        <span>达利食品</span>
      </div>
      <div class="field">
        <label>净含量：</label>
        <span>100g</span>
      </div>
      <div class="field">
        <label>产 地：</label>
        <span>上海</span>
      </div>
      <div class="field">
        <label>生产日期：</label>
        <span>2017-03-01</span>
      </div>
      <div class="field">
        <label>保质期：</label>
        <span>12个月</span>
      </div>
      <div class="field">
        <label>价格：</label>
        <span>¥6.60</span>
      </div>
      <div class="tool">
        <label>购买数量：</label>
        <button class="btn btn-default btn-sm">-</button>
        <input class="buynum" value="1">
        <button class="btn btn-default btn-sm">+</button>
        <br>
        <button class="btn btn-success btn-sm addcart"><i class="glyphicon glyphicon-shopping-cart"></i> 加入购物篮</button>
      </div>
    </div>
    <div class="cart">
      <img class="carticon" src="/static/cart.png" style="height: 80px;">
      <div class="cartinner">
        <ul>
          <li><img src="/static/kebike.jpg" style="height: 60px;width: 60px;"> 可比克薯片 x 1</li>
        </ul>
      </div>
      <div class="checkout">
      <span class="" style="font-size: 20px;">共计：¥6.60</span>
      <button id="checkout" class="btn btn-warning btn-lg"><i class="glyphicon glyphicon-qrcode"></i> 扫码支付</button>
      </div>
    </div>
    <div class="rqcode">
      <h1>请用支付宝或微信扫码支付</h1>
      <img src="/static/rqcode.jpg">
      <div class="brand">
        <img src="/static/brand.jpeg" style="height: 40px; margin-top: 20px;">
      </div>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}
var scene = new THREE.Scene();
var camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight);

var renderer = new THREE.WebGLRenderer({antialias: true});
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setClearColor(0x000000, 1);
setTimeout(function() {
  document.getElementById('app').appendChild(renderer.domElement);
}, 1000)

var orbit = new THREE.OrbitControls(camera, renderer.domElement);
//orbit.enableZoom = false;
var texture = THREE.ImageUtils.loadTexture('/static/muban1.jpeg');
var material = new THREE.MeshPhongMaterial();
material.map = texture;//材质的Map属性用于添加纹理

var planes = [];
for (var i = 0; i < 4; i++) {
  var geometry = new THREE.BoxGeometry(1800, 20, 240);
  var plane = new THREE.Mesh(geometry, material);
  plane.position.y = 500 - i * 300;
  planes.push(plane);
  scene.add(plane)
}

var directionalLight = new THREE.DirectionalLight(0xffffff, 0.5);
scene.add(directionalLight);
var light = new THREE.AmbientLight(0xcccccc); // soft white light
scene.add(light);

var products = [];

for (var i = 0; i < 8; i++) {
  var geometry = new THREE.CylinderGeometry(30, 30, 120, 99);
  var texture = THREE.ImageUtils.loadTexture('/static/coco.jpg');
  var mat = new THREE.MeshPhongMaterial();
  mat.map = texture;//材质的Map属性用于添加纹理
  var cylinder = new THREE.Mesh(geometry, mat);
  cylinder.position.y = planes[2].position.y + geometry.parameters.height / 2 + 10;
  cylinder.position.x = -800 + i * 220;
  scene.add(cylinder);
  products.push(cylinder);
}

for (var i = 0; i < 8; i++) {
  var geometry = new THREE.CylinderGeometry(30, 30, 200, 99);
  var texture = THREE.ImageUtils.loadTexture('/static/kebike.jpg');
  var mat = new THREE.MeshPhongMaterial();
  mat.map = texture;//材质的Map属性用于添加纹理
  var cylinder1 = new THREE.Mesh(geometry, mat);
  cylinder1.position.y = planes[1].position.y + geometry.parameters.height / 2 + 10;
  cylinder1.position.x = -800 + i * 220;
  scene.add(cylinder1);
  products.push(cylinder1);
}


var geometry = new THREE.BoxGeometry(80, 40, 2);
var meterial = new THREE.LineBasicMaterial({
  color: 0xffffff,
  transparent: true,
  opacity: 0.5
});

var loader = new THREE.FontLoader();

loader.load('https://raw.githubusercontent.com/mrdoob/three.js/master/examples/fonts/helvetiker_regular.typeface.json', function(font) {
  var mesh = new THREE.Object3D();
  var geometry = new THREE.TextGeometry('¥6.60', {
    font: font,
    size: 20,
    height: 1,
    curveSegments: 20,
    bevelEnabled: false,
    bevelThickness: 1,
    bevelSize: 1,
    bevelSegments: 5
  });
  var material = new THREE.LineBasicMaterial({
    color: 0x000000,
    transparent: true,
    opacity: 0.9
  });
  for (var i = 0; i < 8; i++) {
    var tag = new THREE.Mesh(geometry, material);
    tag.position.y = planes[1].position.y - 8;
    tag.position.z = 120;
    tag.position.x = -800 + i * 220 -40;
    mesh.add(tag);
    scene.add(mesh);
  }
  var geometry2 = new THREE.TextGeometry('¥2.50', {
    font: font,
    size: 20,
    height: 1,
    curveSegments: 20,
    bevelEnabled: false,
    bevelThickness: 1,
    bevelSize: 1,
    bevelSegments: 5
  });
  for (var i = 0; i < 8; i++) {
    var tag = new THREE.Mesh(geometry2, material);
    tag.position.y = planes[2].position.y - 8;
    tag.position.z = 120;
    tag.position.x = -800 + i * 220 -40;
    mesh.add(tag);
    scene.add(mesh);
  }
});
var manager = new THREE.LoadingManager();
manager.onProgress = function ( item, loaded, total ) {

  console.log( item, loaded, total );

};
var loader = new THREE.OBJLoader(manager);
loader.load( '/static/untitled.obj', function ( object ) {

  object.traverse( function ( child ) {

    if ( child instanceof THREE.Mesh ) {

      //child.material.map = texture;

    }

  } );
//    object.scale.x =  object.scale.y =  object.scale.z = 100;
  object.updateMatrix();
//    object.position.z = 500;
  console.log(scene.children.length);
  scene.add( object );
  console.log(scene.children.length);

}, function(){}, function() {

} );

camera.position.set(0, 250, 800);
camera.up = new THREE.Vector3(0, 1, 0);
camera.lookAt(new THREE.Vector3(0, 0, 0));


window.addEventListener('resize', function() {

  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();

  renderer.setSize(window.innerWidth, window.innerHeight);

}, false);
var raycaster = new THREE.Raycaster();
var mouse = new THREE.Vector2();

function onMouseMove(event) {

  // calculate mouse position in normalized device coordinates
  // (-1 to +1) for both components

  mouse.x = ( event.clientX / window.innerWidth ) * 2 - 1;
  mouse.y = -( event.clientY / window.innerHeight ) * 2 + 1;

}

var INTERSECTED;

function render() {
  products.forEach(function(product) {
    product.rotation.y += 0.01;
  });


  // update the picking ray with the camera and mouse position
  raycaster.setFromCamera(mouse, camera);
  var intersects = raycaster.intersectObjects(scene.children, true);
  if (intersects.length > 0) {
    if (INTERSECTED != intersects[0].object) {
      INTERSECTED = intersects[0].object;
//        INTERSECTED.currentHex = INTERSECTED.material.emissive.getHex();
    }
  } else {
    if (INTERSECTED) {
//        INTERSECTED.position.z = 0;
    }
    INTERSECTED = null;
  }

  renderer.render(scene, camera);

}

function initPosition() {
  for (var i in products) {
    var product = products[i];
    if (!product.ori_position) {
      continue;
    }
    product.position.x = product.ori_position.x;
    product.position.y = product.ori_position.y;
    product.position.z = product.ori_position.z;
  }
}

function onMouseClick(event) {

  if (!INTERSECTED) {
    initPosition();
    $(".info").animate({width:'hide'}, 350);
    return;
  }

  var isProduct = false;
  for (var i in products) {
    if (INTERSECTED === products[i]) {
      isProduct = true;
      break;
    }
  }
  if (!isProduct) {
    return;
  }

  INTERSECTED.ori_position = {
    x: INTERSECTED.position.x,
    y: INTERSECTED.position.y,
    z: INTERSECTED.position.z
  };

  INTERSECTED.position.set(0, 200, 600);
  $(".info").animate({width:'show'}, 350);
}

window.addEventListener('mousemove', onMouseMove, false);
window.addEventListener('mousedown', onMouseClick, false);

function animate() {
  requestAnimationFrame(animate);
  render();
}
animate();
setTimeout(function() {
  $('.carticon').click(function() {
    $('.cartinner').animate({height:'toggle'}, 350);
  });
  $('#checkout').click(function() {
    $('.rqcode').toggle();
  })
}, 1000)

</script>

<style>
  .main {
    height: 100%;
  }
  .info {
    background-color: rgba(255,255,255,0.9);
    height: 100%;
    width: 300px;
    position: fixed;
    right: 0;
    padding: 20px;
    display: none;
  }
  label {
    width: 70px;
  }
  .tool {
    position: absolute;
    padding-bottom: 50px;
    bottom: 0;
    width: 260px;
  }
  .buynum {
    width: 40px;
    text-align: center;
    border: 1px solid #ccc;
    height: 29px;
  }
  .addcart {
    width: 100%;
    margin-top: 20px;
  }
  .cart {
    position: absolute;
    bottom: 0;
    width: 100%;
    background-color: rgba(255,255,255,0.9);
    padding-left: 20px;
  }
  .cartinner {
    height: 100px;
    padding: 20px;
    display: none;
  }
  li {
    list-style: none;
  }
  ul, li {
    margin: 0;
    padding: 0;
  }
  ul {
    margin-left: 10px;
  }
  li {
    float: left;
  }
  .carticon {
    position: absolute;
    top: -70px;
    left: 30px;
  }
  .checkout {
    position: absolute;
    right: 20px;
    top: 30px;
  }
  .rqcode {
    position: fixed;
    display: none;
    width: 500px;
    height: 450px;
    margin: 5% auto; /* Will not center vertically and won't work in IE6/7. */
    left: 0;
    right: 0;
    background-color: rgba(255,255,255,0.9);
    text-align: center;
    -webkit-border-radius:;
    -moz-border-radius:;
    border-radius: 15px;
  }
</style>
