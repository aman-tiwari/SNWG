

---
author: Andrew Viny
title: Tree viewer
thumbnail: w4_aviny_thumbnail.png
---

<title>Tree viewer</title>
<style>
	body { margin: 0; }
	canvas { width: 100%; height: 100% }
</style>

<div style="font-family:'arial'; font-size: 18px"> This is Tree viewer.  It uses a simple orthographic camera to view this nicely textured tree.  The camera moves in response to the mouse but movement is limited.  Here I'm taking the same model I posted to SketchFab but I'm rendering it directly in THREE.js.  Over all I think this works pretty well but the controls are not as expansive.</div>

<script src="http://threejs.org/build/three.js">
</script>

<script src="../code/aviny//OBJ_loader.js"></script>

<div id="canvas"></div>

<script>
var mouseX = 0, mouseY = 0;
var windowHalfX = window.innerWidth / 2;
var windowHalfY = window.innerHeight / 2;

var container = document.getElementById("canvas");

var scene = new THREE.Scene();
var camera = new THREE.OrthographicCamera( window.innerWidth / - 2, window.innerWidth / 2, 700 / 2, 700 / - 2, 1, 1000 );
scene.add( camera );
camera.position.z = 600;
camera.position.x = 0;

var ambient = new THREE.AmbientLight( 0x101030 );
				scene.add( ambient );
var directionalLight = new THREE.DirectionalLight( 0xffeedd );
				directionalLight.position.set( 0, 0, 1 );
				scene.add( directionalLight );

var manager = new THREE.LoadingManager();
				manager.onProgress = function ( item, loaded, total ) {
					console.log( item, loaded, total );
				};

var texture = new THREE.Texture();

var loader = new THREE.ImageLoader( manager );
				loader.load( '../code/aviny/textures/t0.jpg', function ( image ) {
					texture.image = image;
					texture.needsUpdate = true;
				} );



var renderer = new THREE.WebGLRenderer();
renderer.setSize( window.innerWidth, window.innerHeight );

var cubeBumpMaterial = new THREE.MeshPhongMaterial();

var objLoader = new THREE.OBJLoader();
objLoader.load('../code/aviny/models/THING.obj', function (obj) {
    obj.traverse(function (child) {

        if (child instanceof THREE.Mesh) {
        	child.material = cubeBumpMaterial;
            child.material.map = texture;
            child.material.bumpMap = texture;
            child.material.bumpScale = 12;
        }

    });

    obj.rotation.x = Math.PI/180*270;
    obj.position.y = -80;

    obj.scale.x = 2;
    obj.scale.y = 2;
    obj.scale.z = 2;

    console.log(obj);
    scene.add(obj);
});

renderer = new THREE.WebGLRenderer();
renderer.setPixelRatio( window.devicePixelRatio );
renderer.setSize( window.innerWidth, 700 );
container.appendChild( renderer.domElement );

document.addEventListener( 'mousemove', onDocumentMouseMove, false );

function onDocumentMouseMove( event ) {
				mouseX = ( event.clientX - windowHalfX ) / 2;
				mouseY = ( event.clientY - windowHalfY ) / 2;
			}

function animate() {
				requestAnimationFrame( animate );
				render();
			}

function render() {

				camera.position.x += ( mouseX - camera.position.x ) * .2;
				camera.position.y += ( - mouseY - camera.position.y ) * .2;

				camera.lookAt( scene.position );
				renderer.render( scene, camera );
			}

animate();

document.addEventListener( 'mousemove', onDocumentMouseMove, false );

</script>
