<template lang="jade">
	validator(name="validator")
	    input.form-control(:field="schema.model",v-validate="schema.validation",type="text", v-model="value", :maxlength="schema.max", :readonly="schema.readonly", :disabled="disabled", :placeholder="schema.placeholder")
</template>

<script>
	import abstractField from "./abstractField";

	export default {
		mixins: [ abstractField ]
	};
	
</script>

<style lang="sass">
</style>
