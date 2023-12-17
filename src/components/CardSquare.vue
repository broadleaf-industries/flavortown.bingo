<script setup>
import { computed, defineEmits, ref } from 'vue';

const props = defineProps({
  text: String,
  index: Number,
  stamped: Boolean,
})

const emit = defineEmits(['stampSquare', 'unstampSquare'])

const backgroundClass = computed(() => {
  return props.stamped ? 'stamped' : 'unstamped'
})

const toggleStamp = () => {
  (!props.stamped) ? emit('stampSquare', props.index) : emit('unstampSquare', props.index);
}
</script>

<template>
  <div @click="toggleStamp()" :class="backgroundClass + ' card-square'">
    {{ text }}
  </div>
</template>

<style scoped>
.unstamped {
  background-color: black;
  /* background-image: url('./flames_bw.png');
  background-size: contain; */
}
.stamped {
  background-color: black;
  background-image: url('./flames.png');
  background-size: cover;
}

.card-square {
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  max-width: 300px;
  aspect-ratio: 1;
  font-size: large;
  font-weight: bold;

  word-wrap: break-word;
  overflow-wrap: break-word;

  -webkit-hyphens: auto;
  -moz-hyphens: auto;
  hyphens: auto;
}
</style>
