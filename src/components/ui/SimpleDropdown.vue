<template>
    <select
        class="block w-full p-3 bg-inherit focus:ring-inset focus:ring-slate-700 text-slate-50" 
        v-model="selected"
    >
        <option 
            v-for="option in options" 
            :value="option.value" 
            :key="option.value"
        >
            {{ option.name }}
        </option>
    </select>
</template>

<script>
    export default {
        name: 'simple-dropdown',
        props: {
            options: {
                type: Array,
                required: true,
                default: () => [],
            },
            modelValue: [String, Number],
        },
        data() {
            return {
            };
        },
        methods: {
            changeSelect(event) {
                 this.$emit('update:modelValue', event.target.value);
            },
        },
        computed: {
            selected: {
                get() {
                    return this.modelValue;
                },
                set(value) {
                    this.$emit('update:modelValue', event.target.value);
                },
            },
        },

    };
</script>

<style>

</style>