<template>
  <template v-if="!shoppingList">
    <p>Načítavam dáta</p>
  </template>

  <template v-else-if="shoppingList.error">
    Pri načítaní dát nastala chyba: {{ shoppingList.error }}
  </template>

  <template v-else>
    <h2> Nazov zoznamu: {{ shoppingList.title }} </h2>
    Počet položiek v zozname: {{ shoppingList.items.length }}
    <ul v-for='item in shoppingList.items' :key='item.id'>
      <li>
        <p> {{ item.name }}: {{ item.value }} </p>
        <input type="checkbox" :checked="item.is_checked" @click='tickCheckbox(item)'>
      </li>
    </ul>
    <button @click='deleteList(shoppingList)'> Zmazat zoznam </button>
    <button @click="newItem()" > Pridat polozku </button>
    <br>
    <input @keyup.enter="newItem" v-model="input" placeholder="Add new item">
  </template>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      shoppingList: null,
      input: ''
    }
  },

  async mounted() {
    console.log("Mounting")
    try {
      this.updateShoppingList();
    } catch (error) {
      console.error('Error:', error)
      this.shoppingList = { error }
    }
  },

  methods: {
    async updateShoppingList() {
      console.log("Updating list")
      const { data: { data: shoppingLists } } = await axios.get('/api/v1/shopping-lists')
      this.shoppingList = shoppingLists.find(({ id }) => id == this.$route.params.id)
      console.log(this.shoppingList)
    },

    async tickCheckbox(item) {
      try {
        const response = await axios.put(
          `https://shoppinglist.wezeo.dev/cms/api/v1/shopping-lists/${this.$route.params.id}/items/${item.id}`,
          { is_checked: !item.is_checked }
        );
        item.is_checked = response.data.data.is_checked;
      } catch (error) {
        console.error('Checkbox error:', error.message, error.response?.data);
      }
    },
    async deleteList() {
      try {
        await axios.delete(
          `https://shoppinglist.wezeo.dev/cms/api/v1/shopping-lists/${this.$route.params.id}`
        );
        // return to main
        this.$router.push({ name: 'Shopping List - List' })
      } catch (error) {
        console.error('Deleting error:', error.message, error.response?.data);
      }
    },
    async newItem() {
      try {
        await axios.post(
          `https://shoppinglist.wezeo.dev/cms/api/v1/shopping-lists/${this.$route.params.id}/items`,
          {
            value: "1",
            is_checked: false,
            name: this.input,
            unit: "piece"
          }
        );
        this.updateShoppingList();
      } catch (error) {
        console.error('Adding new item error:', error.message, error.response?.data);
      }
    }
  }
}
</script>

<style>

</style>