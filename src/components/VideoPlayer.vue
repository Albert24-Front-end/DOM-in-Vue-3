<script setup lang="ts">
import { ref, useTemplateRef } from 'vue';

defineProps<{
    src: string | undefined,
}>()

const emit = defineEmits<{
    (e: 'canplay'): void
    (e: 'metadata', metadata: {width: number; height: number}): void
}>()

const stats = ref({ resolution: '', duration: '' });

const videoPlayer = useTemplateRef<HTMLVideoElement>('video');

const updateStats = (e: Event) => {
    const videoElem = e.target as HTMLVideoElement;
    stats.value.resolution = `${videoElem.videoWidth}x${videoElem.videoHeight}`;
    stats.value.duration = videoElem.duration.toFixed(2);
    emit('canplay')
    emit('metadata', {width: videoElem.videoWidth, height: videoElem.videoHeight})
}

const makePreview = (): Promise<Blob> => {
    const videoElem = videoPlayer.value!
    const canvas = document.createElement('canvas')
    const ctx = canvas.getContext('2d')!
    canvas.width = videoElem.videoWidth
    canvas.height = videoElem.videoHeight
    ctx.drawImage(videoElem, 0, 0, canvas.width, canvas.height);

    return new Promise((resolve, reject) => {
        canvas.toBlob((blob) => {
            if (blob) {
                resolve(blob)
            } else {
                reject(new Error('Не удалось создать Blob'))
            }
        })
    })
}

defineExpose({
    makePreview,
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