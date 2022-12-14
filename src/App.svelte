<script lang="ts">
  import {
    Canvas,
    Mesh,
    OrbitControls,
    PerspectiveCamera,
    HemisphereLight,
  } from "threlte";
  import { SVGLoader } from "three/examples/jsm/loaders/SVGLoader.js";
  import outerSvg from "./lib/outer.svg?raw";
  import innerSvg from "./lib/inner.svg?raw";
  import { onMount } from "svelte";
  import * as THREE from "three";

  let outerGeometry: THREE.ExtrudeGeometry;
  let innerGeometry: THREE.ExtrudeGeometry;

  onMount(() => {
    const [outerShape] = new SVGLoader()
      .parse(outerSvg)
      .paths[0].toShapes(false);

    outerGeometry = new THREE.ExtrudeGeometry(outerShape, {
      curveSegments: 64,
      depth: 2,
      bevelEnabled: true,
      bevelOffset: 0,
      bevelSegments: 64,
      bevelSize: 1,
      bevelThickness: 2,
    });

    outerGeometry.center();

    const [innerShape] = new SVGLoader()
      .parse(innerSvg)
      .paths[0].toShapes(false);

    innerGeometry = new THREE.ExtrudeGeometry(innerShape, {
      curveSegments: 64,
      depth: 4,
      bevelEnabled: true,
      bevelOffset: 0,
      bevelSegments: 64,
      bevelSize: 1,
      bevelThickness: 2,
    });

    innerGeometry.center();
  });
</script>

<div>
  <Canvas>
    <PerspectiveCamera position={{ x: 0, y: 0, z: -200 }}>
      <OrbitControls autoRotate enableDamping />
    </PerspectiveCamera>

    <HemisphereLight skyColor={0x4c8eac} groundColor={0xac844c} intensity={1} />

    {#if outerGeometry}
      <Mesh
        castShadow
        geometry={outerGeometry}
        position={{ x: 0, y: 0, z: 0 }}
        rotation={{ x: 0, y: 0, z: 0 }}
        material={new THREE.MeshStandardMaterial({
          color: new THREE.Color("#ff3e00"),
          side: THREE.DoubleSide,
        })}
      />
    {/if}

    {#if innerGeometry}
      <Mesh
        castShadow
        geometry={innerGeometry}
        position={{ x: 0, y: 0, z: 0 }}
        rotation={{ x: 0, y: 0, z: 0 }}
        material={new THREE.MeshStandardMaterial({
          color: new THREE.Color("#ffffff"),
          side: THREE.DoubleSide,
        })}
      />
    {/if}
  </Canvas>
</div>

<style>
  div {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: #222;
  }
</style>
