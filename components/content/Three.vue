<template>
  <div ref="threeCanvas" style="width: 100%; height: 100%;"></div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import * as THREE from 'three';
import { RectAreaLightHelper } from 'three/examples/jsm/helpers/RectAreaLightHelper';

// Variabili reattive per la gestione del DOM
const threeCanvas = ref(null);

let scene, camera, renderer;
let sceneGruop, particularGruop, modularGruop;
let INTERSECTED = null;
let cameraValue = false;
const mouse = new THREE.Vector2();
const raycaster = new THREE.Raycaster();
let rectLight = new THREE.RectAreaLight(0x2A2D34, 200, 5, 5)

const colorMode = useColorMode(); // Per gestire il tema

updateLightColor(colorMode.preference);

// Funzione per generare particelle
function generateParticle(num, amp = 2) {
  const gmaterial = new THREE.MeshPhysicalMaterial({
    color: 0xffffff,
    side: THREE.DoubleSide,
  });

  const gparticular = new THREE.CircleGeometry(0.2, 5);

  for (let i = 1; i < num; i++) {
    const pscale = 0.001 + Math.abs(mathRandom(0.03));
    const particular = new THREE.Mesh(gparticular, gmaterial);
    particular.position.set(mathRandom(amp), mathRandom(amp), mathRandom(amp));
    particular.rotation.set(mathRandom(), mathRandom(), mathRandom());
    particular.scale.set(pscale, pscale, pscale);
    particular.speedValue = mathRandom(1);

    particularGruop.add(particular);
  }
}

function mathRandom(num = 1) {
  const setNumber = -Math.random() * num + Math.random() * num;
  return setNumber;
}

function init() {
  for (let i = 0; i < 30; i++) {
    const geometry = new THREE.IcosahedronGeometry(1);
    const material = new THREE.MeshStandardMaterial({
      color: 0x111111,
      transparent: false,
      opacity: 1,
      wireframe: false,
    });
    const cube = new THREE.Mesh(geometry, material);
    cube.speedRotation = Math.random() * 0.1;
    cube.positionX = mathRandom();
    cube.positionY = mathRandom();
    cube.positionZ = mathRandom();
    cube.castShadow = true;
    cube.receiveShadow = true;

    const newScaleValue = mathRandom(0.3);
    cube.scale.set(newScaleValue, newScaleValue, newScaleValue);
    cube.rotation.set(mathRandom(180 * Math.PI / 180), mathRandom(180 * Math.PI / 180), mathRandom(180 * Math.PI / 180));
    cube.position.set(cube.positionX, cube.positionY, cube.positionZ);
    modularGruop.add(cube);
  }
}

function onMouseMove(event) {
  event.preventDefault();
  mouse.x = (event.clientX / window.innerWidth) * 0.5 - 1;
  mouse.y = -(event.clientY / window.innerHeight) * 0.5 + 1;
}

// Funzione di animazione
function animate() {
  const time = performance.now() * 0.0003;
  requestAnimationFrame(animate);

  // Aggiorna rotazioni particelle
  particularGruop.children.forEach((child) => {
    child.rotation.x += child.speedValue / 100;
    child.rotation.y += child.speedValue / 100;
    child.rotation.z += child.speedValue / 100;
  });

  // Aggiorna rotazioni cubi
  modularGruop.children.forEach((cube) => {
    cube.rotation.x += 0.0008;
    cube.rotation.y += 0.0005;
    cube.rotation.z += 0.0003;
    cube.position.x = Math.sin(time * cube.positionZ) * cube.positionY;
    cube.position.y = Math.cos(time * cube.positionX) * cube.positionZ;
    cube.position.z = Math.sin(time * cube.positionY) * cube.positionX;
  });

  particularGruop.rotation.y += 0.005;
  modularGruop.rotation.y -= (mouse.x * 4 + modularGruop.rotation.y) * 0.1;
  modularGruop.rotation.x -= (-mouse.y * 4 + modularGruop.rotation.x) * 0.1;

  camera.lookAt(scene.position);
  renderer.render(scene, camera);
}

// Funzione per aggiornare il colore della luce in base al tema
function updateLightColor(theme) {
  const darkThemeColor = 0x111111; // Colore della luce per tema dark
  const lightThemeColor = 0xFFFFFF; // Colore della luce per tema light

  if (rectLight) {
    rectLight.color.setHex(theme === 'dark' ? darkThemeColor : lightThemeColor);
  }
}

// Guarda il cambiamento del tema e aggiorna la luce
watch(colorMode, (newTheme) => {
  console.log(newTheme)
  updateLightColor(newTheme.preference);
});


// Inizializzazione della scena
onMounted(() => {
  if (import.meta.client) {
    renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    threeCanvas.value.appendChild(renderer.domElement);

    camera = new THREE.PerspectiveCamera(35, window.innerWidth / window.innerHeight, 1, 500);
    camera.position.set(0, 0, 3);
    scene = new THREE.Scene();
    // scene.background = new THREE.Color(0x000000);
    // scene.fog = new THREE.Fog(0x000000, 2.5, 3.5);

    sceneGruop = new THREE.Object3D();
    particularGruop = new THREE.Object3D();
    modularGruop = new THREE.Object3D();

    generateParticle(200, 2);
    sceneGruop.add(particularGruop);
    scene.add(modularGruop);
    scene.add(sceneGruop);

    const light = new THREE.SpotLight(0xffffff, 3);
    light.position.set(5, 5, 2);
    light.castShadow = true;
    scene.add(light);

    const lightBack = new THREE.PointLight(0xffffff, 1);
    lightBack.position.set(0, -3, -1);
    scene.add(lightBack);

    rectLight.position.set(0, 0, 1);
    rectLight.lookAt(0, 0, 0);
    scene.add(rectLight);

    const rectLightHelper = new RectAreaLightHelper(rectLight);
    scene.add(rectLightHelper);

    // Eventi mouse e resize
    window.addEventListener('mousemove', onMouseMove, false);
    window.addEventListener('resize', () => {
      camera.aspect = window.innerWidth / window.innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(window.innerWidth, window.innerHeight);
    });

    init();
    animate();
  }
});
</script>

<style scoped>
div {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
</style>
