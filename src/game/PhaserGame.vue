<script setup>
import { onMounted, ref } from 'vue';
import { EventBus } from './EventBus';
import StartGame from './main';

// Save the current scene instance
const scene = ref();
const game = ref();

const emit = defineEmits(['current-active-scene']);

onMounted(() => {

    game.value = StartGame('game-container');

    EventBus.on('current-scene-ready', (scene) => {

        emit('current-active-scene', scene);

        scene.value = scene;

    });

});

defineExpose({ scene, game });
</script>

<template>
    <div id="game-container"></div>
</template>