<template>
  <div>
    <b-navbar toggleable="lg" type="light">
      <div class="container">
        <b-navbar-brand href="#">Adventure Foods</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <li class="nav-item">
              <router-link to="/" class="nav-link">Home</router-link>
            </li>
            <li class="nav-item">
              <router-link to="/foods" class="nav-link">Foods</router-link>
            </li>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <b-nav-form>
             <li class="nav-item">
              <router-link to="/keranjang" class="nav-link">
                Keranjang
                <b-icon-bag></b-icon-bag>
                <div class="badge badge-success ml-2">{{ updateKeranjang ? updateKeranjang.length : jumlah_pesanan.length }}</div>
              </router-link>
              </li>
            </b-nav-form>
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Navbar",
  props: ["updateKeranjang"],
  data() {
    return {
      jumlah_pesanan: [],
    };
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => (this.jumlah_pesanan = response.data))
      .catch((error) => console.log(error));
  },
};
</script>