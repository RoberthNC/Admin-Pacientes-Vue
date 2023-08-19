<script setup>
    import { computed } from 'vue';

    const props = defineProps({
        alerta:{
            type:Object,
            required:true
        }
    });

    const isError = computed(() => props.alerta.tipo === 'error' ? 'bg-red-500':'bg-green-500' );
</script>

<template>
    <div
        class="text-white text-center p-3 uppercase font-bold mb-3 rounded-md"
        :class="isError"
    >
        {{ alerta.mensaje }}
    </div>
</template>