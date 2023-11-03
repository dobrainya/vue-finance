<template>
    <select
        class="block w-full p-3 bg-inherit focus:ring-inset focus:ring-slate-700 text-slate-50" 
        v-model="selected"
    >
        <option
            v-for="option in options"
            :value="option.code"
            :key="option.code"
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
        computed: {
            selected: {
                get() {
                    return this.modelValue;
                },
                set(value) {
                    this.$emit('update:modelValue', value);
                },
            },
        },

    };
</script>

<style>

</style>
