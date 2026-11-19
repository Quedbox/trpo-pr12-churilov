<script>
import FilterPanel from './components/FilterPanel.vue'
import ItemList from './components/ItemList.vue'
export default {
  components: { FilterPanel, ItemList },
  data() { return { items: [], filters: [], selected: 'all', loading: true, error: '', title: '', formError: '' } },
  computed: {
    filteredItems() { return this.selected === 'all' ? this.items : this.items.filter(item => item.status === this.selected) },
    count() { return this.filteredItems.length }
  },
  methods: {
    async loadData() {
      this.loading = true; this.error = ''
      try {
        const [items, filters] = await Promise.all([fetch('/data/items.json'), fetch('/data/filters.json')])
        if (!items.ok || !filters.ok) throw new Error('Не удалось загрузить данные')
        this.items = await items.json(); this.filters = await filters.json()
      } catch (error) { this.error = error.message }
      finally { this.loading = false }
    },
    addItem() {
      const title = this.title.trim()
      if (!title) { this.formError = 'Введите название'; return }
      const id = Math.max(0, ...this.items.map(item => item.id)) + 1
      this.items.push({ id, title, status: 'new', priority: 1 }); this.title = ''; this.formError = ''
    },
    removeItem(id) { this.items = this.items.filter(item => item.id !== id) }
  },
  mounted() { this.loadData() }
}
</script>
<template><main><h1>Список задач</h1><form @submit.prevent="addItem"><label>Новая задача <input v-model="title"></label><button>Добавить</button><p v-if="formError" class="error">{{ formError }}</p></form><p v-if="loading">Загрузка...</p><div v-else-if="error"><p class="error">{{ error }}</p><button @click="loadData">Повторить</button></div><template v-else><FilterPanel :filters="filters" :selected="selected" @change="selected = $event" /><p>Найдено: {{ count }}</p><p v-if="!count">Задачи не найдены.</p><ItemList v-else :items="filteredItems" @remove="removeItem" /></template></main></template>
