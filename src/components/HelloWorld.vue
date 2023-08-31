<template>
  <div ref="container" class="coin-scene"></div>
  <div class="scene-container">
    
    <button @click="flipCoin" class="flip-button" >Flip Coin</button>
    <div class="coin-result" v-if="result !== null">
      <h1>{{ result }}</h1>
    </div>
   
  </div>
</template>

<script>
import * as THREE from 'three';
import heads from '../assets/heads.png';
import tails from '../assets/tails.png';

export default {
  
  data() {
    return {
      result: null,
      flipping: false,
      
    };
  },
  mounted() {
    this.initScene();
    this.createCoins();
    this.animate();
  },
  methods: {
    initScene() {
      this.scene = new THREE.Scene();

      // Camera
      this.camera = new THREE.PerspectiveCamera(
        60,
        window.innerWidth / window.innerHeight,
        0.5,
        1000
      );
      this.camera.position.z = 20;

      this.renderer = new THREE.WebGLRenderer();
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.$refs.container.appendChild(this.renderer.domElement);
    },
    createCoins() {
      this.coins = [];

      const geometry = new THREE.CylinderGeometry(5, 5, 0.5,20,40, false);
      

       const textureLoader = new THREE.TextureLoader()
       const face1Texture = textureLoader.load(heads)
        const face1Material = new THREE.MeshLambertMaterial({map:face1Texture})

        // Face 2
        const face2Texture = textureLoader.load(tails)
        const face2Material = new THREE.MeshLambertMaterial({map:face2Texture})

        const whiteMaterial = new THREE.MeshStandardMaterial({color: 0xffffff})
        const material = [
      whiteMaterial, // Coin edge
      face1Material, // Coin face 1
     face2Material,
    ];

        const coin = new THREE.Mesh(geometry, material);
        coin.position.x = 0;
        coin.rotation.x = (Math.PI / 2);
        this.coins.push(coin);
        this.scene.add(coin);
      

        const pointLight = new THREE.PointLight(0xffffff, 2)
        pointLight.position.set(5, -28.5,5)

        const pointLight2 = new THREE.PointLight(0xffffff, 2)
        pointLight2.position.set(5, 5, 5)

        const ambientLight = new THREE.AmbientLight( 0xffffff, 2 )
        const directionalLight = new THREE.DirectionalLight( 0xffffff, 2 )

        this.scene.add(pointLight,pointLight2, ambientLight, directionalLight)
      
    },
    animate() {
      requestAnimationFrame(this.animate);
      this.coins.forEach((coin, index) => {
        coin.rotation.x += 0.02 * (index + 1);
        //coin.rotation.x = this.coinRotation; // Apply coin rotation
      });
      
      this.renderer.render(this.scene, this.camera);
    },
    flipCoin() {
      if (this.flipping) return;

      this.flipping = true;
      this.result = null;

      setTimeout(() => {
        const randomResult = Math.random() < 0.5 ? 'Heads' : 'Tails';
        this.result = randomResult;
        this.coins.forEach((coin) => {
          coin.rotation.x= this.result==="Tails" ? -(Math.PI / 2):(Math.PI / 2);
          });
        
        this.flipping = false;
      }, 1000); // Simulate the coin flip duration

      this.animateCoinFlip();
    },
    animateCoinFlip() {
      const duration = 1000; // Same duration as setTimeout in flipCoin
      const startTime = Date.now();
      const rotationSpeed = 0.01;

      const animateRotation = () => {
        const elapsedTime = Date.now() - startTime;
        if (elapsedTime >= duration) return;

        this.coins.forEach((coin, index) => {
          coin.rotation.x = Math.sin(rotationSpeed * elapsedTime * (index + 1));
        });

        requestAnimationFrame(animateRotation);
      };

      animateRotation();
    },
  },
 
};
</script>

<style>
.scene-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: #f2f2f2;
}
.flip-button {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}
.flip-button:hover {
  background-color: #0056b3;
}
.coin-result {
  margin-top: 20px;
}



.coin-scene {
  width: 100%;
  height: 50vh;
}

</style>
