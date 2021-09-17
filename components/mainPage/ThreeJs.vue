<template>
  <div>
    <div id="container"></div>
    <Map />
    <client-only>
      <dat-gui
        closeText="Close controls"
        openText="Open controls"
        closePosition="bottom"
      >
        <dat-folder label="Hart position">
          <dat-number
            v-model="meshPoint.x"
            :min="-600"
            :max="600"
            :step="1"
            label="Mesh Point X"
          />
          <dat-number
            v-model="meshPoint.y"
            :min="-600"
            :max="600"
            :step="1"
            label="Mesh Point Y"
          />
          <dat-number
            v-model="meshPoint.z"
            :min="-600"
            :max="600"
            :step="1"
            label="Mesh Point Z"
          />
        </dat-folder>
      </dat-gui>
    </client-only>
  </div>
</template>
<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
import Map from "./modules/map.vue";

export default {
  name: "ThreeTest",
  components: {
    Map,
  },
  data() {
    return {
      camera: null,
      scene: null,
      renderer: null,
      isUserInteracting: false,
      onPointerDownMouseX: 0,
      onPointerDownMouseY: 0,
      lon: 0,
      onPointerDownLon: 0,
      lat: 0,
      onPointerDownLat: 0,
      phi: 0,
      theta: 0,
      groupPointsInterest: null,
      arrow1: null,
      move: 0,
      meshPoint: {
        x: 0,
        y: 0,
        z: 0,
      },
    };
  },
  mounted() {
    this.init();
    this.animate();
  },
  methods: {
    init() {
      const container = document.getElementById("container");

      this.camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        1,
        1100
      );

      this.scene = new THREE.Scene();

      const gltfLoader = new GLTFLoader();
      const fontLoader = new THREE.FontLoader();

      /**
       * Lights
       */
      const ambientLight = new THREE.HemisphereLight(0xff0000, 0x2b2b2a, 1);
      this.scene.add(ambientLight);

      const geometry = new THREE.SphereGeometry(600, 60, 40);
      // invert the geometry on the x-axis so that all of the faces point inward
      geometry.scale(-1, 1, 1);

      const texture = new THREE.TextureLoader().load("threeJs/garden.JPG");
      const material = new THREE.MeshBasicMaterial({ map: texture });

      const mesh = new THREE.Mesh(geometry, material);

      this.scene.add(mesh);

      /**
       * Texts
       */

      const textMaterial = new THREE.MeshPhongMaterial({
        color: 0xffffff,
        specular: 0xffffff,
        emissive: 0xb2a5a5,
      });

      const textPoints = [["Rosen Garten", -8, -18, 290]];

      for (const points of textPoints) {
        fontLoader.load("fontJson/Montserrat_Regular.json", (font) => {
          const textGeometry = new THREE.TextGeometry(points[0], {
            font: font,

            size: 16,
            height: 2,
            curveSegments: 12,

            bevelThickness: 1,
            bevelSize: 1,
            bevelEnabled: true,
          });

          textGeometry.center();

          const mesh = new THREE.Mesh(textGeometry, textMaterial);
          mesh.position.set(points[1], points[2], points[3]);
          mesh.name = "textLau";
          this.groupPointsInterest.add(mesh);
        });
      }

      /**
       * Points on Scene (Heart)
       */

      this.groupPointsInterest = new THREE.Group();
      this.scene.add(this.groupPointsInterest);

      const arrPoints = [[-8, -43, 290]];

      for (const point of arrPoints) {
        gltfLoader.load("threeJs/heart-point.gltf", (gltf) => {
          let obj = gltf.scene;
          obj.scale.set(-0.23, 0.23, 0.2);
          obj.position.set(point[0], point[1], point[2]);
          // Debug
          this.meshPoint.x = point[0];
          this.meshPoint.y = point[1];
          this.meshPoint.z = point[2];
          this.groupPointsInterest.add(obj);
        });
      }

      /**
       * Arrow pointer
       */
      const arrowArray = [
        [0, -64, -156],
        [0, -64, -196],
        [0, -64, -243]
        ]

        this.arrow1 = new THREE.Group();
        this.scene.add(this.arrow1);


      for(const arrow of arrowArray){
        gltfLoader.load("threeJs/arrow.glb", (glb) => {
        let obj = glb.scene;
        obj.position.set(arrow[0], arrow[1], arrow[2]);
        obj.name = 'arrow'
        this.arrow1.add(obj);
      });
      }
      console.log(this.arrow1.children)

      /**
       * Render
       */
      this.renderer = new THREE.WebGLRenderer();
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      container.appendChild(this.renderer.domElement);

      /**
       * Add Event Listener
       */
      container.style.touchAction = "none";
      container.addEventListener("pointerdown", this.onPointerDown);
      container.addEventListener("pointerup", this.onPointerUp);
      // Weel move camera
      // document.addEventListener("wheel", this.onDocumentMouseWheel)
      window.addEventListener("resize", this.onWindowResize);
    },

    onWindowResize() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();

      this.renderer.setSize(window.innerWidth, window.innerHeight);
    },
    onPointerDown(event) {
      if (event.isPrimary === false) return;

      this.isUserInteracting = true;

      this.onPointerDownMouseX = event.clientX;
      this.onPointerDownMouseY = event.clientY;

      this.onPointerDownLon = this.lon;
      this.onPointerDownLat = this.lat;

      document.addEventListener("pointermove", this.onPointerMove);
    },
    onPointerMove(event) {
      if (event.isPrimary === false) return;

      this.lon =
        (this.onPointerDownMouseX - event.clientX) * 0.1 +
        this.onPointerDownLon;
      this.lat =
        (event.clientY - this.onPointerDownMouseY) * 0.1 +
        this.onPointerDownLat;
    },
    onPointerUp() {
      this.isUserInteracting = false;
      document.removeEventListener("pointermove", this.onPointerMove);
    },
    onDocumentMouseWheel(event) {
      const fov = this.camera.fov + event.deltaY * 0.05;

      this.camera.fov = THREE.MathUtils.clamp(fov, 50, 115);

      this.camera.updateProjectionMatrix();
    },
    animate() {
      requestAnimationFrame(this.animate);
      this.update();
    },
    update() {
      if (this.isUserInteracting === false) {
        //	this.lon += 0.1
      }

      this.lat = Math.max(-85, Math.min(85, this.lat));
      this.phi = THREE.MathUtils.degToRad(90 - this.lat);
      this.theta = THREE.MathUtils.degToRad(this.lon);

      const x = 500 * Math.sin(this.phi) * Math.cos(this.theta);
      const y = 500 * Math.cos(this.phi);
      const z = 500 * Math.sin(this.phi) * Math.sin(this.theta);

      this.camera.lookAt(x, y, z);

      this.move += 0.015;
      let pointsLength = this.groupPointsInterest.children.length;
      if (pointsLength > 0) {
        for (const point of this.groupPointsInterest.children) {
          if (point.name == "textLau") {
            // Text move
            point.rotation.y =
              Math.sin(Date.now() * 0.001) * Math.PI * 0.1 - Math.PI;
          } else {
            // Point of interes Move
            point.rotation.y = this.move;
          }
        }
        /**
         * Debug
         */
        this.debugThreeJs(pointsLength);
      }
      const arrow = this.arrow1
      if(arrow.children.length > 0) arrow.position.z = Math.sin(Date.now() * 0.003) * 25

      this.renderer.render(this.scene, this.camera);
    },
    debugThreeJs(pointsLength) {
      // First heart
      const point = this.groupPointsInterest.children[1];
      if (pointsLength > 1 && point.name != "textLau")
        point.position.set(
          this.meshPoint.x,
          this.meshPoint.y,
          this.meshPoint.z
        );
    },
  },
};
</script>

