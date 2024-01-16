<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';

interface Props {
  isChecked: boolean;
}

const props = defineProps<Props>()
const emit = defineEmits(['update:checked']);

const toggleChecked = () => {
  emit('update:checked', !props.isChecked);
};
</script>

<template>
  <div>
    <input type="checkbox" :checked="props.isChecked" @change="toggleChecked" />
    <label :class="{ 'checked': props.isChecked }">
      <slot></slot>
    </label>
  </div>
</template>

<style scoped>
label {
  cursor: pointer;
}
.checked {
  text-decoration: line-through;
}
</style>
