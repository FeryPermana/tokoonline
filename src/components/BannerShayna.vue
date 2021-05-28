<template>
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-12 mt-5" v-if="products.length > 0">
          <carousel
            class="product-slider"
            :nav="false"
            :autoplay="true"
            :dots="false"
          >
            <div
              class="product-item"
              v-for="itemProduct in products"
              v-bind:key="itemProduct.id"
            >
              <div class="pi-pic">
                <img v-bind:src="itemProduct.galleries[0].photo" alt />
                <ul
                  @click="
                    saveKeranjang(
                      itemProduct.id,
                      itemProduct.name,
                      itemProduct.price,
                      itemProduct.galleries[0].photo
                    )
                  "
                >
                  <li class="w-icon active">
                    <a href="#">
                      <i class="icon_bag_alt"></i>
                    </a>
                  </li>
                  <li class="quick-view">
                    <router-link v-bind:to="'/product/' + itemProduct.id"
                      >+ Quick View</router-link
                    >
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{ itemProduct.type }}</div>
                <router-link to="/product">
                  <h5>{{ itemProduct.name }}</h5>
                </router-link>
                <div class="product-price">
                  ${{ itemProduct.price }}
                  <span>$35.00</span>
                </div>
              </div>
            </div>
          </carousel>
        </div>
        <div class="col-lg-12" v-else></div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.product-item {
  margin-right: 25px;
}

.product-item img {
  max-height: 455px;
}
</style>

<script>
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
  name: "BannerShayna",
  components: {
    carousel,
  },
  data() {
    return {
      products: [],
      keranjangUser: [],
    };
  },
  methods: {
    saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {
      var productStorage = {
        id: idProduct,
        name: nameProduct,
        price: priceProduct,
        photo: photoProduct,
      };

      this.keranjangUser.push(productStorage);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);

      window.location.reload();
    },
  },
  mounted() {
    if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjangUser");
      }
    }
    axios
      .get("http://localhost:8000/api/products")
      .then((res) => (this.products = res.data.data.data));
  },
};
</script>
