<template>
    <div v-if="medalls[0].x.length > 0">
        <div class="row" style="padding:20px">
            <div class="col-sm">
                <VuePlotly :layout="layoutMBC" :data="medalls"></VuePlotly>
            </div>
            <div class="col-sm">
                <DataTable class="table-hover table-striped" width="100%" :data="medallsData">
                    <thead>
                        <tr>
                            <th v-for="m in ['Medalls', 'Count']">{{ m }}</th>
                        </tr>
                    </thead>
                </DataTable>
            </div>
        </div>
        <div class ="col-sm">
            <VuePlotly :layout="layoutMBC" :data="medalls"></VuePlotly>
        </div>
        <div class="row" style="padding:20px">
            <div class="col-sm">
                <VuePlotly :layout="layoutMBS2" :data="medallsbySeason"></VuePlotly>
            </div>
    </div>
    </div>
    <div v-else style="padding:20px">
        <h1>This Country has won 0 medalls</h1>
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
            medalls: [{ x: [] }],
            medallsData: [],
            medallsbySex: [],
            medallsbySeason: [],
            layoutMBS: { title: "Medalls by Sex" },
            layoutMBS2: { title: "Goldmedals per Season"},
            layoutMBC: { title: "Medalls by Country" }
        }
    },
    methods: {
        async getMedalls() {
            await axios.get(`http://127.0.0.1:5000/medals2/${this.currentNoc}`)
                .then(result => this.medalls = result.data)
        },
        async getMedallsData() {
            await axios.get(`http://127.0.0.1:5000/medals/${this.currentNoc}`)
                .then(result => this.medallsData = result.data)
        },
        async getMedallsbySex() {
            await axios.get(`http://127.0.0.1:5000/count_by_sex2`)
                .then(result => this.medallsbySex = result.data)
        },
        async getMedallsbySeason() {
            await axios.get(`http://127.0.0.1:5000//goldMedalls_per_NOC/${this.currentNoc}`)
                .then(result => this.medallsbySeason = result.data)
        }
    },
    components: {
        VuePlotly,
        DataTable
    },
    props: {
        currentEvent: {
            default: "None",
            required: true
        },
        currentNoc: {
            default: "AUT",
            required: true
        }
    }, watch: {
        currentNoc() {
            this.getMedalls()
            this.getMedallsData()
            this.getMedallsbySex()
            this.getMedallsbySeason()
        }
    }, mounted() {
        this.getMedalls()
        this.getMedallsData()
        this.getMedallsbySex()
        this.getMedallsbySeason()
    }

}

</script>