<script setup lang="ts">
// Import Vue Composition API functions
import { ref, onMounted } from 'vue'

// Create a reactive reference to store the AudioContext
// Starts as null, will be initialized when component mounts
const audioContext = ref<AudioContext | null>(null)

// Lifecycle hook: runs after component is mounted to the DOM
onMounted(() => {
    // Initialize the AudioContext (enables Web Audio API)
    audioContext.value = new AudioContext()
})

function playNote(note: number) {
    // Check if AudioContext exists before trying to use it
    if (!audioContext.value) return

    // Create an oscillator node (generates sound waves)
    const oscillator = audioContext.value.createOscillator()

    // Create a gain node (controls volume)
    const gainNode = audioContext.value.createGain()

    // Connect oscillator to gain node
    oscillator.connect(gainNode)

    // Connect gain node to speakers (destination)
    gainNode.connect(audioContext.value.destination)

    // Set the frequency to specific button frequency
    oscillator.frequency.value = note

    // Set the wave type to sine (smooth, pure tone)
    oscillator.type = 'sine'

    // Set volume to 30% (prevents ear damage!)
    gainNode.gain.value = 0.3

    // Get current audio timestamp for precise timing
    const now = audioContext.value.currentTime

    // Start playing the note immediately
    oscillator.start(now)

    // Stop playing after 0.5 seconds
    oscillator.stop(now + 0.5)
}
</script>

<template>
    <div class="app-container">
        <div class="outlined header">Welcome to HawkinS</div>
        <div class="button-container">
            <button class="outlined button" @click="playNote(261.63)">C4</button>
            <button class="outlined button" @click="playNote(329.63)">E4</button>
            <button class="outlined button" @click="playNote(392.0)">G4</button>
            <button class="outlined button" @click="playNote(493.88)">B4</button>
            <button class="outlined button" @click="playNote(523.25)">C5</button>
        </div>
    </div>
</template>

<style scoped></style>
