<template>
    <button
        type="button"
        class="p-3 focus:ring-0 focus:ring-slate-700 text-slate-50"
        @click="clickBtn"
    >
        <slot></slot>
    </button>
</template>

<script>
    export default {
        name: 'simple-button',
        props: {
            
        },
        data() {
            return {

            };
        },
        methods: {
            clickBtn(event) {
                this.$emit('onclick', event);
            },
        },
    };
</script>

<style>

</style>