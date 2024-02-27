<script setup>
import { computed } from 'vue';
const props = defineProps({
    alerta: {
        type: Object,
        required: true
    }
});

// computed permite que en el template al poner los : ejecute la función, en caso contrario lo colocaria como cadena de texto
const isError = computed(() => {
    return props.alerta.tipo === 'error';
});
</script>

<template>
    <div
        :class="[isError ? 'bg-red-500' : 'bg-green-500']"
        class="text-white text-center p-3 uppercase font-bold mb-3 rounded-md"
    >
        {{alerta.mensaje}}
    </div>
</template>
