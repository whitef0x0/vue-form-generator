<template lang="jade">
    .wrapper
        validator(name="validator")
            input(v-validate="schema.validation",type="color", v-model="value", :disabled="disabled")
        span.helper {{ value }}
</template>

<script>
    import abstractField from "./abstractField";
    export default {
        mixins: [ abstractField ]
    };
</script>


<style lang="sass">
    .vue-form-generator .field-color {

        input {
            width: 60px;
        }

        .helper {
            margin-left: 0.3em;
        }
    }
</style>
