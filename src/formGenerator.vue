<template lang="jade">
.form-group.input-field(v-for="field in fields", v-if="fieldVisible(field)", :class="getFieldRowClasses(field)")
	h5.secondary-title {{ field.label }}
	.field-wrap
		component(:is="getFieldType(field)", :disabled="fieldDisabled(field)", :model.sync="model", :schema.sync="field")
		.buttons(v-if="field.buttons && field.buttons.length > 0")
			button(v-for="btn in field.buttons", @click="btn.onclick(model, field)", :class="btn.classes") {{ btn.label }}
	.hint(v-if="field.hint") {{ field.hint }}
	.errors(v-if="field.errors && field.errors.length > 0")
		span(v-for="error in field.errors", track-by="$index") {{ error }}
</template>

<script>
	import Vue from "vue";
	//import Joi from "joi";
	import {each, isFunction, isNil, isArray, isString} from "lodash";

	// Load all fields from '../fields' folder
	let Fields = require.context("./fields/", false, /^\.\/field([\w-_]+)\.vue$/);
	let fieldComponents = {};
	each(Fields.keys(), (key) => {
		let compName = Vue.util.classify(key.replace(/^\.\//, "").replace(/\.vue/, ""));
		fieldComponents[compName] = Fields(key);
	});


	export default {
		components: fieldComponents,
		
		props: [
			"schema",
			"options",
			"model",
			"multiple",
			"isNewModel"
		],
		
		data () {
			return {
				errors: [] // Validation errors
			};
		},

		computed: {
			fields() {
				let res = [];
				if (this.schema) {
					each(this.schema.fields, (field) => {
						if (!this.multiple || field.multi === true)
							res.push(field);
					});
				}

				return res;
			}
		},
	
		methods: {
			getFieldRowClasses(field) {
				let baseClasses = {
					error: field.errors && field.errors.length > 0, 
					disabled: this.fieldDisabled(field), 
					readonly: field.readonly, 
					featured: field.featured, 
					required: field.required
				};

				if (isArray(field.styleClasses)) {
					each(field.styleClasses, (c) => baseClasses[c] = true);
				}
				else if (isString(field.styleClasses)) {
					baseClasses[field.styleClasses] = true;
				}

				baseClasses["field-" + field.type] = true;

				return baseClasses;
			},

			getFieldType(fieldSchema) {
				return "field-" + fieldSchema.type;
			},

			fieldDisabled(field) {
				if (isFunction(field.disabled))
					return field.disabled(this.model);

				if (isNil(field.disabled))
					return false;

				return field.disabled;
			},

			fieldVisible(field) {
				if (isFunction(field.visible))
					return field.visible(this.model);

				if (isNil(field.visible))
					return true;

				return field.visible;
			}
		}
	};
	
</script>

<style lang="sass">

</style>