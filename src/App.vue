<template>
    <simple-modal
        v-model:visible="showExpenceRemoveConfirmation"
    >
        <svg class="mx-auto mb-4 text-gray-400 w-12 h-12 dark:text-gray-200" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 11V6m0 8h.01M19 10a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/>
        </svg>
        <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Are you sure you want to delete this expence?</h3>

        <simple-button
            @onclick="confirmRemove"
            class="text-white bg-red-600 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 dark:focus:ring-red-800 font-medium rounded-lg text-sm inline-flex items-center px-5 py-2.5 text-center mr-2"
        >
            Yes
        </simple-button>
        <simple-button
            @onclick="resetConfirmation"
            class="text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-gray-200 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10 dark:bg-gray-700 dark:text-gray-300 dark:border-gray-500 dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-gray-600"
        >
            No
        </simple-button>
    </simple-modal>
    <simple-modal
        v-model:visible="showIncomeRemoveConfirmation"
    >
        <svg class="mx-auto mb-4 text-gray-400 w-12 h-12 dark:text-gray-200" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 11V6m0 8h.01M19 10a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/>
        </svg>
        <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Are you sure you want to delete this income?</h3>

        <simple-button
            @onclick="confirmRemove"
            class="text-white bg-red-600 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 dark:focus:ring-red-800 font-medium rounded-lg text-sm inline-flex items-center px-5 py-2.5 text-center mr-2"
        >
            Yes
        </simple-button>
        <simple-button
            @onclick="resetConfirmation"
            class="text-gray-500 bg-white hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-gray-200 rounded-lg border border-gray-200 text-sm font-medium px-5 py-2.5 hover:text-gray-900 focus:z-10 dark:bg-gray-700 dark:text-gray-300 dark:border-gray-500 dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-gray-600"
        >
            No
        </simple-button>
    </simple-modal>
    <div class="">
        <div class="bg-slate-900 m-0 text-3xl text-slate-50 text-bold p-6">Finance</div>
            <div class="flex flex-wrap">
                <div class="basis-1/3">
                    <add-form 
                        @create-item="createItem"
                    />
                </div>
                <div class="basis-1/3">
                    <ItemsList 
                        title="Expences" 
                        :records="expences"
                        class="mx-3"
                        @remove-item="removeExpence"
                        :is-loading="expencesIsLoading"
                        >
                    </ItemsList>
                </div>
                <div class="basis-1/3">
                    <ItemsList 
                        title="Incomes" 
                        :records="incomes"
                        class="mx-3"
                        @remove-item="removeIncome"
                        :is-loading="incomesIsLoading"
                        >
                    </ItemsList>
                </div>
            </div>
    </div>
</template>

<script>
    import ItemsList from '@/components/ItemsList.vue';
    import AddForm from '@/components/AddForm.vue';
    import SimpleModal from '@/components/ui/SimpleModal.vue';
    import SimpleButton from '@/components/ui/SimpleButton.vue';
    import axios from 'axios';

    export default {
        data() {
            return {
                expences: [],
                incomes: [],
                // expences: new Set([
                //     {id: 1, name: "Expence 1", value: 10.00},
                //     {id: 2, name: "Expence 2", value: 6.00},
                //     {id: 3, name: "Expence 3", value: 7.80},
                //     {id: 4, name: "Expence 4", value: 2.10},
                // ]),
                // incomes: new Set([
                //     {id: 1, name: "incomes 1", value: 100.00},
                //     {id: 2, name: "incomes 2", value: 60.00},
                //     {id: 3, name: "incomes 3", value: 17.80},
                //     {id: 4, name: "incomes 4", value: 21.10},
                // ]),
                showExpenceRemoveConfirmation: false,
                expenceToRemove: null,
                showIncomeRemoveConfirmation: false,
                incomeToRemove: null,
                expencesIsLoading: false,
                incomesIsLoading: false,
            };
        },
        methods: {
            createItem(record) {
                this[record.category].push({
                    id: record.id,
                    name: record.name,
                    value: record.value,
                });
            },
            removeExpence(record) {
                this.expenceToRemove = record;
                this.showExpenceRemoveConfirmation = true;
            },
            removeIncome(record) {
                this.incomeToRemove = record;
                this.showIncomeRemoveConfirmation = true;
            },
            confirmRemove() {
                if (this.expenceToRemove) {
                    this.expences = this.expences.filter(i => i.id !== this.expenceToRemove.id);
                } else if (this.incomeToRemove) {
                    this.incomes = this.incomes.filter(i => i.id !== this.incomeToRemove.id);
                }

                this.resetConfirmation();
            },
            resetConfirmation() {
                this.expenceToRemove = null;
                this.showExpenceRemoveConfirmation = false;
                this.incomeToRemove = null;
                this.showIncomeRemoveConfirmation = false;
            },
            async fetchExpences() {
                try {
                    this.expencesIsLoading = true;
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                        params: {
                            _limit: 10,
                        },
                    });

                    this.expences = response.data.map(i => {
                        i.value = Math.ceil(Math.random() * 100 * 1.00);
                        i.name = i.title;

                        return i;
                    });
                } catch (e) {
                    alert('Ошибка');
                } finally {
                    this.expencesIsLoading = false;
                }
            },
            async fetchIncomes() {
                try {
                    this.incomesIsLoading = true;
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                        params: {
                            _limit: 10,
                        },
                    });

                    this.incomes = response.data.map(i => {
                        i.value = Math.ceil(Math.random() * 100 * 1.00);
                        i.name = i.title;

                        return i;
                    });
                } catch (e) {
                    alert('Ошибка');
                } finally {
                    this.incomesIsLoading = false;
                }
            },
        },
        components: { ItemsList, AddForm, SimpleButton, SimpleModal },
        mounted() {
            this.fetchExpences();
            this.fetchIncomes();
        },
    }
</script>

<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }
    
</style>