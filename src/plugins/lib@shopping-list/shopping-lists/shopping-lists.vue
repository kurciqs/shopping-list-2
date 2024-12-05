<template>
	<h1>Lists</h1>

	<template v-if="!shoppingLists">
		<p>Načítavam dáta</p>
	</template>

	<template v-else-if="shoppingLists.error">
		Pri načítaní dát nastala chyba: {{ shoppingLists.error }}
	</template>

	<template v-else>
		Počet položiek v zozname: {{ shoppingLists.length }}
    <div v-for="list in shoppingLists" :key="list.id"> 
      <a :href="`/shopping-lists/${list.id}`" @click.prevent="openShoppingListDetail(list)">
      <h3> {{list.title}} </h3>
        <div v-for="item in list.items" :key="item.id">
          <p> {{item.name}}  ({{item.value}}) </p>
        </div>  
      </a>
    </div>
	</template>
</template>

<script>
import axios from 'axios'

export default {
	data() {
		return {
			shoppingLists: null
		}
	},

	async mounted() {
		try {
			const response = await axios.get('https://shoppinglist.wezeo.dev/cms/api/v1/shopping-lists/')
			console.log(response)
			const shoppingLists = response.data.data
			// const { data: { data: shoppingLists} } = await axios.get('/api/v1/shopping-lists')
			this.shoppingLists = shoppingLists
		} catch (error) {
			console.error('Error:', error)
			this.shoppingLists = { error }
		}
	},

  methods: {
		openShoppingListDetail({ id }) {
			this.$router.push({ name: 'Shopping List - Detail', params: { id } })
		}
	}
}
</script>
