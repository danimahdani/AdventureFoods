<template>
  <div class="detail">
    <Navbar />
    <div class="container">
      <!-- Breadcrumb -->
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
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
                Food Detail
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <!-- End Breadcrumb -->

      <div class="row mt-4">
        <div class="col-md-6">
          <img :src="'/img/' + product.gambar" class="img-fluid shadow" />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ product.nama }}</strong>
          </h2>
          <hr />
          <h4>
            Harga : <strong>{{ product.harga }}</strong>
          </h4>
          <form class="mt-3" v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah_pesan">Jumlah Pesan</label>
              <input
                type="number"
                id="jumlah_pesan"
                class="form-control"
                v-model="pesan.jumlahPesan"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                class="form-control"
                placeholder="Keterangan : Pedas, Nasi Setengah, ..."
                v-model="pesan.keterangan"
              ></textarea>
            </div>

            <button type="submit" class="btn btn-success" @click="pemesanan">
              <b-icon-cart></b-icon-cart> Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "FoodDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      product: [],
      pesan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
    pemesanan() {
      if (this.pesan.jumlahPesan) {
        this.pesan.product = this.product;
        axios
          .post("http://localhost:3000/keranjangs/", this.pesan)
          .then(() => {
            this.$toast.success("Sukses masuk keranjang", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            })
              this.$router.push({ path: "/keranjang" })
          })
          .catch((error) => console.log(error));
      } else {
        this.$toast.error("Jumlah pesanan harus diisi", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products/" + this.$route.params.id)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style>
.breadcrumb {
  background: transparent;
  padding: 0;
}

.img-fluid {
  border-radius: 15px;
}
</style>