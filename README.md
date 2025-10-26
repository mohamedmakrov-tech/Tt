# Tt
Hh
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>3D Shooter V2 - Three.js</title>
<style>
  body { margin: 0; overflow: hidden; }
  #ui {
    position: absolute; top: 10px; left: 10px; color: white;
    font-family: Arial; font-size: 20px;
  }
</style>
</head>
<body>
<div id="ui">Health: 100 | Score: 0 | Weapon: Rifle</div>
<script src="https://cdn.jsdelivr.net/npm/three@0.155.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.155.0/examples/js/controls/OrbitControls.js"></script>

<script>
// ====== Scene ======
const scene = new THREE.Scene();
scene.background = new THREE.Color(0x87CEEB);
const camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
camera.position.set(0, 5, 10);
const renderer = new THREE.WebGLRenderer({antialias:true});
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// ====== Controls ======
const controls = new THREE.OrbitControls(camera, renderer.domElement);
controls.target.set(0, 2, 0);
controls.update();

// ====== Lights ======
const ambientLight = new THREE.AmbientLight(0xffffff,0.6); scene.add(ambientLight);
const dirLight = new THREE.DirectionalLight(0xffffff,0.8); dirLight.position.set(10,20,10); scene.add(dirLight);

// ====== Floor ======
const floor = new THREE.Mesh(new THREE.PlaneGeometry(50,50), new THREE.MeshStandardMaterial({color:0x228B22}));
floor.rotation.x = -Math.PI/2; scene.add(floor);

// ====== House ======
const house = new THREE.Mesh(new THREE.BoxGeometry(6,4,6), new THREE.MeshStandardMaterial({color:0x8B4513}));
house.position.set(0,2,0); scene.add(house);

// ====== Player ======
const player = new THREE.Mesh(new THREE.BoxGeometry(1,2,1), new THREE.MeshStandardMaterial({color:0x0000ff}));
player.position.set(0,3,0); scene.add(player);
let playerHealth = 100;
let score = 0;

// ====== Weapons ======
let weapon = "Rifle"; // placeholder for multiple weapons

// ====== Enemies ======
const enemies = [];
function spawnEnemy(){
    const enemy = new THREE.Mesh(new THREE.BoxGeometry(
