<script lang="ts">
    import {OrbitControls, T} from '@threlte/core'
    import { Debug } from '@threlte/rapier'
    import Emitter from './Emitter.svelte'
    import Ground from './Ground.svelte'
</script>
<T.PerspectiveCamera
        makeDefault
        position={[10, 10, 10]}
>
    <OrbitControls enableZoom={true} />
</T.PerspectiveCamera>
<T.DirectionalLight
        castShadow
        position={[8, 20, -3]}
/>
<T.GridHelper args={[50]} />
<Ground />
<Debug />
<Emitter />