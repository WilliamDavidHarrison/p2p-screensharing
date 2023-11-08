<script setup lang="ts">
defineProps<{
    title: string;
}>();

defineEmits<{
    (e: "cancel"): void;
}>();
</script>

<template>
    <div class="text-center">
        <h2 class="motion-safe:animate-pulse text-xl mt-6 mb-8">{{ title }}</h2>

        <button class="text-red-500 hover:text-red-400 font-semibold transition-colors" @click="$emit('cancel')">Cancel</button>
    </div>
</template>
