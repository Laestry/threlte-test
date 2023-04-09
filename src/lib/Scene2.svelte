<script lang="ts">
  import type { PlanetType } from "./Planet.svelte";
  import { OrbitControls, T } from "@threlte/core";
  import { Environment } from "@threlte/extras";
  import PlanetGroup from "$lib/PlanetGroup.svelte";
  import { AutoColliders } from '@threlte/rapier'



  function generateRandomPlanets(numPlanets: number): PlanetType[] {
    const planets: PlanetType[] = [];

    for (let i = 0; i < numPlanets; i++) {
      const x = Math.floor(Math.random() * 200) - 100;
      const y = Math.floor(Math.random() * 200) - 100;
      const z = Math.floor(Math.random() * 200) - 100;
      const text = `${i + 1}`;

      planets.push({ text, position: { x, y, z } });
    }

    return planets;
  }

  let planetGroup1: PlanetType[] = generateRandomPlanets(20);

</script>

<Environment
  path="/"
  files="nebula_low_res.hdr"
  isBackground={true}
  format="hdr"
/>

<T.AmbientLight intensity={0.15} />

<T.PerspectiveCamera makeDefault position={[0, 3, 40]} fov={90}>
  <OrbitControls enableDamping target={{ y: 0.5 }} enableZoom={true} />
</T.PerspectiveCamera>

<PlanetGroup planets={planetGroup1} position={[0, 0, 0]} />

<AutoColliders shape={'cuboid'} position={[0, -0.5, 0]}>
  <T.Mesh receiveShadow>
    <T.BoxGeometry args={[10, 1, 10]} />
    <T.MeshStandardMaterial />
  </T.Mesh>
</AutoColliders>