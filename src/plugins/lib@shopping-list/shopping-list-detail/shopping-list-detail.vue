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
    <ul>
      <ShoppingListItem v-for="item in shoppingList.items" :item="item" :key="item.id"/>
    </ul>
    
    <button @click="deleteList(shoppingList)"> Zmazat zoznam </button>
    <button @click="addNewItem()"> Pridat polozku </button>
    <br>
    <input @keyup.enter="addNewItem" v-model="newItemName" placeholder="Add new item">
  </template>
</template>

<script>
import axios from 'axios'
import ShoppingListItem from '../shopping-list-item/shopping-list-item.vue'

export default {
  components: {
    ShoppingListItem
  },

  data() {
    return {
      shoppingList: null,
      newItemName: ''
    }
  },

  async mounted() {
    try {
      this.getShoppingList();
    } catch (error) {
      console.error('Error:', error)
      this.shoppingList = { error }
    }
  },

  methods: {
    async getShoppingList() {
      // const { data: { data: shoppingLists } } = await axios.get('/api/v1/shopping-lists') /*toto nejak nejde na vercel*/
      const response = await axios.get('https://shoppinglist.wezeo.dev/cms/api/v1/shopping-lists/')
      console.log(response)
      const shoppingLists = response.data.data
      this.shoppingList = shoppingLists.find(({ id }) => id == this.$route.params.id)
      console.log(this.shoppingList)
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
    async addNewItem() {
      try {
        const response = await axios.post(
          `https://shoppinglist.wezeo.dev/cms/api/v1/shopping-lists/${this.$route.params.id}/items`,
          {
            value: "1",
            is_checked: false,
            name: this.newItemName,
            unit: "piece"
          }
        );
        console.log(this.shoppingList)
        this.shoppingList.items.push(response.data.data)
      } catch (error) {
        console.error('Adding new item error:', error.message, error.response?.data);
      }
    }
  }
}
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}

ul li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 15px;
  border-bottom: 1px solid #ddd;
}

ul li:last-child {
  border-bottom: none;
}

ul li span {
  font-weight: bold;
  color: #2c3e50;
}

input[type="checkbox"] {
  transform: scale(1.2);
  cursor: pointer;
}

ul li:hover {
  background-color: #f4f4f4;
}

button {
  background-color: #3498db;
  color: #ffffff;
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

button:hover {
  background-color: #2980b9;
  transform: scale(1.05);
}

button:active {
  background-color: #1c6ea4;
  transform: scale(0.98);
}

button:disabled {
  background-color: #bdc3c7;
  cursor: not-allowed;
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

input[type="text"]:focus {
  border-color: #3498db;
  box-shadow: 0 0 5px rgba(52, 152, 219, 0.5);
  outline: none;
}
</style>
