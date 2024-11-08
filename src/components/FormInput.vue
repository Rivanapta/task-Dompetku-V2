<template>
    <v-card>
      <v-card-title class="text-h5">Dompetku - Expense & Income Money Management</v-card-title>
      <v-card-text>
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-text-field v-model="description" label="Keterangan" required></v-text-field>
  
          <!-- Dropdown untuk memilih jenis transaksi -->
          <v-select v-model="type" :items="types" label="Pilih Jenis Transaksi" outlined required></v-select>
  
          <!-- Dropdown untuk memilih kategori transaksi -->
          <v-select v-model="category" :items="categories" label="Kategori" outlined required></v-select>
  
          <v-text-field v-model="amount" label="Jumlah Uang" type="number" required></v-text-field>
  
          <v-btn color="primary" @click="handleSubmit">Submit</v-btn>
        </v-form>
      </v-card-text>
    </v-card>
  </template>
  
  <script>
  export default {
    data() {
      return {
        description: '',
        type: null,
        types: ['Transaksi Masuk', 'Transaksi Keluar'],
        category: null,
        categories: ['Food', 'Transport', 'Shopping', 'Bills', 'Others'],
        amount: null,
        valid: true,
      };
    },
    methods: {
      handleSubmit() {
        if (this.description && this.amount && this.type && this.category) {
          this.$emit('add-record', {
            description: this.description,
            amount: parseInt(this.amount),
            type: this.type,
            category: this.category,
          });
          // Reset form setelah submit
          this.description = '';
          this.amount = null;
          this.type = null;
          this.category = null;
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .v-btn {
    margin-top: 10px;
  }
  </style>
  