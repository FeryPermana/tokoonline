<template>
  <div class="product">
    <HeaderShayna />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i>Home</router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="gambar_default" alt="" />
                </div>
                <div
                  class="product-thumbs"
                  v-if="productDetails.galleries.length > 0"
                >
                  <carousel
                    class="product-thumbs-track ps-slider"
                    :nav="false"
                    :dots="true"
                  >
                    <div
                      v-for="ss in productDetails.galleries"
                      :key="ss.id"
                      class="pt"
                      @click="changeImage(ss.photo)"
                      :class="ss.photo == gambar_default ? 'active' : ''"
                    >
                      <img :src="ss.photo" alt />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ productDetails.type }}</span>
                    <h3>{{ productDetails.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <p v-html="productDetails.description"></p>
                    <h4>${{ productDetails.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart">
                      <a
                        @click="
                          saveKeranjang(
                            productDetails.id,
                            productDetails.name,
                            productDetails.price,
                            productDetails.galleries[0].photo
                          )
                        "
                        href="#"
                        class="primary-btn pd-cart"
                        >add To Cart</a
                      >
                    </router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <RelatedProducts />
    <!-- Product Shop Section End -->
    <FooterShayna />
  </div>
</template>

<style scoped>
.product-thumbs .pt {
  margin-right: 7px;
  margin-left: 7px;
}
</style>

<script>
// @ is an alias to /src

import HeaderShayna from "@/components/HeaderShayna";
import FooterShayna from "@/components/FooterShayna";
import carousel from "vue-owl-carousel";
import RelatedProducts from "@/components/RelatedProducts";
import axios from "axios";

export default {
  name: "Product",
  components: {
    HeaderShayna,
    FooterShayna,
    carousel,
    RelatedProducts,
  },
  data() {
    return {
      gambar_default: "",
      // idProduct: this.$route.params.id
      productDetails: [],
      keranjangUser: [],
    };
  },
  methods: {
    changeImage(urlImage) {
      this.gambar_default = urlImage;
      //   console.log(this.idProduct);
    },
    setDataPicture(data) {
      this.productDetails = data;
      this.gambar_default = data.galleries[0].photo;
    },
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
    },
  },
  mounted() {
    axios
      .get("http://localhost:8000/api/products", {
        params: {
          id: this.$route.params.id,
        },
      })
      .then((res) => this.setDataPicture(res.data.data))
      .catch((err) => console.log(err));
  },
};
</script>
