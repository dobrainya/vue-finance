<template>
    <div class="flex flex-row hover:text-slate-50">
        <div class="basis-3/4">
            <div :class="{'flex': true, 'bg-slate-800': mode === 'edit'}">
                <div class="flex-auto w-3/4 p-3" v-if="mode !== 'edit'">{{ record.name }}</div>
                <input 
                    v-else
                    class="flex-auto w-3/4 p-3 bg-inherit border-none focus:ring-inset focus:ring-slate-700"
                    type="text" 
                    v-model="record.name"
                />
                <div class="flex-auto w-1/4 p-3" v-if="mode !== 'edit'">{{ record.value }}</div>
                <input 
                    v-else
                    class="flex-auto w-1/4 p-3 bg-inherit border-none focus:ring-inset focus:ring-slate-700"
                    type="text" 
                    v-model="record.value"
                />
            </div>
        </div>
        <div class="basis-1/4">
            <div class="flex divide-x divide-slate-800">
                <button
                    v-if="mode !== 'edit'"
                    class="flex-auto w-1/2 p-3 mx-2 hover:text-green-400"
                    @click="editItem" 
                    type="button"
                >
                    Edit
                </button>
                <button
                    v-if="mode == 'edit'"
                    class="flex-auto w-1/2 p-3 mx-2 hover:text-green-400"
                    @click="saveItem" 
                    type="button"
                >
                    Save
                </button>
                <button
                    :disabled="mode === 'edit'"
                    :class="{'text-slate-900': mode === 'edit', 'flex-auto w-1/2 p-3 mx-2': true, 'hover:text-red-400': mode !== 'edit'}"
                    @click="removeItem" 
                    type="button"
                >
                    Remove
                </button>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            record: Object,
        },
        data() {
            return {
                mode: 'normal',
            }
        },
        methods: {
            editItem() {
               this.mode = 'edit'; 
            },
            saveItem() {
                this.mode = 'normal';
            },
            removeItem() {
                this.$emit('remove-item', this.record);
            },
        },
    }
</script>

<style>

</style>