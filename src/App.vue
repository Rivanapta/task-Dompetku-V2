<template>
  <v-app>
    <v-container>
      <v-row justify="center" class="mt-5">
        <v-col cols="12" md="8">
          <!-- Komponen Form untuk input data -->
          <formInput @add-record="addRecord" />

          <!-- Summary Dashboard -->
          <v-row class="mt-4" dense>
            <!-- Total Transaksi Card -->
            <v-col cols="12" sm="6" md="3">
              <v-card outlined color="primary lighten-5">
                <v-card-title class="justify-center">
                  <v-icon color="primary" large>mdi-format-list-bulleted</v-icon>
                </v-card-title>
                <v-card-subtitle class="text-center text-h6 font-weight-bold">Total Transaksi</v-card-subtitle>
                <v-card-text class="text-center text-h5 font-weight-bold">{{ records.length }}</v-card-text>
              </v-card>
            </v-col>

            <!-- Jumlah Masuk Card -->
            <v-col cols="12" sm="6" md="3">
              <v-card outlined color="green lighten-5">
                <v-card-title class="justify-center">
                  <v-icon color="green" large>mdi-cash-plus</v-icon>
                </v-card-title>
                <v-card-subtitle class="text-center text-h6 font-weight-bold">Jumlah Masuk</v-card-subtitle>
                <v-card-text class="text-center text-h5 font-weight-bold">Rp {{ totalIncome.toLocaleString() }}</v-card-text>
              </v-card>
            </v-col>

            <!-- Jumlah Keluar Card -->
            <v-col cols="12" sm="6" md="3">
              <v-card outlined color="red lighten-5">
                <v-card-title class="justify-center">
                  <v-icon color="red" large>mdi-cash-minus</v-icon>
                </v-card-title>
                <v-card-subtitle class="text-center text-h6 font-weight-bold">Jumlah Keluar</v-card-subtitle>
                <v-card-text class="text-center text-h5 font-weight-bold">Rp {{ totalExpense.toLocaleString() }}</v-card-text>
              </v-card>
            </v-col>

            <!-- Saldo Akhir Card -->
            <v-col cols="12" sm="6" md="3">
              <v-card outlined color="blue lighten-5">
                <v-card-title class="justify-center">
                  <v-icon color="blue" large>mdi-bank</v-icon>
                </v-card-title>
                <v-card-subtitle class="text-center text-h6 font-weight-bold">Saldo Akhir</v-card-subtitle>
                <v-card-text class="text-center text-h5 font-weight-bold">
                  Rp {{ balance.toLocaleString() }}
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>

          <!-- Komponen Rekap Keuangan untuk daftar transaksi -->
          <recordList :records="records" @remove-record="removeRecord" @clear-all-records="clearAllRecords" />
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import formInput from './components/FormInput.vue';
import recordList from './components/RecordList.vue';

export default {
  components: {
    formInput,
    recordList,
  },
  data() {
    return {
      records: [],
    };
  },
  computed: {
    totalIncome() {
      return this.records
        .filter((record) => record.type === 'Transaksi Masuk')
        .reduce((sum, record) => sum + record.amount, 0);
    },
    totalExpense() {
      return this.records
        .filter((record) => record.type === 'Transaksi Keluar')
        .reduce((sum, record) => sum + record.amount, 0);
    },
    balance() {
      return this.totalIncome - this.totalExpense;
    },
  },
  created() {
    this.loadFromLocalStorage();
  },
  methods: {
    addRecord(record) {
      this.records.push(record);
      this.saveToLocalStorage();
    },
    removeRecord(index) {
      this.records.splice(index, 1);
      this.saveToLocalStorage();
    },
    clearAllRecords() {
      this.records = [];
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      localStorage.setItem('records', JSON.stringify(this.records));
    },
    loadFromLocalStorage() {
      const savedRecords = localStorage.getItem('records');
      if (savedRecords) {
        this.records = JSON.parse(savedRecords);
      }
    },
  },
};
</script>

<style>
.v-application {
  background-color: #f5f5f5;
}
</style>
