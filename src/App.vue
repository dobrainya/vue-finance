<template>
    <simple-modal
        v-model:visible="showExpenseRemoveConfirmation"
    >
        <svg class="mx-auto mb-4 text-gray-400 w-12 h-12 dark:text-gray-200" aria-hidden="true"
             xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M10 11V6m0 8h.01M19 10a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/>
        </svg>
        <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Are you sure you want to delete this
            expence?</h3>

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
        <svg class="mx-auto mb-4 text-gray-400 w-12 h-12 dark:text-gray-200" aria-hidden="true"
             xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M10 11V6m0 8h.01M19 10a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/>
        </svg>
        <h3 class="mb-5 text-lg font-normal text-gray-500 dark:text-gray-400">Are you sure you want to delete this
            income?</h3>

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
                    :references="references"
                    :is-loading="formIsProcessing"
                />
            </div>
            <div class="basis-1/3">
                <ItemsList
                    title="expenses"
                    :records="expenses"
                    class="mx-3"
                    @remove-item="removeExpense"
                    @save-item="saveAmount"
                    :is-loading="expensesIsLoading"
                >
                </ItemsList>
            </div>
            <div class="basis-1/3">
                <ItemsList
                    title="Incomes"
                    :records="incomes"
                    class="mx-3"
                    @remove-item="removeIncome"
                    @save-item="saveAmount"
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
            expenses: [],
            incomes: [],
            references: [],
            showExpenseRemoveConfirmation: false,
            expenseToRemove: null,
            showIncomeRemoveConfirmation: false,
            incomeToRemove: null,
            expensesIsLoading: false,
            incomesIsLoading: false,
            formIsProcessing: false,
        };
    },
    methods: {
        async createItem(record, successCallback) {
            try {
                if (record.type === '') {
                    throw Error('Select category');
                }

                if (record.name === '') {
                    throw Error('Please, specify the name');
                }

                if (record.amount === '' || record.amount <= 0) {
                    throw Error('Amount can\'t be empty or 0');
                }
                this.formIsProcessing = true;
                const response = await axios.post('http://app.local/api/v1/amount/', record, {
                    headers: {
                        'content-type': 'application/x-www-form-urlencoded'
                    },
                });

                this[response.data.item.type === 'inc' ? 'incomes' : 'expenses'].push(response.data.item);

                successCallback(true);
            } catch (e) {
                alert(e.message);
                successCallback(false);
            } finally {
                this.formIsProcessing = false;
            }
        },
        removeExpense(record) {
            this.expenseToRemove = record;
            this.showExpenseRemoveConfirmation = true;
        },
        removeIncome(record) {
            this.incomeToRemove = record;
            this.showIncomeRemoveConfirmation = true;
        },
        confirmRemove() {
            if (this.expenseToRemove) {
                this.expenses = this.expenses.filter(i => i.id !== this.expenseToRemove.id);
            } else if (this.incomeToRemove) {
                this.incomes = this.incomes.filter(i => i.id !== this.incomeToRemove.id);
            }

            this.resetConfirmation();
        },
        resetConfirmation() {
            this.expenseToRemove = null;
            this.showExpenseRemoveConfirmation = false;
            this.incomeToRemove = null;
            this.showIncomeRemoveConfirmation = false;
        },
        async fetchReferences() {
            try {
                this.formIsProcessing = true;
                const response = await axios.get('http://app.local/api/v1/refs');
                this.references = response.data.items;
            } catch (e) {
                alert('Ошибка');
            } finally {
                this.formIsProcessing = false;
            }
        },
        async fetchExpenses() {
            try {
                this.expensesIsLoading = true;
                const response = await axios.get('http://app.local/api/v1/amount/exp', {
                    params: {
                        _limit: 10,
                    },
                });

                this.expenses = response.data.items;
            } catch (e) {
                alert('Ошибка');
            } finally {
                this.expensesIsLoading = false;
            }
        },
        async fetchIncomes() {
            try {
                this.incomesIsLoading = true;
                const response = await axios.get('http://app.local/api/v1/amount/inc', {
                    params: {
                        _limit: 10,
                    },
                });

                this.incomes = response.data.items;
            } catch (e) {
                alert('Ошибка');
            } finally {
                this.incomesIsLoading = false;
            }
        },
        async saveAmount(record, loaderCallback) {
            try {
                loaderCallback(true);
                const response = await axios.post(`http://app.local/api/v1/amount/${record.id}`, record, {
                    headers: {
                        'content-type': 'application/x-www-form-urlencoded',
                    },
                });
            } catch (e) {
                alert(e.message);
            } finally {
                loaderCallback(false);
            }
        },
    },
    components: {ItemsList, AddForm, SimpleButton, SimpleModal},
    mounted() {
        this.fetchReferences();
        this.fetchExpenses();
        this.fetchIncomes();
    },
    watch: {
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
