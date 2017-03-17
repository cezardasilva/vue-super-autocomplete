<style scoped lang="scss">
.autocomplete{

	$border-color: #e1e6ed;
	$lightgray: #c5c9d0;

	position: relative;

	ul.dropdown-suggestions{
		-webkit-transition: all .2s ease;
		-moz-transition: all .2s ease;
		-ms-transition: all .2s ease;
		-o-transition: all .2s ease;
		transition: all .2s ease;

		position: absolute;

		visibility: hidden;
		opacity: 0;
		top: -100%;
		left: 0;

		background-color: #FFF;
		border: 2px solid $border-color;
		border-radius: 2px;
		z-index: 10;

		list-style: none;
		padding: 0;
		margin: 0;

		li{
			font-size: 0.813em;
			color: $lightgray;

			a{
				display: block;
				height: 100%;
				padding: 15px 8px;
			}

			&:hover{
				background: transparentize($lightgray, 0.5);
			}
		}
	}

	&.open{
		.dropdown-suggestions{
			top: 100%;
			visibility: visible;
			opacity: 1;
		}
	}
}
</style>

<template>
	<div class="autocomplete" :class="{open: openSuggestion}">
		<input type="text" v-model="selection" maxlength="150" @input='change' @blur='blur' @focus="focus()"/>
		<ul class="dropdown-suggestions">
			<li v-for="(suggestion, index) in matches" :class="{active: isActive(index)}" @click="suggestionClick(index)">
				<a href="javascript:void(0);">{{suggestion[searchByIndex]}}</a>
			</li>
			<template v-if="openNotFound">
				<li class="empty">{{notFoundText}}</li>
				<li class="empty"><a href="javascript:void(0);" @click="newClick()">{{newItemText}}</a></li>
			</template>
		</ul>
	</div>
</template>

<script>

export default {
	name: "SuperAutocomplete",

	data(){
		return {
			open: false,
			current: 0,
			selection: ""
		}
	},

	props:{
		suggestions: {
			type: Array,
			required: true
		},
		searchByIndex: {
			type: String,
			required: true
		},
		notFoundText: {
			type: String,
			required: false,
			default: "Not found."
		},
		newItemText: {
			type: String,
			required: false,
			default: "It's a new item"
		}
	},

	computed: {
		matches(){
			return this.suggestions.filter((obj) => {
				if (obj[this.searchByIndex].search(new RegExp(this.selection, 'gi')) !== -1) {
					return obj
				}
			})
		},

		openSuggestion(){
			return this.open === true
		},

		openNotFound(){
			return this.selection !== "" &&
			this.matches.length === 0 &&
			this.open === true
		}
	},
	methods: {
		enter() {
			this.selection = this.matches[this.current];
			this.open = false;
		},

		//For highlighting element
		isActive(index) {
			return index === this.current;
		},

		//When the user changes input
		change() {
			if (this.open == false) {
				this.open = true;
				this.current = 0;
			}
		},

		blur(){
			if (this.selection.length > 0 && this.matches.length <= 0) {
				this.$emit("newItemClick", this.selection)
			}
		},

		focus(){
			this.open = true
		},

		//When one of the suggestion is clicked
		suggestionClick(index) {
			this.$emit("suggestionClick", this.matches[index])
			this.selection = this.matches[index][this.searchByIndex]
			this.open = false;
		},

		newClick(){
			this.open = false;
			this.$emit("newItemClick", this.selection)
		}
	}
}
</script>
