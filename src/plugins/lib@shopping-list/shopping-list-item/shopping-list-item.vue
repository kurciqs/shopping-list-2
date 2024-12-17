<template>
    <li>
        <p> {{ item.name }}: {{ item.value }} </p>
        <input type="checkbox" :checked="item.is_checked" @click="tickCheckbox(item)">
    </li>
</template>

<script>
import axios from 'axios'

export default {
    props: {
        item: {
            type: Object,
            required: true
        }
	},
    methods: {
        async tickCheckbox(item) {
            const oldState = item.is_checked
            try {
                const response = await axios.put(
                `https://shoppinglist.wezeo.dev/cms/api/v1/shopping-lists/${this.$route.params.id}/items/${item.id}`,
                { is_checked: !oldState }
                );
                item.is_checked = response.data.data.is_checked;
            } catch (error) {
                console.error('Checkbox error:', error.message, error.response?.data);
                item.is_checked = oldState
            }
        }   
    }
}
</script>