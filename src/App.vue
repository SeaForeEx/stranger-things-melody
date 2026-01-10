<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

// ===== STATE =====
const audioContext = ref<AudioContext | null>(null)
const activeOscillators = ref<Map<number, [OscillatorNode, GainNode]>>(new Map())

const keyToFrequency: Record<string, number> = {
    a: 261.63, // C4
    s: 329.63, // E4
    d: 392.0, // G4
    f: 493.88, // B4
    g: 523.25, // C5
}

// ===== LIFECYCLE =====
onMounted(() => {
    audioContext.value = new AudioContext()
    window.addEventListener('keydown', handleKeyDown)
    window.addEventListener('keyup', handleKeyUp)
})

onUnmounted(() => {
    window.removeEventListener('keydown', handleKeyDown)
    window.removeEventListener('keyup', handleKeyUp)
})

// ===== EVENT HANDLERS =====
function handleKeyDown(event: KeyboardEvent) {
    const key = event.key.toLowerCase()
    const frequency = keyToFrequency[key]

    if (!frequency || event.repeat) return

    startNote(frequency)
}

function handleKeyUp(event: KeyboardEvent) {
    const key = event.key.toLowerCase()
    const frequency = keyToFrequency[key]

    if (!frequency) return

    stopNote(frequency)
}

// ===== AUDIO FUNCTIONS =====
function startNote(note: number) {
    if (!audioContext.value) return

    // Resume AudioContext if suspended (mobile requirement)
    if (audioContext.value.state === 'suspended') {
        try {
            audioContext.value.resume()
        } catch (error) {
            console.error('Failed to resume context:', error)
            return
        }
    }

    if (activeOscillators.value.has(note)) return

    // Create and configure audio nodes
    const oscillator = audioContext.value.createOscillator()
    const gainNode = audioContext.value.createGain()

    oscillator.connect(gainNode)
    gainNode.connect(audioContext.value.destination)

    oscillator.frequency.value = note
    oscillator.type = 'sine'

    // Smooth attack envelope (fade in)
    const now = audioContext.value.currentTime
    gainNode.gain.setValueAtTime(0, now)
    gainNode.gain.linearRampToValueAtTime(0.3, now + 0.005)

    oscillator.start()

    activeOscillators.value.set(note, [oscillator, gainNode])
}

function stopNote(note: number) {
    if (!activeOscillators.value) return

    const nodes = activeOscillators.value.get(note)
    if (!nodes?.[0] || !nodes?.[1]) return

    const now = audioContext.value!.currentTime
    const [oscillator, gain] = nodes

    // Smooth release envelope (fade out)
    gain.gain.cancelScheduledValues(now)
    gain.gain.setValueAtTime(gain.gain.value, now)
    gain.gain.exponentialRampToValueAtTime(0.0000001, now + 0.15)

    oscillator.stop(now + 0.2)

    activeOscillators.value.delete(note)
}
</script>
f

<template>
    <div class="app-container">
        <div class="outlined header">StrangeR ThingS MelodY</div>

        <div class="instructions">
            <p>Press the keys A, S, D, F, G (or click the buttons) to play the melody!</p>
            <br />
            <p>Sequence: A→S→D→F→G→F→D→S</p>
            <br />
            <div class="silent-mode-container">
                <p class="silent-mode">📱 iPhone/iPad users:</p>
                &nbsp;
                <p class="silent-mode">TURN OFF SILENT MODE!</p>
            </div>
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
