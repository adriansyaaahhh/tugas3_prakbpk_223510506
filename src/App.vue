<template>
  <div class="app">
    <h1> Data Barang Rayens Clothing</h1>
    <!-- Tombol untuk menampilkan form tambah barang -->
    <button @click="showAddForm">Tambah Barang</button>

    <!-- Form untuk menambahkan barang baru (ditampilkan saat showFormAdd true) -->
    <div v-if="showFormAdd" class="form-barang">
      <h2>Tambah Barang</h2>
      <form @submit.prevent="Tambahbarang">
        <div class="form-group">
          <label for="name">Nama Barang:</label>
          <select v-model="newItem.name" required>++
            <option value="">Pilih Nama Barang</option>
            <option value="Sepatu">Sepatu</option>
            <option value="Baju Kaos">Baju Kaos</option>
            <option value="Kemeja">Kemeja</option>
            <option value="Celana Panjang">Celana Panjang</option>
            <option value="Jacket">Jacket</option>
          </select>
        </div>
        <div class="form-group">
          <label for="jumlah">Jumlah:</label>
          <input type="number" v-model.number="newItem.jumlah" required>
        </div>
        <div class="form-group">
          <label for="harga">Harga:</label>
          <input type="text" v-model="newItem.harga" required>
        </div>
        <div class="form-group">
          <label for="tanggalmasuk">Tanggal Pemasukan:</label>
          <input type="date" v-model="newItem.tanggalmasuk" required>
        </div>
        <div class="form-group">
          <label for="tanggalpengeluaran">Tanggal Keluar Barang:</label>
          <input type="date" v-model="newItem.tanggalpengeluaran">
        </div>
        <div class="form-group">
          <button type="button" @click="cancelAddForm">Batal</button>
          <button type="submit">Tambah</button>
        </div>
      </form>
    </div>

    <!-- Daftar barang -->
    <div class="daftarbarang">
      <h2>Daftar Barang</h2>
      <table>
        <thead>
          <tr>
            <th>Nama Barang</th>
            <th>Jumlah</th>
            <th>Harga</th>
            <th>Tanggal Pemasukan</th>
            <th>Tanggal Keluar</th>
            <th>Total</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in items" :key="index">
            <td>{{ item.name }}</td>
            <td>{{ item.jumlah }}</td>
            <td>{{ item.harga }}</td>
            <td>{{ item.tanggalmasuk }}</td>
            <td>{{ item.tanggalpengeluaran }}</td>
            <td>{{ getTotalharga(item) }}</td>
            <td>
              <button @click="editbrg(index)">Edit</button>
              <button @click="hapusbrg(index)">Hapus</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newItem: {
        name: '',
        jumlah: null,
        harga: '',
        tanggalmasuk: null,
        tanggalpengeluaran: null,
      },
      items: [],  
      showFormAdd: false,
    };
  },
  methods: {
    showAddForm() {
      this.showFormAdd = true;
    },
    cancelAddForm() {
      this.showFormAdd = false;
      this.resetForm();
    },
    Tambahbarang() {
      if (this.newItem.name.trim() !== '' && this.newItem.jumlah !== null && this.newItem.jumlah > 0 && /^\d+(\.\d{1,2})?$/.test(this.newItem.harga) && this.newItem.tanggalmasuk !== null) {
        this.items.push({ ...this.newItem });
        this.resetForm();
      } else {
        alert('Nama barang, jumlah, harga, dan tanggal pemasukan harus diisi. Harga harus berupa angka.');
      }
    },
    editbrg(index) {
      // Mendapatkan barang yang akan diedit
      const itemToEdit = this.items[index];
      
      // Mengisi kembali form dengan nilai barang yang akan diedit
      this.newItem = { ...itemToEdit };
      
      // Menghapus barang dari daftar barang
      this.items.splice(index, 1);
      
      // Menampilkan kembali form tambah barang
      this.showFormAdd = true;
    },
    hapusbrg(index) {
      // Menghapus barang dari daftar barang
      this.items.splice(index, 1);
    },
    resetForm() {
      this.newItem.name = '';
      this.newItem.jumlah = null;
      this.newItem.harga = '';
      this.newItem.tanggalmasuk = null;
      this.newItem.tanggalpengeluaran = null;
      this.showFormAdd = false; // Menyembunyikan form setelah reset
    },
    getTotalharga(item) {
      return item.jumlah * parseFloat(item.harga);
    },
  },
};
</script>
