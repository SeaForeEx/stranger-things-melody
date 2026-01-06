<script setup lang="ts">
// Import Vue Composition API functions
import { ref, onMounted, onUnmounted } from 'vue'

// Reactive reference for Web Audio API context
const audioContext = ref<AudioContext | null>(null)

// Map to track currently playing notes (frequency → [oscillator, gain])
const activeOscillators = ref<Map<number, [OscillatorNode, GainNode]>>(new Map())

// Lookup table: keyboard keys to note frequencies
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

    console.log('AudioContext created')
    console.log('Initial state:', audioContext.value.state)
    console.log('Destination:', audioContext.value.destination)
    console.log('Sample rate:', audioContext.value.sampleRate)

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
    if (!frequency) return

    // Prevent key repeat
    if (event.repeat) return

    startNote(frequency)
}

function handleKeyUp(event: KeyboardEvent) {
    const key = event.key.toLowerCase()
    const frequency = keyToFrequency[key]

    // Only stop if the released key is one of our musical keys
    if (!frequency) return

    stopNote(frequency)
}

async function startNote(note: number) {
    console.log('startNote called', note)

    // Exit if AudioContext doesn't exist
    if (!audioContext.value) {
        console.log('no audioContext')
        return
    }

    console.log('AudioContext state:', audioContext.value.state)

    // Resume AudioContext if suspended (required for mobile)
    if (audioContext.value.state === 'suspended') {
        console.log('Resuming suspended context...')
        try {
            await audioContext.value.resume()
            console.log('Context resumed, new state:', audioContext.value.state)
        } catch (error) {
            console.error('Failed to resume context:', error)
            return
        }
    }

    // Exit if note is already playing
    if (activeOscillators.value.has(note)) {
        console.log('Note already playing')
        return
    }

    console.log('Creating oscillator...')

    // Create nodes
    const oscillator = audioContext.value.createOscillator()
    const gainNode = audioContext.value.createGain()

    // Connect
    oscillator.connect(gainNode)
    gainNode.connect(audioContext.value.destination)

    // Configure
    oscillator.frequency.value = note
    oscillator.type = 'sine'
    gainNode.gain.value = 1.0

    console.log('Oscillator configured:')
    console.log('- Frequency:', note)
    console.log('- Type:', oscillator.type)
    console.log('- Gain:', gainNode.gain.value)
    console.log('- Gain outputs:', gainNode.numberOfOutputs)

    // Start
    oscillator.start()

    console.log('Oscillator started')

    // Store reference
    activeOscillators.value.set(note, [oscillator, gainNode])
}

function stopNote(note: number) {
    // Safety check: exit if no note is currently playing
    if (!activeOscillators.value) return

    // Get oscillator and gain node for this note
    const oscillator = activeOscillators!.value.get(note)

    // Safety check: exit if note isn't playing or nodes are missing
    if (!oscillator || !oscillator[0] || !oscillator[1]) return

    // Get current audio timestamp for precise timing
    const now = audioContext.value!.currentTime

    // Fade out volume over 0.05 seconds to prevent clicking/popping
    oscillator[1].gain.exponentialRampToValueAtTime(0.01, now + 0.05)

    // Stop the oscillator after fade out completes
    oscillator[0].stop(now + 0.05)

    // Remove this note from activeOscillators map
    activeOscillators.value.delete(note)
}
</script>

<template>
    <div class="app-container">
        <div class="outlined header">Stranger ThingS MelodY</div>
        <div class="instructions">
            <p>Press the keys A, S, D, F, G (or click the buttons) to play the melody!</p>
            <br />
            <p>Sequence: A→S→D→F→G→F→D→S</p>
        </div>
        <div class="button-container">
            <div class="button-and-label">
                <button
                    class="key-button"
                    @mousedown="startNote(261.63)"
                    @mouseup="stopNote(261.63)"
                    @mouseleave="stopNote(261.63)"
                    @touchstart.prevent="startNote(261.63)"
                    @touchend.prevent="stopNote(261.63)"
                >
                    A
                </button>
                <span class="note-label">C4</span>
            </div>
            <div class="button-and-label">
                <button
                    class="key-button"
                    @mousedown="startNote(329.63)"
                    @mouseup="stopNote(329.63)"
                    @mouseleave="stopNote(329.63)"
                    @touchstart.prevent="startNote(329.63)"
                    @touchend.prevent="stopNote(329.63)"
                >
                    S
                </button>
                <span class="note-label">E4</span>
            </div>
            <div class="button-and-label">
                <button
                    class="key-button"
                    @mousedown="startNote(392.0)"
                    @mouseup="stopNote(392.0)"
                    @mouseleave="stopNote(392.0)"
                    @touchstart.prevent="startNote(392.0)"
                    @touchend.prevent="stopNote(392.0)"
                >
                    D
                </button>
                <span class="note-label">G4</span>
            </div>
            <div class="button-and-label">
                <button
                    class="key-button"
                    @mousedown="startNote(493.88)"
                    @mouseup="stopNote(493.88)"
                    @mouseleave="stopNote(493.88)"
                    @touchstart.prevent="startNote(493.88)"
                    @touchend.prevent="stopNote(493.88)"
                >
                    F
                </button>
                <span class="note-label">B4</span>
            </div>
            <div class="button-and-label">
                <button
                    class="key-button"
                    @mousedown="startNote(523.25)"
                    @mouseup="stopNote(523.25)"
                    @mouseleave="stopNote(523.25)"
                    @touchstart.prevent="startNote(523.25)"
                    @touchend.prevent="stopNote(523.25)"
                >
                    G
                </button>
                <span class="note-label">C5</span>
            </div>
        </div>
    </div>
</template>

<style scoped></style>
