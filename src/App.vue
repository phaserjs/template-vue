<script setup>
import Phaser from 'phaser';
import { ref, toRaw } from 'vue';
import PhaserGame from './game/PhaserGame.vue';

// Only we can move the logo in the main menu
const can_move_logo = ref();
// Reference to the PhaserGame component (game and scene are exposed)
const phaser_ref = ref();
const logo_position = ref({ x: 0, y: 0 });

const changeScene = () => {
    const scene = toRaw(phaser_ref.value.scene);
    if (scene) {
        scene.changeScene();
    }
}

const moveLogo = () => {
    const scene = toRaw(phaser_ref.value.scene);
    if (scene) {
        // Get the update logo position
        scene.moveLogo(({ x, y }) => {
            logo_position.value = { x, y };
        });
    }
}

const addStars = () => {
    const scene = toRaw(phaser_ref.value.scene);
    if (scene) {
        // Add more stars
        const x = Phaser.Math.Between(100, scene.scale.width - 100);
        const y = Phaser.Math.Between(100, scene.scale.height - 100);

        scene.add.image(x, y, 'star');
    }
}

// Event emitted from the PhaserGame component
const currentScene = (scene) => {
    can_move_logo.value = (scene.scene.key !== "MainMenu");
}
</script>

<template>
    <PhaserGame ref="phaser_ref" @current-active-scene="currentScene" />
    <div>
        <div>
            <button class="button-change-scene" @click="changeScene">Change Scene</button>
        </div>
        <div>
            <button :disabled="can_move_logo" class="button-change-scene" @click="moveLogo">Move main Logo</button>
        </div>
        <div class="margin-left">Logo position:
            <pre>{{ logo_position }}</pre>
        </div>
        <div>
            <button class="button-change-scene" @click="addStars">Add stars</button>
        </div>
    </div>
</template>
