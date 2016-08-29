<template lang="jade">
	validator(name="validator")
	    textarea.form-control(v-validate="schema.validation", v-model="value", :rows="schema.rows || 2", :readonly="schema.readonly", :placeholder="schema.placeholder", :disabled="disabled", :maxlength="schema.max")
</template>

<script>
	import abstractField from "./abstractField";

	export default {
		mixins: [ abstractField ]
	};
	
</script>


<style lang="sass">
</style>
