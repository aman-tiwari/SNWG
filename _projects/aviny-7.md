---
layout: post
author: Andrew Viny
title: Cube builder
thumbnail: w2_aviny_thumbnail.png
assignment: 7
---
<meta charset=utf-8>
<title>Cube builder</title>

<div style="font-family:'arial'; font-size: 24px"> Cube builder! </div>
<br>
<div style="font-family:'arial'; font-size: 18px"> Move the cube usind the arrow keys (use 'U' for up and 'D' for down in the z axis).  To place a cube press the 's' key and another cube will appear at the origin.  The current cube is the tranlucent yellow one. </div>

<script src="http://threejs.org/build/three.js">
</script>

<script src="../code/aviny/OBJ_loader.js"></script>

<script>
var mouseX = 0, mouseY = 0;
var windowHalfX = window.innerWidth / 2;
var windowHalfY = window.innerHeight / 2;

var container = document.createElement( 'div' );
document.body.appendChild( container );

var scene = new THREE.Scene();
var pcamera = new THREE.PerspectiveCamera( 35, window.innerWidth/600, 1, 1000);
pcamera.position.set(-6,-6,6);
scene.add( pcamera );

// var ambient = new THREE.AmbientLight( 0xffffff );
// scene.add( ambient );
var light = new THREE.DirectionalLight( 0xff0000);
var light2 = new THREE.DirectionalLight( 0x404040);
var light3 = new THREE.DirectionalLight( 0xff0000);
var light4 = new THREE.DirectionalLight( 0xffffff);
// var helper = new THREE.DirectionalLightHelper(light, 5);
light.position.set(0,1,0).normalize();
scene.add(light);
light2.position.set(0,-1,0).normalize();
scene.add(light2);
light3.position.set(-1,0,0).normalize();
scene.add(light3);
light4.position.set(1,0,1).normalize();
scene.add(light4);
// scene.add(helper);
var cubeBumpMaterial1 = new THREE.MeshPhongMaterial({color: 'red'});
var cubeBumpMaterial2 = new THREE.MeshPhongMaterial({color: 'yellow'});

var cube;
var objLoader = new THREE.OBJLoader();
objLoader.load('../code/aviny/models/cube.obj', function (obj) {
	cube = obj;
	obj.traverse(function (child) {
		if (child instanceof THREE.Mesh) {
			child.material = cubeBumpMaterial2;
		}

	});

	scene.add(obj);
	renderer.render(scene, pcamera);
	drawCubes();
});

renderer = new THREE.WebGLRenderer();
// renderer.setPixelRatio( 1 );
renderer.setClearColor(0x000000, 1.0);
renderer.setSize( window.innerWidth, 600 );
container.appendChild( renderer.domElement );


function animate() {
	requestAnimationFrame( animate );
	render();
}

function render() {
	pcamera.position.set(10,10,10);
	pcamera.lookAt( scene.position );;

	renderer.render( scene, pcamera );
}

var cubes;
function drawCubes() {
	cubes = [cube];
}

function onDocumentKeyDown( event ) {
	if (event.key == "ArrowUp") {
		cubes[cubes.length-1].position.x-=1.1;
		event.preventDefault();
	}
	if (event.key == "ArrowDown") {
		cubes[cubes.length-1].position.x+=1.1;
		event.preventDefault();
	}
	if (event.key == "ArrowRight") {
		cubes[cubes.length-1].position.z-=1.1;
		event.preventDefault();
	}
	if (event.key == "ArrowLeft") {
		cubes[cubes.length-1].position.z+=1.1;
		event.preventDefault();
	}
	if (event.key == "d") {
		cubes[cubes.length-1].position.y-=1.1;
		event.preventDefault();
	}
	if (event.key == "u") {
		cubes[cubes.length-1].position.y+=1.1;
		event.preventDefault();
	}
	if (event.key == "s") {
		cubes[cubes.length-1].traverse(function (child) {
			if (child instanceof THREE.Mesh) {
				child.material = cubeBumpMaterial1;
			}

		});
		cubes.push(cube.clone());
		cubes[cubes.length-1].traverse(function (child) {
			if (child instanceof THREE.Mesh) {
				child.material = cubeBumpMaterial2;
			}

		});
		scene.add(cubes[cubes.length-1]);
		event.preventDefault();
	}
}

animate();

document.addEventListener( 'keydown', onDocumentKeyDown, false );

</script>
