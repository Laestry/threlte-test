<script lang="ts">
  import type { RigidBody as RapierRigidBody } from "@dimforge/rapier3d-compat";
  import { T, useFrame } from "@threlte/core";
  import { Vector3, type Mesh } from "three";
  import type { PlanetType } from "./Planet.svelte";
  import Planet from "./Planet.svelte";
  import PlanetGroupCore from "./PlanetGroupCore.svelte";

  export const position: number[] | undefined = undefined;
  export let planets: PlanetType[];

  let coreRigidBody: RapierRigidBody;
  let coreMesh: Mesh | undefined;
  const corePosition = new Vector3();
  const planetPosition = new Vector3();
  const maxF = 50;
  const forceMultiplier = new Vector3(10, 10, 10);
  const min = new Vector3(-maxF, -maxF, -maxF);
  const max = new Vector3(maxF, maxF, maxF);

  const startDelay = 50; // Start animating after this many frames
  let frameCount = 0;
  useFrame(() => {
    frameCount++;
    if (frameCount < startDelay) return;
    // Go through each planet at apply force in the direction of the core if
    // sufficiently far away.
    coreMesh?.getWorldPosition(corePosition);
    planets.forEach((planet) => {
      planet.mesh?.getWorldPosition(planetPosition);
      const diff = corePosition.clone().sub(planetPosition).divideScalar(20);
      const f = diff.clamp(min, max);
      if (diff.length() > 0.5) {
        planet.rigidBody?.applyImpulse(f.multiply(forceMultiplier), true);
      }
    });
  });
</script>

<T.Group {position}>

  {#each planets as planet}
    <Planet
      bind:rigidBody={planet.rigidBody}
      bind:mesh={planet.mesh}
      text={planet.text}
      position={planet.position}
    />
  {/each}
</T.Group>
