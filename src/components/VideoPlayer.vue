<script setup lang="ts">
import { ref, useTemplateRef } from 'vue';

defineProps<{
    src: string | undefined,
}>()

const emit = defineEmits<{
    (e: 'canplay'): void
}>()

const stats = ref({ resolution: '', duration: '' });

const videoPlayer = useTemplateRef<HTMLVideoElement>('video');

const updateStats = (e: Event) => {
    const videoElem = e.target as HTMLVideoElement;
    stats.value.resolution = `${videoElem.videoWidth}x${videoElem.videoHeight}`;
    stats.value.duration = videoElem.duration.toFixed(2);
    emit('canplay')
}

const makePreview = (canvas: HTMLCanvasElement) => {
    const videoElem = videoPlayer.value!
    const ctx = canvas.getContext('2d')!
    canvas.width = videoElem.videoWidth
    canvas.height = videoElem.videoHeight
    ctx.drawImage(videoElem, 0, 0, canvas.width, canvas.height);
}

defineExpose({
    makePreview
})
</script>

<template>
    <div class="player">
        <video ref="video" controls :src="src" @canplay="updateStats"></video>
        <div v-if="stats.duration" class="stats">
            Длительность: {{ stats.duration }} сек.
            Разрешение: {{ stats.resolution }}
        </div>
    </div>
</template>

<style scoped>
.player {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.player video {
    flex: 1;
}

.stats {
    margin-top: 10px;
}

</style>