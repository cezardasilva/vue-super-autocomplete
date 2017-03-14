<template>
	<div class="autocomplete-demo">
		<super-autocomplete :suggestions="suggestions" :search-by-index="'name'" @suggestionClick="selected($event)" @newItemClick="newItem($event)"></super-autocomplete>

		<div class="demo-selected">
			<h1>Item selected</h1>
			<code>
				{{itemSelected}}
			</code>
		</div>
	</div>
</template>
<script>

import SuperAutocomplete from './components/SuperAutocomplete.vue'

export default {
	name: "AutocompleteDemo",
	components: {
		SuperAutocomplete
	},
	data(){
		return {
			suggestions: [
				{
					id: 10,
					name: "Brazil"
				},
				{
					id: 91,
					name: "United States of America"
				},
				{
					id: 203,
					name: "Chile"
				},
				{
					id: 12,
					name: "Canada"
				},
			],
			itemSelected: null
		}
	},

	methods: {
		selected(e){
			console.log(e);
			this.itemSelected = e
		},
		newItem(e){
			this.itemSelected = `New Item: ${e}`
		}
	}
}
</script>
