<script>
export default { props: { item: Object }, emits: ['remove'] }
</script><template><article><strong>{{ item.title }}</strong><span>Статус: {{ item.status }} · Приоритет: {{ item.priority }}</span><button @click="$emit('remove', item.id)">Удалить</button></article></template>
