<template>
    <q-page class="flex flex-center q-gutter-lg q-pa-lg">
      <q-btn
        class="bubbleBtns"
        glossy
        outline
        label="Курсор"
      />
      <q-btn
        class="bubbleBtns"
        glossy
        outline
        label="Смещение"
      />
      <q-btn
        class="bubbleBtns"
        glossy
        outline
        label="Вращение"
      />
      <q-btn
        class="bubbleBtns"
   
        glossy
        outline
        label="Масштабирование"
      />
    <canvas ref="renderCanvas" id="renderCanvas" touch-action="none"></canvas>
    </q-page>
</template>

<script setup>
import { ref, onMounted } from "vue";

import * as BABYLON from "@babylonjs/core/Legacy/legacy";
const { pages } = defineProps({
  pages: Array,
});

let bubbleBtns = ref([true, false, false]);
const renderCanvas = ref(null);
const gizmoManager = ref(null);

onMounted(() => {
  const canvas = renderCanvas.value;
  const engine = new BABYLON.Engine(canvas);
  let scene = new BABYLON.Scene(engine);

  //создаем камеру
  let camera = new BABYLON.FreeCamera(
    "camera1", new BABYLON.Vector3(0, 5, -10),scene
  );
  camera.setTarget(BABYLON.Vector3.Zero());
  camera.attachControl(canvas, true);

  //создаем свет
  let light = new BABYLON.HemisphericLight( 
    "light", new BABYLON.Vector3(0, 1, 0), scene
  );
  light.position = new BABYLON.Vector3(0, 5, -2);
  light.intensity = 0.4;

   //создаем ground
  let ground = BABYLON.MeshBuilder.CreateGround(
    "ground1",{width: 8, height: 8,}, scene
  );
  ground.position.y = -1;
  scene.createDefaultXRExperienceAsync({ floorMeshes: [ground] });

//создаем объекты
  let spheres = [];

  for (let i = 0; i < 3; i++) {
    let sphere = BABYLON.Mesh.CreateSphere("sphere1", 16, 1.8, scene);
    sphere.material = new BABYLON.StandardMaterial("sphere material", scene);
    sphere.position.z = i+1;
    sphere.position.y = 1;
    sphere.position.x = i-1.5
    sphere.actionManager = new BABYLON.ActionManager(scene);
    sphere.material.diffuseColor = BABYLON.Color3.Green();
    spheres.push(sphere);
  }
  // Initialize GizmoManager
  const gizmoManager = new BABYLON.GizmoManager(scene)

  gizmoManager.positionGizmoEnabled = true;
  gizmoManager.rotationGizmoEnabled = false;
  gizmoManager.scaleGizmoEnabled = true;
  gizmoManager.boundingBoxGizmoEnabled = true;


  // Restrict gizmos to only spheres
  gizmoManager.attachableMeshes = spheres
  // Toggle gizmos with keyboard buttons
  document.onkeydown = (e)=>{
      if(e.key == 'w'){
          gizmoManager.positionGizmoEnabled = !gizmoManager.positionGizmoEnabled
      }
      if(e.key == 'e'){
          gizmoManager.rotationGizmoEnabled = !gizmoManager.rotationGizmoEnabled
      }
      if(e.key == 'r'){
          gizmoManager.scaleGizmoEnabled = !gizmoManager.scaleGizmoEnabled
      }
      if(e.key == 'q'){
          gizmoManager.boundingBoxGizmoEnabled = !gizmoManager.boundingBoxGizmoEnabled
      }
      	return scene;
  }
 
 // Register a render loop
  engine.runRenderLoop(() => {
    scene.render();
  });
});
</script>

<style lang="scss">
#renderCanvas {
        width: 100%;
        height: 100%;
        touch-action: none;
      }

</style>