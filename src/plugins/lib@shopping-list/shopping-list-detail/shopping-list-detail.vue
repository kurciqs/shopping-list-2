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
    try {
      this.updateShoppingList();
    } catch (error) {
      console.error('Error:', error)
      this.shoppingList = { error }
    }
  },

  methods: {
    async updateShoppingList() {
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

<style>/* Remove the default blue color and underlining */
a {
  color: inherit; /* Inherits text color from parent */
  text-decoration: none; /* Removes underline */
}

/* Optionally add a custom style */
a:hover {
  color: red; /* Change color on hover */
}

a:visited {
  color: gray; /* Set color for visited links */
}
/* Style for the main container */
body {
  font-family: 'Arial', sans-serif;
  margin: 0;
  padding: 20px;
  background-color: #f9f9f9;
  color: #333;
  line-height: 1.6;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  background: #ffffff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

/* Header styles */
h1 {
  font-size: 24px;
  color: #2c3e50;
  text-align: center;
  margin-bottom: 10px;
}

h2 {
  font-size: 18px;
  color: #34495e;
  text-align: center;
  margin-bottom: 20px;
}

/* List styles */
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

/* Checkbox styles */
input[type="checkbox"] {
  transform: scale(1.2);
  cursor: pointer;
}

/* Add hover effect */
ul li:hover {
  background-color: #f4f4f4;
}
/* Button styling */
button {
  background-color: #3498db; /* Primary color */
  color: #ffffff; /* White text */
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

button:hover {
  background-color: #2980b9; /* Darker blue on hover */
  transform: scale(1.05); /* Slight zoom effect */
}

button:active {
  background-color: #1c6ea4; /* Even darker blue when pressed */
  transform: scale(0.98); /* Slight compression effect */
}

/* Disabled button styling */
button:disabled {
  background-color: #bdc3c7; /* Gray background for disabled state */
  cursor: not-allowed;
}

/* Text input box styling */
input[type="text"] {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #ccc; /* Light gray border */
  border-radius: 5px;
  font-size: 16px;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

input[type="text"]:focus {
  border-color: #3498db; /* Blue border on focus */
  box-shadow: 0 0 5px rgba(52, 152, 219, 0.5); /* Blue glow */
  outline: none; /* Remove default focus outline */
}

</style>