<script lang="ts">
    import { useFrame } from '@threlte/core'
    import { Euler, Vector3 } from 'three'
    import Particle from './Particle.svelte'
    const getId = () => {
        return Math.random().toString(16).slice(2)
    }
    const getRandomPosition = () => {
        return new Vector3(500, 500, 500)
    }
    const getRandomRotation = () => {
        return new Euler(Math.random() * 10, Math.random() * 10, Math.random() * 10)
    }
    type Body = {
        id: string
        mounted: number
        position: Vector3
        rotation: Euler
    }
    let bodies: Body[] = []
    let lastBodyMounted: number = 0
    let bodyEveryMilliseconds = 1010
    let longevityMilliseconds = 8000
    useFrame(() => {
        if (lastBodyMounted + bodyEveryMilliseconds < Date.now()) {
            const body: Body = {
                id: getId(),
                mounted: Date.now(),
                position: getRandomPosition(),
                rotation: getRandomRotation()
            }
            console.log(body)
            bodies.unshift(body)
            lastBodyMounted = Date.now()
            bodies = bodies
        }
        const deleteIds: string[] = []
        bodies.forEach((body) => {
            if (body.mounted + longevityMilliseconds < Date.now()) {
                deleteIds.push(body.id)
            }
        })
        if (deleteIds.length) {
            deleteIds.forEach((id) => {
                const index = bodies.findIndex((body) => body.id === id)
                if (index !== -1) bodies.splice(index, 1)
            })
            bodies = bodies
        }
    })
</script>
{#each bodies as body (body.id)}
    <Particle rotation={body.rotation} />
{/each}