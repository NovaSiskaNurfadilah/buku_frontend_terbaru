<template>
  <div>
    <h1 class="mb-4">PEMINJAMAN BUKU</h1>
    <h3>Form Peminjaman Buku</h3>
    <form @submit.prevent="submitPeminjaman">
      <div class="form-group">
        <label for="idPeminjaman">ID Peminjaman</label>
        <input type="text" class="form-control" v-model="pengembalian.idPeminjaman" id="idPeminjaman" required>
      </div>
      <div class="form-group">
        <label for="tanggal">Tanggal Peminjaman</label>
        <input type="date" class="form-control" v-model="peminjaman.tanggal" id="tanggal" required>
      </div>
      <div class="form-group">
        <label for="tanggal">Tanggal Pengembalian</label>
        <input type="date" class="form-control" v-model="pengembalian.tanggal" id="tanggal" required>
      </div>

      <div class="form-group">
        <label for="nomorAnggota">Nomor Anggota</label>
        <input type="text" class="form-control" v-model="peminjaman.nomorAnggota" id="nomorAnggota" required>
      </div>
      <div class="form-group">
        <label for="kodeBuku">Kode Buku</label>
        <input type="text" class="form-control" v-model="peminjaman.kodeBuku" id="kodeBuku" required>
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
    <h3>Form Pengembalian Buku</h3>
    <form @submit.prevent="submitPengembalian">
      <div class="form-group">
        <label for="idPeminjaman">ID Peminjaman</label>
        <input type="text" class="form-control" v-model="pengembalian.idPeminjaman" id="idPeminjaman" required>
      </div>
      <div class="form-group">
        <label for="tanggal">Tanggal Pengembalian</label>
        <input type="date" class="form-control" v-model="pengembalian.tanggal" id="tanggal" required>
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
    <input type="number" v-model="selectedPeminjaman" />
    <button @click="selectPeminjaman" type="submit" class="btn btn-primary">Pilih Peminjaman</button>
    <h3>List Peminjaman (Dipinjam)</h3>
    <table class="table table-blue margin-top: 30px">
      <thead>
      <tr>
        <th>ID</th>
        <th>Tanggal Peminjaman</th>
        <th>Nomor Anggota</th>
        <th>Kode Buku</th>
        <th>Status Peminjaman</th>
        <th>Tanggal Pengembalian</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="peminjaman in peminjamanDipinjam" :key="peminjaman.id">
        <td>{{ peminjaman.id }}</td>
        <td>{{ peminjaman.tanggal_peminjaman }}</td>
        <td>{{ peminjaman.nomor_anggota }}</td>
        <td>{{ peminjaman.kode_buku }}</td>
        <td>{{ peminjaman.status_peminjaman }}</td>
        <td>{{ peminjaman.tanggal_pengembalian }}</td>
      </tr>
      </tbody>
    </table>

    <h3>List Peminjaman (Dikembalikan)</h3>
    <table class="table table-blue">
      <thead>
      <tr>
        <th>ID</th>
        <th>Tanggal Peminjaman</th>
        <th>Nomor Anggota</th>
        <th>Kode Buku</th>
        <th>Status Peminjaman</th>
        <th>Tanggal Pengembalian</th>
        <th>Durasi Keterlambatan</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="peminjaman in peminjamanDikembalikan" :key="peminjaman.id">
        <td>{{ peminjaman.id }}</td>
        <td>{{ peminjaman.tanggal_peminjaman }}</td>
        <td>{{ peminjaman.nomor_anggota }}</td>
        <td>{{ peminjaman.kode_buku }}</td>
        <td>{{ peminjaman.status_peminjaman }}</td>
        <td>{{ peminjaman.tanggal_pengembalian }}</td>
        <td>{{ peminjaman.durasi_keterlambatan }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
.table-blue {
  background-color: #007bff; /* Warna latar belakang tabel */
  color: #000000; /* Warna teks tabel */
}

.table-blue thead th {
  background-color: #007bff; /* Warna latar belakang header kolom */
  color: #ffffff; /* Warna teks header kolom */
}

.table-blue tbody td {
  background-color: #ffffff; /* Warna latar belakang sel-sel isi */
  color: #000000; /* Warna teks sel-sel isi */
}
</style>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      peminjaman: {
        tanggal: '',
        nomorAnggota: '',
        kodeBuku: '',
      },
      pengembalian: {
        idPeminjaman: '',
        tanggalPengembalian: '',
      },
      peminjamanDipinjam: [],
      peminjamanDikembalikan: [],
    };
  },
  mounted() {
    // Mengambil data peminjaman dengan status DIPINJAM dari API backend PHP
    this.fetchPeminjamanDipinjam();
    // Mengambil data peminjaman dengan status DIKEMBALIKAN dari API backend PHP
    this.fetchPeminjamanDikembalikan();
    this.selectPeminjaman();
  },
  methods: {
    submitPeminjaman() {
      fetch('https://bukubackend--novasiskanurfad.repl.co/peminjaman-buku.php', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(this.peminjaman),
      })
          .then(response => response.json())
          .then(data => {
            console.log(data);
            // Lakukan tindakan setelah peminjaman berhasil disimpan
          })
          .catch(error => {
            console.error(error);
            // Lakukan tindakan jika terjadi kesalahan saat melakukan peminjaman
          });
    },
    submitPengembalian() {
        fetch('https://bukubackend--novasiskanurfad.repl.co/pengembalian-buku.php', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(this.pengembalian),
        })
            .then(response => response.json())
            .then(data => {
              console.log(data);
              // Lakukan tindakan setelah pengembalian berhasil disimpan
            })
            .catch(error => {
              console.error(error);
              // Lakukan tindakan jika terjadi kesalahan saat melakukan pengembalian
            });
      },
    selectPeminjaman() {
      try {
        const response = fetch(`https://bukubackend--novasiskanurfad.repl.co/select-peminjaman-berdasarkan-id.php${this.selectedNumber}`);
        if (response.ok) {
          const data = response.json();
          console.log(data);
        } else {
          console.error("Gagal memuat data idPeminjaman");
        }
      } catch (error) {
        console.error(error);
      }
    },

    fetchPeminjamanDipinjam() {
      // Panggil API backend PHP untuk mendapatkan data peminjaman dengan status DIPINJAM
      axios.get('https://bukubackend--novasiskanurfad.repl.co/status-dipinjam-list-peminjaman.php')
          .then((response) => {
            this.peminjamanDipinjam = response.data;
          })
          .catch((error) => {
            console.error(error);
          });
    },
    fetchPeminjamanDikembalikan() {
      // Panggil API backend PHP untuk mendapatkan data peminjaman dengan status DIKEMBALIKAN
      axios.get( 'https://bukubackend--novasiskanurfad.repl.co/status-dikembalikan-list-peminjaman.php')
          .then((response) => {
            this.peminjamanDikembalikan = response.data;
          })
          .catch((error) => {
            console.error(error);
          });
    },
  },
};
</script>