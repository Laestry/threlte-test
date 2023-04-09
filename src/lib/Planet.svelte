<script context="module" lang="ts">
  export type PlanetType = {
    text: string;
    position: Position;
    rigidBody?: RapierRigidBody | undefined;
    mesh?: Mesh | undefined;
  };
</script>

<script lang="ts">
  import type { Position, Rotation } from "@threlte/core";
  import { T, useFrame } from "@threlte/core";
  import { RigidBody, Collider, Attractor } from "@threlte/rapier";
  import type { RigidBody as RapierRigidBody } from "@dimforge/rapier3d-compat";
  import { LayerMaterial, Fresnel } from "lamina/vanilla";
  import { Color, Mesh } from "three";
  import { Text } from "@threlte/extras";
  import { get } from "svelte/store";

  export let position: Position | undefined = undefined;
  export let rotation: Rotation | undefined = undefined;
  export let text: string | undefined;
  export let rigidBody: RapierRigidBody | undefined;
  export let mesh: Mesh | undefined;

  let cameraRotation: Rotation;

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

  useFrame(({ camera }) => {
    cameraRotation = get(camera).rotation;
  });
</script>

<RigidBody
  bind:rigidBody
  type={"dynamic"}
  {position}
  {rotation}
>
  <T.Group>
    <Text
      {text}
      position={{ y: 1.7 }}
      rotation={cameraRotation}
      scale={5}
      textAlign="center"
      anchorX="center"
    />
    <T.Mesh  bind:ref={mesh}>
      <T.BoxGeometry />
      <Collider
        contactForceEventThreshold={30}
        restitution={0.4}
        shape={"ball"}
        args={[1]}
      />
    </T.Mesh>
  </T.Group>
</RigidBody>
