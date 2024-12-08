<script setup>
import { onMounted } from 'vue';  // 引入 Vue 的生命周期钩子
import { Scene, BoxGeometry, MeshBasicMaterial, PerspectiveCamera, Mesh, WebGLRenderer, AxesHelper } from 'three';

onMounted(() => {
  // 在 onMounted 中获取 canvas 元素，确保模板渲染完毕
  const canvas = document.querySelector('#canvas');
  const scene = new Scene();
  const geometry = new BoxGeometry(100, 100, 100);
  const material = new MeshBasicMaterial({
    color: 0x00ff00, 
    transparent: true,//开启透明
    opacity: 0.5,
  });
  const box = new Mesh(geometry, material);
  // axeHelper 辅助线
  const axeHelper=new AxesHelper(150);
  scene.add(box);
  scene.add(axeHelper);

  const camera = new PerspectiveCamera(30, canvas.width / canvas.height, 1, 3000);
  camera.position.set(200, 200, 200);
  camera.lookAt(box.position); // 注意这里的 `box.position`，而不是 `mesh.position`，因为 `mesh` 没有定义

  const renderer = new WebGLRenderer({ canvas: canvas });
  // 设置渲染器的背景色
  renderer.setClearColor(0x999999);


  // 渲染函数
  function render() {
    renderer.render(scene, camera);
    box.rotateY(0.01); // 每次绕y轴旋转0.01弧度
    requestAnimationFrame(render); // 请求再次执行渲染函数render，渲染下一帧
  }
  render();
});
</script>

<template>
  <header>
  </header>
  <main>
    <canvas id="canvas" style="background: black;" width="800" height="800"></canvas>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}
</style>
