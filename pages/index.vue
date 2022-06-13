<template>
  <div>
    <div class="row">
      <div class="col-md-12">
        <SfHero class="hero" :slider-options="{ autoplay: false }">
          <SfHeroItem
            v-for="(img, index) in heroes"
            :key="index"
            :title="img.title"
            :subtitle="img.subtitle"
            :button-text="img.buttonText"
            :background="img.background"
            :class="img.className"
          />
        </SfHero>
      </div>
      <div class="col-md-12">
        <h4 class="text-center mt-5 mb-5">All Products</h4>
      </div>
    </div>

    <div v-if="products.length">
      <div class="row mb-5">
        <ProductCard
          v-for="product in products"
          :key="product.id"
          :item="product"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Axios from "axios";
import { SfHero } from "@storefront-ui/vue";
export default {
  name: "ProductsIndex",
  components: {
    SfHero,
  },
  data() {
    return {
      products: [],
      heroes: [
        {
          title: "Colorful T-shirts already in store",
          subtitle: "JUNE COLLECTION 2022",
          buttonText: "Learn more",
          background: "rgb(236, 239, 241)",
        },
        {
          title: "Colorful Sweatshirts already in store",
          subtitle: "JUNE COLLECTION 2022",
          buttonText: "Learn more",
          background: "rgb(239, 235, 233)",
        },
        {
          title: "Colorful Sweatpants already in store",
          subtitle: "JUNE COLLECTION 2022",
          buttonText: "Learn more",
          background: "rgb(236, 239, 241)",
        },
      ],
    };
  },
  async fetch() {
    // Fetching the products from Medusa server
    try {
      const {
        data: { products },
      } = await Axios.get(`${process.env.baseUrl}/store/products`);
      this.products = products;
    } catch (e) {
      console.log("An error occured", e);
    }
  },
};
</script>
