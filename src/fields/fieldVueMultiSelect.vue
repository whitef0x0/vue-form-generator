<template lang="jade">
	multiselect(		
		:id="schema.selectOptions.id",
		:options="options",
		:multiple="schema.multiSelect",
		:selected="value",
		:key="schema.selectOptions.key || null",
		:label="schema.selectOptions.label || null",
		:searchable="schema.selectOptions.searchable",
		:local-search="schema.selectOptions.localSearch",
		:clear-on-select="schema.selectOptions.clearOnSelect",
		:hide-selected="schema.selectOptions.hideSelected",
		:placeholder="schema.placeholder",
		:max-height="schema.selectOptions.maxHeight",
		:allow-empty="schema.selectOptions.allowEmpty",
		:reset-after="schema.selectOptions.resetAfter",
		:close-on-select="schema.selectOptions.closeOnSelect",
		:custom-label="schema.selectOptions.customLabel || null",
		:taggable="schema.selectOptions.taggable",
		:tag-placeholder="schema.selectOptions.tagPlaceholder",
		:max="schema.max || null",
		@update="updateSelected",
		@tag="addTag",
		@select="onSelect",
		@remove="onRemove",
		@search-change="onSearchChange",
		@open="onOpen",
		@close="onClose",		
		:select-label="schema.selectOptions.selectLabel",
		:selected-label="schema.selectOptions.selectedLabel",
		:deselect-label="schema.selectOptions.deselectLabel",
		:show-labels="schema.selectOptions.showLabels",
		:limit="schema.selectOptions.limit",
		:limit-text="schema.selectOptions.limitText",
		:loading="schema.selectOptions.loading",
		:disabled="disabled",
		:option-partial="schema.selectOptions.optionPartial",
		:show-pointer="schema.selectOptions.showPointer",
		:option-height="schema.selectOptions.optionHeight"
	)
</template>
<script>
	import Vue from "vue";
	import abstractField from "./abstractField";

	export default {
		mixins: [abstractField],
		computed: {
			options() {
				let values = this.schema.values;
				if (typeof(values) == "function") {
					return values.apply(this, [this.model, this.schema]);
				} else {
					return values;
				}
			}
		},
		methods: {
			updateSelected(value/*, id*/) {
				this.value = value;
			},
			addTag(newTag, id) {
				let onNewTag = this.schema.selectOptions.onNewTag;
				if (typeof(onNewTag) == "function") {
					onNewTag(newTag, id, this.options, this.value);
				}
			},
			onSearchChange(searchQuery, id) {
				let onSearch = this.schema.selectOptions.onSearch;
				if (typeof(onSearch) == "function") {
					onSearch(searchQuery, id, this.options);
				}
			},
			onSelect(/*selectedOption, id*/) {
				// console.log("onSelect", selectedOption, id);
			},
			onRemove(/*removedOption, id*/) {
				// console.log("onRemove", removedOption, id);
			},
			onOpen(/*id*/) {
				// console.log("onOpen", id);
			},
			onClose(/*value, id*/) {
				// console.log("onClose", value, id);
			}
		},
		created() {
			// Check if the component is loaded
			if (window.VueMultiselect) {
				Vue.component("multiselect", window.VueMultiselect.default);
			} else {
				console.error("'vue-multiselect' is missing. Please download from https://github.com/monterail/vue-multiselect and load the script in the HTML head section!");
			}
		}
	};
</script>

