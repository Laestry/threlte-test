<script lang="ts">
  import { Canvas } from '@threlte/core'
  import { HTML } from '@threlte/extras'
  import { World } from '@threlte/rapier'
  import Scene from "$lib/Scene.svelte";


</script>

<div style="width: 100vw; height: 100vh">
  <Canvas>
    <World>
      <Scene/>
      <HTML slot="fallback" transform>
      <p>
        It seems your browser<br/>
        doesn't support WASM.<br/>
        I'm sorry.
      </p>
      </HTML>
    </World>
  </Canvas>
</div>
<style>
  p {
    font-size: 0.75rem;
    line-height: 1rem;
  }
</style>