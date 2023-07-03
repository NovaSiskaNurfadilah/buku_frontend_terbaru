<template>
  <div>
    <input type="number" v-model="selectedNumber" />
    <button @click="selectMember">Pilih Anggota</button>

    <h3>Tambah Anggota Baru</h3>
    <div>
      <input type="text" v-model="newMember.nomor" placeholder="Nomor" />
      <input type="text" v-model="newMember.nama" placeholder="Nama" />
      <input type="text" v-model="newMember.jenis_kelamin" placeholder="Jenis Kelamin" />
      <input type="text" v-model="newMember.alamat" placeholder="Alamat" />
      <input type="text" v-model="newMember.no_hp" placeholder="No. HP" />
      <input type="text" v-model="newMember.tanggal_terdaftar" placeholder="Tanggal Terdaftar" />
      <button @click="addMember">Tambah</button>
    </div>

    <h3>Update Anggota</h3>
    <div>
      <input type="text" v-model="updateMember.nomor" placeholder="Nomor" />
      <input type="text" v-model="updateMember.nama" placeholder="Nama" />
      <input type="text" v-model="updateMember.jenis_kelamin" placeholder="Jenis Kelamin" />
      <input type="text" v-model="updateMember.alamat" placeholder="Alamat" />
      <input type="text" v-model="updateMember.no_hp" placeholder="No. HP" />
      <input type="text" v-model="updateMember.tanggal_terdaftar" placeholder="Tanggal Terdaftar" />
      <button @click="updateMemberData">Update</button>
    </div>

    <h3>Hapus Anggota</h3>
    <div>
      <input type="number" v-model="deleteMemberNumber" placeholder="Nomor" />
      <button @click="deleteMember">Hapus</button>
    </div>

    <h3>Semua Anggota</h3>
    <table class="table">
      <tr>
        <th>ID</th>
        <th>Nomor</th>
        <th>Nama</th>
        <th>Jenis Kelamin</th>
        <th>Alamat</th>
        <th>No. HP</th>
        <th>Tanggal Terdaftar</th>
      </tr>
      <tr v-for="member in members" :key="member.id">
        <td>{{ member.id }}</td>
        <td>{{ member.nomor }}</td>
        <td>{{ member.nama }}</td>
        <td>{{ member.jenis_kelamin }}</td>
        <td>{{ member.alamat }}</td>
        <td>{{ member.no_hp }}</td>
        <td>{{ member.tanggal_terdaftar }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedNumber: null,
      deleteMemberNumber: null,
      members: [],
      newMember: {
        nomor: "",
        nama: "",
        jenis_kelamin: "",
        alamat: "",
        no_hp: "",
        tanggal_terdaftar: ""
      },
      updateMember: {
        nomor: "",
        nama: "",
        jenis_kelamin: "",
        alamat: "",
        no_hp: "",
        tanggal_terdaftar: ""
      }
    };
  },
  mounted() {
    this.fetchMembers();
  },
  methods: {
    async fetchMembers() {
      try {
        const response = await fetch("https://bukubackend--novasiskanurfad.repl.co/select-anggota.php");
        if (response.ok) {
          const data = await response.json();
          this.members = data;
        } else {
          console.error("Gagal memuat data anggota");
        }
      } catch (error) {
        console.error(error);
      }
    },
    async selectMember() {
      try {
        const response = await fetch(`https://bukubackend--novasiskanurfad.repl.co/select-nomor-anggota.php?nomor=${this.selectedNumber}`);
        if (response.ok) {
          const data = await response.json();
          console.log(data);
        } else {
          console.error("Gagal memuat data anggota");
        }
      } catch (error) {
        console.error(error);
      }
    },
    async addMember() {
      try {
        const response = await fetch("https://bukubackend--novasiskanurfad.repl.co/insert-anggota.php", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(this.newMember)
        });
        if (response.ok) {
          console.log("Anggota baru ditambahkan");
          this.fetchMembers();
        } else {
          console.error("Gagal menambahkan anggota baru");
        }
      } catch (error) {
        console.error(error);
      }
    },
    async updateMemberData() {
      try {
        const response = await fetch(`https://bukubackend--novasiskanurfad.repl.co/update-nomor-anggota.php?nomor=${this.updateMember.nomor}`, {
          method: "PUT",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(this.updateMember)
        });
        if (response.ok) {
          console.log("Data anggota diperbarui");
          this.fetchMembers();
        } else {
          console.error("Gagal memperbarui data anggota");
        }
      } catch (error) {
        console.error(error);
      }
    },
    async deleteMember() {
      try {
        const response = await fetch(`/api/members/${this.deleteMemberNumber}`, {
          method: "DELETE"
        });
        if (response.ok) {
          console.log("Anggota dihapus");
          this.fetchMembers();
        } else {
          console.error("Gagal menghapus anggota");
        }
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>
