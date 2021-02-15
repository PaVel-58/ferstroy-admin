<template>
    <b-card no-body class="border-0">
        <b-tabs pills card vertical class="box-size">
            <b-tab title="Tab 1" active @click="filter = ''">
                <b-container fluid>
                    <!-- User Interface controls -->
                    <b-row class="mb-4">
                        <b-col lg="6" class="my-1">
                            <b-form-group
                                class="mb-0"
                            >
                                <b-input-group size="sm">
                                    <b-form-input
                                        id="filter-input"
                                        v-model="filter"
                                        type="text"
                                        placeholder="Поиск по таблице"
                                    ></b-form-input>

                                    <b-input-group-append>
                                        <b-button :disabled="!filter" @click="filter = ''">Стереть</b-button>
                                    </b-input-group-append>
                                </b-input-group>
                            </b-form-group>
                        </b-col>
                    </b-row>
                    <b-row>
                        <b-col sm="5" md="1">
                            <b-form-group
                                label="По:"
                                label-for="per-page-select"
                                label-cols-sm="6"
                                label-cols-md="4"
                                label-cols-lg="3"
                                label-align-sm="right"
                                label-size="sm"
                                class="mb-0"
                            >
                                <b-form-select
                                    id="per-page-select"
                                    v-model="perPage"
                                    :options="pageOptions"
                                    align="center"
                                ></b-form-select>
                            </b-form-group>
                        </b-col>

                        <b-col sm="10">
                            <b-pagination
                                v-model="currentPage"
                                :total-rows="totalRows"
                                :per-page="perPage"
                                align="center"

                            ></b-pagination>
                        </b-col>
                    </b-row>

                    <!-- Main table element -->
                    <b-table
                        striped 
                        hover
                        :items="items"
                        :fields="fields"
                        :current-page="currentPage"
                        :per-page="perPage"
                        :filter="filter"
                        :filter-included-fields="filterOn"
                        :sort-by.sync="sortBy"
                        :sort-desc.sync="sortDesc"
                        :sort-direction="sortDirection"
                        :bordered="bordered"
                        stacked="md"
                        show-empty
                        small
                    >
                        <template #cell(name)="row">
                            {{ row.value }}
                        </template>

                        <template #cell(information)="row">
                            <div @click="info(row.item, row.index, $event.target)" class="infoBtn border-0 rounded-circle">
                                <img src="@/assets/images/png/infoIcon.png"/> 
                            </div>
                        </template>

                        <template #cell(history)="row">
                            <div style="cursor: pointer">{{row.value}}</div>
                        </template>

                        
                    </b-table>

                    <b-col>
                        <b-pagination
                            v-model="currentPage"
                            :total-rows="totalRows"
                            :per-page="perPage"
                            align="center"

                        ></b-pagination>
                    </b-col>

                    <!-- Info modal -->
                    <b-modal :id="infoModal.id" :title="infoModal.title" ok-only>
                        <pre>{{ infoModal.content }}</pre>
                    </b-modal>
                </b-container>
            </b-tab>
            <b-tab title="Tab 2" @click="filter = ''">
                content
            </b-tab>
            <b-tab title="Tab 3" @click="filter = ''">
                content
            </b-tab>
        </b-tabs>
    </b-card>
</template>

<script>
    export default {
        data() {
            return {
                items: [
                    { name: 'Anol Group', rating: 5, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'AkayCity', rating: 4, phone: '+998 99 985 45 65', history: '2016-2024' },
                    { name: 'Aparto', rating: 3, phone: '+998 95 943 56 72', history: '2016-2024' },
                    { name: 'Aristocrat Home', rating: 5, phone: '+998 95 847 34 23', history: '2016-2024' },
                    { name: 'Arjun', rating: 3, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'Asklepiy svift', rating: 5, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'Asser Stroy Servis', rating: 5, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'BB-Stroy', rating: 2, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'Murad Buildings', rating: 5, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'Bizning Uylar Development', rating: 5, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'Akay City', rating: 4, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'NOVASTROY', rating: 2, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'GENESYS', rating: 5, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'Dream House', rating: 3, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'Real House', rating: 5, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'First Development Group', rating: 5, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'NRG', rating: 5, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'Future House', rating: 1, phone: '+998 90 987 43 21', history: '2016-2024' },
                    { name: 'Golden House', rating: 5, phone: '+998 90 987 43 21', history: '2016-2024' }
                ],
                fields: [
                    { key: 'name', label: 'Имя', sortable: true },
                    { key: 'rating', label: 'Рейтинг', sortable: true, class: 'text-center' },
                    { key: 'phone', label: 'Контакт', sortable: false, class: 'text-center' },
                    { key: 'information', label: 'Допю информация', sortable: false, class: 'text-center' },
                    { key: 'history', label: 'История', sortable: false, class: 'text-center' },
                ],
                totalRows: 100,
                currentPage: 1,
                perPage: 5,
                pageOptions: [5, 10, 20, { value: 100, text: "Show a lot" }],
                sortBy: '',
                sortDesc: false,
                sortDirection: 'asc',
                filter: null,
                filterOn: [],
                infoModal: {
                    id: 'info-modal',
                    title: 'Golden House',
                    content: ''
                }
            }
        },
        computed: {
            sortOptions() {
                // Create an options list from our fields
                return this.fields
                .filter(f => f.sortable)
                .map(f => {
                    return { text: f.label, value: f.key }
                })
            }
        },
        mounted() {
            // Set the initial number of items
            this.totalRows = this.items.length
        },
        methods: {
            info(item, index, button) {
                this.infoModal.title = `${item.name}`
                this.infoModal.content = JSON.stringify(item, null, 2)
                this.$root.$emit('bv::show::modal', this.infoModal.id, button)
            },
            resetInfoModal() {
                this.infoModal.title = ''
                this.infoModal.content = ''
            },
            onFiltered(filteredItems) {
                // Trigger pagination to update the number of buttons/pages due to filtering
                this.totalRows = filteredItems.length
                this.currentPage = 1
            }
        }
    }
</script>

<style>
    div button, div input {
        outline: none;
        box-shadow: none !important;
    }
    .box-size {
        height: 100vh;
    }
    div .tableTitle {
        outline: none;
        cursor: pointer;
    }
    table#table-transition-example .flip-list-move {
       transition: transform 1s;
    }
    .infoBtn {
        max-width: 50px;
        width: 100%;
        font-size: 0; 
        margin: 0 auto;
        cursor: pointer;
    }
    .infoBtn img {
        width: 100%;
        transform: rotate(180deg);
    }
    div .table th, .table td {
        vertical-align: inherit;
    }
</style>