<template>
    <input
        class="block w-full p-3 bg-inherit focus:ring-inset focus:ring-slate-700 text-slate-50"
        :type="type" 
        :value="modelValue"
        :placeholder="placeholder"
        @input="updateInput"
    />
</template>

<script>
    export default {
        name: 'simple-input',
        props: {
            type: {
                type: String,
                required: true,
            },
            placeholder: String,
            modelValue: [String, Number],
        },
        data() {
            return {

            };
        },
        methods: {
            updateInput(event) {
                this.$emit('update:modelValue', event.target.value);
            },
        },
    };
</script>

<style>

</style>