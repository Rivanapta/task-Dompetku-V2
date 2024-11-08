<template>
    <v-card class="mt-5">
      <v-card-title class="text-h6">Rekap Keuangan</v-card-title>
      <v-card-text>
        <v-list>
          <v-list-item v-for="(record, index) in records" :key="index">
            <v-list-item-content>
              <v-list-item-title>
                {{ record.description }} - {{ record.category }} - {{ record.type }}
              </v-list-item-title>
              <v-list-item-subtitle>Rp {{ record.amount.toLocaleString() }}</v-list-item-subtitle>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn icon @click="removeRecord(index)">
                <v-icon color="red">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-card-text>
      <v-card-actions>
        <v-btn color="red" @click="confirmClearAll = true">Hapus Semua Data</v-btn>
      </v-card-actions>
  
      <!-- Dialog konfirmasi hapus semua -->
      <v-dialog v-model="confirmClearAll" max-width="400px">
        <v-card>
          <v-card-title class="headline">Konfirmasi Hapus Semua</v-card-title>
          <v-card-text>
            Apakah Anda yakin ingin menghapus semua data?
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="green darken-1" text @click="confirmClearAll = false">Batal</v-btn>
            <v-btn color="red darken-1" text @click="clearAllRecords">Hapus</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-card>
  </template>
  
  <script>
  export default {
    props: {
      records: {
        type: Array,
        required: true,
      },
    },
    data() {
      return {
        confirmClearAll: false,
      };
    },
    methods: {
      removeRecord(index) {
        this.$emit('remove-record', index);
      },
      clearAllRecords() {
        this.confirmClearAll = false;
        this.$emit('clear-all-records');
      },
    },
  };
  </script>
  
  <style scoped>
  .v-card {
    padding: 20px;
  }
  </style>
  