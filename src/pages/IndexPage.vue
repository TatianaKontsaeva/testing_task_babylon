<template>
    <q-page class="flex flex-center q-gutter-lg q-pa-lg">
      <q-btn
        outline
        label="Курсор"
        :class="toggleBtn[0] ? 'pushedButton' : ''"
        @click="toggleGizmo(0)"
      />
      <q-btn
        outline
        label="Смещение"
        :class="toggleBtn[1] ? 'pushedButton' : ''"
        @click="toggleGizmo(1,'positionGizmoEnabled')"
      />
      <q-btn
        outline
        label="Вращение"
        :class="toggleBtn[2] ? 'pushedButton' : ''"
        @click="toggleGizmo(2,'rotationGizmoEnabled')"
      />
      <q-btn
        outline
        label="Масштабирование"
        :class="toggleBtn[3] ? 'pushedButton' : ''"
        @click="toggleGizmo(3,'scaleGizmoEnabled')"

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
const renderCanvas = ref(null);

//переключение действий
let toggleBtn = ref([false, true, false, false]);
const gizmoManager = ref(null);
const currentAction = ref("positionGizmoEnabled");

const toggleGizmo = (index, gizmo) => {
  toggleBtn.value = [false, false, false, false];
  toggleBtn.value[index] = true;
  if (gizmoManager.value) {
    gizmoManager.value[currentAction.value] = false;
    gizmoManager.value[gizmo] = !gizmoManager.value[gizmo];
  }
  currentAction.value = gizmo;
};
const toggleAction = function (arrayValues) {
  gizmoManager.value.positionGizmoEnabled = arrayValues[1];
  gizmoManager.value.rotationGizmoEnabled = arrayValues[2];
  gizmoManager.value.scaleGizmoEnabled = arrayValues[3];
  toggleAction(toggleBtn.value);

};

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

//создаем объект
  let sphere = BABYLON.Mesh.CreateSphere("sphere1", 16, 2, scene);
  sphere.material = new BABYLON.StandardMaterial("sphere material", scene);
  sphere.rotation.z = Math.PI/2
  sphere.position.y = 1;
  sphere.material.diffuseColor = BABYLON.Color3.Yellow();

// Initialize GizmoManager
  gizmoManager.value = new BABYLON.GizmoManager(scene);
  gizmoManager.value.positionGizmoEnabled = true;
  gizmoManager.updateGizmoRotationToMatchAttachedMesh = false;

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
.pushedButton {
  color: rgb(53, 93, 214);
  border: 1px solid rgb(53, 93, 214);
  transform: translateY(3px);
}

</style>