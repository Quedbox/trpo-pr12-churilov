<script>
export default { props: { filters: Array, selected: String }, emits: ['change'] }
</script><template><label>Статус <select :value="selected" @change="$emit('change', $event.target.value)"><option v-for="filter in filters" :key="filter.id" :value="filter.id">{{ filter.title }}</option></select></label></template>
