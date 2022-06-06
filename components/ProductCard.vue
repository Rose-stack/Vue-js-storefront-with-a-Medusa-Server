<template>
  <SfProductCard
    :image="item.thumbnail"
    :imageWidth="216"
    :imageHeight="326"
    badgeLabel=""
    badgeColor=""
    :title="item.title"
    :link="this.url"
    :linkTag="item.id"
    :scoreRating="4"
    :reviewsCount="7"
    :maxRating="5"
    :regularPrice="this.highestPrice.amount"
    :specialPrice="this.lowestPrice.amount"
    wishlistIcon="heart"
    isInWishlistIcon="heart_fill"
    :isInWishlist="false"
    showAddToCartButton
    :isAddedToCart="false"
    :addToCartDisabled="false"
  />
</template>

<script>
import { SfProductCard } from "@storefront-ui/vue"; // Import the components
export default {
  name: "ProductCard",
  components: {
    SfProductCard,
  },
  props: {
    item: {
      type: Object,
    },
  },
  computed: {
    url() {
      return `/products/${this.item.id}`; // Product page
    },
    lowestPrice() {
      // Get the lowest price from the list of prices.
      const lowestPrice = this.item.variants.reduce(
        (acc, curr) => {
          return curr.prices.reduce((lowest, current) => {
            if (lowest.amount > current.amount) {
              return current;
            }
            return lowest;
          });
        },
        { amount: 0 }
      );
      // Format the amount and also add currency
      return {
        amount:
          lowestPrice.amount > 0
            ? (lowestPrice.amount / 100).toLocaleString("en-US", {
                style: "currency",
                currency: "USD",
              })
            : 0,
        currency_code: "USD",
      };
    },
    highestPrice() {
      // Get the highest price from the list of prices
      const highestPrice = this.item.variants.reduce(
        (acc, curr) => {
          return curr.prices.reduce((highest, current) => {
            if (highest.amount < current.amount) {
              return current;
            }
            return highest;
          });
        },
        { amount: 0 }
      );

      // Format the amount and also add currency
      return {
        amount:
          highestPrice.amount > 0
            ? (highestPrice.amount / 100).toLocaleString("en-US", {
                style: "currency",
                currency: "USD",
              })
            : 0,
        currency_code: "USD",
      };
    },
  },
};
</script>
