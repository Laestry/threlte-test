<script lang="ts">
    import { T } from '@threlte/core'
    import { AutoColliders } from '@threlte/rapier'
    import { BoxGeometry, MeshStandardMaterial } from 'three'
</script>

<AutoColliders shape={'cuboid'} position={[0, -0.5, 0]}>
    <T.Mesh receiveShadow>
        <T.BoxGeometry args={[100, 1, 100]} />
        <T.MeshStandardMaterial />
    </T.Mesh>
</AutoColliders>
