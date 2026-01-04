<script setup lang="ts">
// Import Vue Composition API functions
import { ref, onMounted, onUnmounted } from 'vue'

// Create reactive references for Web Audio API nodes
const audioContext = ref<AudioContext | null>(null)
const currentOscillator = ref<OscillatorNode | null>(null)
const currentGain = ref<GainNode | null>(null)

// Map keyboard keys to frequencies
const keyToFrequency: Record<string, number> = {
    a: 261.63, // C4
    s: 329.63, // E4
    d: 392.0, // G4
    f: 493.88, // B4
    g: 523.25, // C5
}

// Lifecycle hook: runs after component is mounted to the DOM
onMounted(() => {
    // Initialize the AudioContext (enables Web Audio API)
    audioContext.value = new AudioContext()

    // Add keyboard event listeners
    window.addEventListener('keydown', handleKeyDown)
    window.addEventListener('keyup', handleKeyUp)
})

// Lifecycle hook: runs when component is destroyed/removed from DOM
onUnmounted(() => {
    // Remove keyboard event listeners on cleanup
    window.removeEventListener('keydown', handleKeyDown)
    window.removeEventListener('keyup', handleKeyUp)
})

function handleKeyDown(event: KeyboardEvent) {
    const key = event.key.toLowerCase()
    const frequency = keyToFrequency[key]

    // Exit if key is not mapped or note is already playing
    if (!frequency || currentOscillator.value) return

    // Prevent key repeat
    if (event.repeat) return

    startNote(frequency)
}

function handleKeyUp(event: KeyboardEvent) {
    const key = event.key.toLowerCase()
    const frequency = keyToFrequency[key]

    // Only stop if the released key is one of our musical keys
    if (!frequency) return

    stopNote()
}

function startNote(note: number) {
    // Exit if AudioContext doesn't exist OR if a note is already playing
    if (!audioContext.value || currentOscillator.value) return

    // Create nodes
    const oscillator = audioContext.value.createOscillator()
    const gainNode = audioContext.value.createGain()

    // Connect
    oscillator.connect(gainNode)
    gainNode.connect(audioContext.value.destination)

    // Configure
    oscillator.frequency.value = note
    oscillator.type = 'sine'
    gainNode.gain.value = 0.3

    // Start
    oscillator.start()

    // Store references
    currentOscillator.value = oscillator
    currentGain.value = gainNode
}

function stopNote() {
    // Exit if no note is currently playing
    if (!currentOscillator.value) return

    // Get current audio timestamp for precise timing
    const now = audioContext.value!.currentTime

    // Fade out volume over 0.05 seconds to prevent clicking/popping
    currentGain.value!.gain.exponentialRampToValueAtTime(0.01, now + 0.05)

    // Stop the oscillator after fade out completes
    currentOscillator.value.stop(now + 0.05)

    // Clear references so a new note can be started
    currentOscillator.value = null
    currentGain.value = null
}
</script>

<template>
    <div class="app-container">
        <div class="outlined header">Welcome to HawkinS</div>
        <div class="button-container">
            <button
                class="outlined button"
                @mousedown="startNote(261.63)"
                @mouseup="stopNote"
                @mouseleave="stopNote"
            >
                C4
            </button>
            <button
                class="outlined button"
                @mousedown="startNote(329.63)"
                @mouseup="stopNote"
                @mouseleave="stopNote"
            >
                E4
            </button>
            <button
                class="outlined button"
                @mousedown="startNote(392.0)"
                @mouseup="stopNote"
                @mouseleave="stopNote"
            >
                G4
            </button>
            <button
                class="outlined button"
                @mousedown="startNote(493.88)"
                @mouseup="stopNote"
                @mouseleave="stopNote"
            >
                B4
            </button>
            <button
                class="outlined button"
                @mousedown="startNote(523.25)"
                @mouseup="stopNote"
                @mouseleave="stopNote"
            >
                C5
            </button>
        </div>
    </div>
</template>

<style scoped></style>
