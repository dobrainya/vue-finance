<template>
    <form class="px-3" @submit.prevent>
        <simple-dropdown
            v-model="record.category"
            :options="categoryList"
            class="border rounded-md border-slate-700 mb-4 focus:bg-slate-800"
        />
        <simple-input 
            v-model.trim="record.name" 
            type="text"
            placeholder="Name"
            class="border rounded-md border-slate-700 mb-4 focus:bg-slate-800"
        />

        <simple-input 
            v-model.number="record.value" 
            type="number"
            placeholder="Value"
            class="border rounded-md border-slate-700 mb-4 focus:bg-slate-800"
        />

        <simple-button
            @onclick="createItem"
            class="bg-green-800 hover:bg-green-900 mb-3 rounded-md"
        >
            Create
        </simple-button>
    </form>    
</template>

<script>
    import SimpleInput from '@/components/ui/SimpleInput.vue'
    import SimpleDropdown from '@/components/ui/SimpleDropdown.vue'
    import SimpleButton from '@/components/ui/SimpleButton.vue'

    export default {
        data() {
            return {
                categoryList: [
                    {name: 'Expences', value: 'expences'},
                    {name: 'Incomes', value: 'incomes'},
                ],
                record: {id: null, name: '', value: '', category: ''},
            };
        },
        components: {
            SimpleInput,
            SimpleDropdown,
            SimpleButton,
        },
        methods: {
            createItem() {
                this.record.id = Math.ceil(Math.random() * 100);
                this.$emit('create-item', this.record);   
                this.record.id = null;
                this.record.name = null;
                this.record.value = null;
                this.record.category = null; 
            },
        },
    };
</script>

<style>

</style>