<template>
    <div ref="container" class="scene-container"></div>
  </template>
  
  <script>
  import * as THREE from 'three';
  
  export default {
    name: 'CoinScene',
    mounted() {
      this.initScene();
      this.createCoins();
      this.animate();
    },
    methods: {
      initScene() {
        this.scene = new THREE.Scene();
        this.camera = new THREE.PerspectiveCamera(
          75,
          window.innerWidth / window.innerHeight,
          0.1,
          1000
        );
        this.camera.position.z = 5;
  
        this.renderer = new THREE.WebGLRenderer();
        this.renderer.setSize(window.innerWidth, window.innerHeight);
        this.$refs.container.appendChild(this.renderer.domElement);
      },
      createCoins() {
        this.coins = [];
  
        const geometry = new THREE.CylinderGeometry(1, 1, 0.1, 100);
        const material = new THREE.MeshPhongMaterial({ color: 0xffd700 });
  
        
          const coin = new THREE.Mesh(geometry, material);
          coin.position.x = 0;
          coin.rotation.x = Math.PI / 2;
          this.coins.push(coin);
          this.scene.add(coin);
        
  
        const light = new THREE.DirectionalLight(0xffffff, 2.5);
        light.position.set(0, 1, 2);
        this.scene.add(light);
      },
      animate() {
        requestAnimationFrame(this.animate);
        this.coins.forEach((coin, index) => {
          coin.rotation.z += 0.01 * (index + 1);
        });
        this.renderer.render(this.scene, this.camera);
      },
    },
    
  };
  </script>
  
  <style>
  .scene-container {
    width: 100%;
    height: 100vh;
    overflow: hidden;
    position: relative;
  }
  </style>
  