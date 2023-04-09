<script lang="ts">
  import type { Position, Rotation } from "@threlte/core";
  import { T, InteractiveObject } from "@threlte/core";
  import { RigidBody, Collider } from "@threlte/rapier";
  import type { RigidBody as RapierRigidBody } from "@dimforge/rapier3d-compat";
  import { LayerMaterial, Fresnel } from "lamina/vanilla";
  import { Color, Mesh } from "three";
  import { spring } from "svelte/motion";

  export let position: Position | undefined = undefined;
  export let rotation: Rotation | undefined = undefined;
  export let rigidBody: RapierRigidBody | undefined = undefined;
  export let mesh: Mesh | undefined;

  const scale = spring(1);

  const laminaMaterial = new LayerMaterial({
    color: "#ffffff",
    lighting: "standard",
    layers: [
      new Fresnel({
        color: new Color("#b4baff"),
        alpha: 1,
        power: 1.5,
        intensity: 1.5,
        bias: 0,
        mode: "normal",
        visible: true,
      }),
    ],
  });
</script>

<RigidBody
  bind:rigidBody
  type={"dynamic"}
  {position}
  {rotation}
  linearDamping={2}
>
  <T.Group scale={$scale}>
    <T.Mesh material={laminaMaterial} bind:ref={mesh} let:ref>
      <T.SphereGeometry args={[3]} />
      <Collider
        contactForceEventThreshold={30}
        restitution={0.4}
        shape={"ball"}
        args={[3]}
      />
      <InteractiveObject
        object={ref}
        interactive
        on:pointerenter={() => ($scale = 1.2)}
        on:pointerleave={() => ($scale = 1)}
      />
    </T.Mesh>
  </T.Group>
</RigidBody>
