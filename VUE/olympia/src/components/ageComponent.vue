<template>
    <h1>Avarage Age per Country :</h1>
    <div class="col-sm">
        <DataTable class="table-hover table-striped" :data="avgAgeData">
            <thead>
                <tr>
                    <th v-for="c in ['country', 'Team', 'age']">{{ c }}</th>
                </tr>
            </thead>
        </DataTable>
    </div>
</template>

<script>
import { VuePlotly } from 'vue3-plotly';
import DataTable from 'datatables.net-vue3'
import DataTablesLib from 'datatables.net-bs5';
import axios from 'axios';

DataTable.use(DataTablesLib);

export default {
    data() {
        return {
            avgAgeData: []
        }
    },
    methods: {
        async getAvgAgeData() {
            await axios.get(`http://127.0.0.1:5000/avg_age`)
                .then(result => this.avgAgeData = result.data)
        }
    },
    components: {
        VuePlotly,
        DataTable
    },
    mounted() {
        this.getAvgAgeData()
    }
}

</script>

<style>
table {
    width: 100% !important;
}
</style>