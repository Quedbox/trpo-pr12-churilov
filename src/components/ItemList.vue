<script>
import ItemCard from './ItemCard.vue'
export default { components: { ItemCard }, props: { items: Array }, emits: ['remove'] }
</script><template><div><ItemCard v-for="item in items" :key="item.id" :item="item" @remove="$emit('remove', $event)" /></div></template>
