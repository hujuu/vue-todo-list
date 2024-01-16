<script setup lang="ts">
import ListItem from './ListItem.vue'
import { ref, onMounted, computed } from 'vue'
import type { Ref } from 'vue'

type Item = {
  title: string,
  checked?: boolean
}

const storageItems: Ref<Item[]> = ref([])

const initListItems = (): void => {
  if (storageItems.value?.length === 0) {
    const listItems = [
      {title: 'Make a todo list app', checked: true},
      {title: 'Predict the weather', checked: false},
      {title: 'Read some comics', checked: false},
      {title: 'Let\'s get cooking', checked: false},
      {title: 'Pump some iron', checked: false},
      {title: 'Track my expenses', checked: false},
      {title: 'Organise a game night', checked: false},
      {title: 'Learn a new language', checked: false},
      {title: 'Publish my work'}
    ]
    setToStorage(listItems)
    storageItems.value = listItems
  }
}

const sortedList = computed(() => [...storageItems.value].sort((a, b) => (a.checked ? 1 : 0) - (b.checked ? 1 : 0)))
const setToStorage = (items: Item[]): void => {
  localStorage.setItem('list-items', JSON.stringify(items))
}
const getFromStorage = (): Item[] | [] => {
  const stored = localStorage.getItem('list-items')
  if (stored) {
    return JSON.parse(stored)
  }
  return []
}

const handleUpdateChecked = (item: Item, newChecked: boolean) => {
  const foundItem = storageItems.value.find(i => i.title === item.title);
  if (foundItem) {
    foundItem.checked = newChecked;
    setToStorage(storageItems.value);
  }
};

onMounted(() => {
  const storedItems = getFromStorage();
  if (storedItems && storedItems.length > 0) {
    storageItems.value = storedItems;
  } else {
    initListItems();
  }
});
</script>

<template>
  <ul>
    <li :key="item.title" v-for="item in sortedList">
      <ListItem :is-checked="item.checked" @update:checked="newChecked => handleUpdateChecked(item, newChecked)">
        {{ item.title }}
      </ListItem>
    </li>
  </ul>
</template>

<style scoped>
ul {
  list-style: none;
}

li {
  margin: 0.4rem 0;
}
</style>
