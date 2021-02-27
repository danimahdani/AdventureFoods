<template>
  <div class="keranjang">
    <!-- Kirim data keranjang ke navbar untuk mengupdate keranjang navbar -->
    <Navbar :updateKeranjang="keranjangs"/>
    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb mt-3">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Foods</router-link>
              </li>
              <li
                class="breadcrumb-item active font-weight-bold"
                aria-current="page"
              >
                Keranjang Pesanan
              </li>
            </ol>
          </nav>
        </div>
        <div class="table-responsive">
          <table class="table">
            <thead>
              <tr>
                <th scope="col">#</th>
                <th scope="col">Foto</th>
                <th scope="col">Makanan</th>
                <th scope="col">Keterangan</th>
                <td scope="col">Jumlah</td>
                <th scope="col">Harga</th>
                <th scope="col">Total Harga</th>
                <th scope="col">Hapus</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(keranjang, index) in keranjangs" :key="keranjang.id">
                <td>{{ index + 1 }}</td>
                <td>
                  <img
                    :src="'/img/' + keranjang.product.gambar"
                    class="img-fluid shadow"
                    width="250"
                  />
                </td>
                <td>{{ keranjang.product.nama }}</td>
                <td>{{ keranjang.keterangan ? keranjang.keterangan : "-" }}</td>
                <td>{{ keranjang.jumlahPesan }}</td>
                <td align="right">Rp. {{ keranjang.product.harga }}</td>
                <td align="right">
                  Rp.
                  <strong>{{
                    keranjang.jumlahPesan * keranjang.product.harga
                  }}</strong>
                </td>
                <td align="center" class="text-danger">
                  <b-icon-trash
                    @click="hapusKeranjang(keranjang.id)"
                  ></b-icon-trash>
                </td>
              </tr>

              <tr>
                <td colspan="6" align="right">
                  <strong>Grand Total :</strong>
                </td>
                <td align="right">
                  Rp .<strong>{{ setGrandTotal }}</strong>
                </td>
                <td></td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
    };
  },
  methods: {
    setKeranjans(data) {
      this.keranjangs = data;
    },
    hapusKeranjang(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          this.$toast.error("Sukses Hapus Keranjang", {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });

          //Update data ketika sudah dihapus
          axios
            .get("http://localhost:3000/keranjangs")
            .then((response) => this.setKeranjans(response.data))
            .catch((error) => console.log(error));
        })
        .catch((error) => console.log(error));
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setKeranjans(response.data))
      .catch((error) => console.log(error));
  },
  computed: {
    setGrandTotal() {
      return this.keranjangs.reduce(function (items, data) {
        return items + (data.product.harga * data.jumlahPesan);
      }, 0);
    },
  },
};
</script>

<style>
</style>