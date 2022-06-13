<template>
  <div id="product">
    <SfBreadcrumbs
      class="breadcrumbs desktop-only"
      :breadcrumbs="breadcrumbs"
    />

    <p v-if="$fetchState.pending">Fetching Data...</p>
    <p v-else-if="$fetchState.error">An error occurred :(</p>

    <div v-else class="product">
      <SfGallery
        :images="this.getImages"
        :image-width="422"
        :image-height="664"
        :thumb-width="160"
        :thumb-height="160"
        :sliderOptions="{ autoplay: 3000, rewind: true, gap: 0 }"
        :enableZoom="true"
      />

      <div class="product__info">
        <div class="product__header">
          <SfHeading
            :title="product.title"
            :level="1"
            class="sf-heading--no-underline sf-heading--left"
          />

          <SfIcon
            icon="drag"
            size="42px"
            color="#E0E0E1"
            class="product__drag-icon smartphone-only"
          />
        </div>

        <div class="product__price-and-rating">
          <SfPrice :regular="this.lowestPrice.amount" />
        </div>

        <div>
          <p class="product__description desktop-only">
            {{ this.product.description }}
          </p>

          <SfAddToCart
            v-model="qty"
            class="product__add-to-cart"
            @click="addToCart"
          />
        </div>
      </div>
    </div>

    <transition name="slide">
      <SfNotification
        class="notification desktop-only"
        type="success"
        :visible="isOpenNotification"
        :message="`${qty} ${product.title} has been added to CART`"
        @click:close="isOpenNotification = true"
      >
        <template #icon>
          <span></span>
        </template>
      </SfNotification>
    </transition>
  </div>
</template>

<script>
import {
  // UI components
  SfGallery,
  SfHeading,
  SfPrice,
  SfIcon,
  SfAddToCart,
  SfBreadcrumbs,
  SfNotification,
} from "@storefront-ui/vue";
import Axios from "axios";
export default {
  name: "Product",
  components: {
    SfGallery,
    SfHeading,
    SfPrice,
    SfIcon,
    SfAddToCart,
    SfBreadcrumbs,
    SfNotification,
  },
  data() {
    // default data
    return {
      current: 1,
      qty: 1,
      selected: true,
      product: {
        name: "",
        title: "",
        description: "",
        images: [],
        price: {
          regular: 0,
        },
      },
      breadcrumbs: [
        {
          text: "Home",
          link: "/",
        },
      ],
      isOpenNotification: false,
    };
  },
  computed: {
    getImages() {
      // Format Product images in a way that the component understands.
      return this.product
        ? this.product.images.map((image) => {
            return {
              mobile: {
                url: image.url,
              },
              desktop: {
                url: image.url,
              },
              big: {
                url: image.url,
              },
              alt: this.product?.title,
              name: this.product?.title,
            };
          })
        : [];
    },

    lowestPrice() {
      // Get the least price
      const lowestPrice = this.product.variants
        ? this.product.variants.reduce(
            (acc, curr) => {
              return curr.prices.reduce((lowest, current) => {
                if (lowest.amount > current.amount) {
                  return current;
                }
                return lowest;
              });
            },
            { amount: 0 }
          )
        : { amount: 0 };
      // Format the amount and append the currency.
      return {
        amount:
          lowestPrice.amount > 0
            ? (lowestPrice.amount / 100).toLocaleString("en-US", {
                style: "currency",
                currency: "USD",
              })
            : 0,
        currency: "USD",
      };
    },
  },
  methods: {
    addToCart() {
      this.isOpenNotification = true;// show notification
      
      setTimeout(() => {
        this.isOpenNotification = false; // hide notification
      }, 3000);
    },
  },
  async fetch() {
    // Fetch the product based on the id.
    try {
      const {
        data: { product },
      } = await Axios.get( `${process.env.baseUrl}/store/products/${this.$route.params.id}`
      
      );
      this.product = product;
    } catch (e) {
      // eslint-disable-next-line no-console
      console.log("The server is not responding");
    }
  },
};
</script>

</script>

<style lang="scss" scoped>
@import "~@storefront-ui/vue/styles";
#product {
  box-sizing: border-box;
  @include for-desktop {
    max-width: 1272px;
    padding: 0 var(--spacer-sm);
    margin: 0 auto;
  }
}
.product {
  margin-bottom: 20px;
  @include for-desktop {
    display: flex;
  }
  &__info {
    margin: var(--spacer-xs) auto;
    @include for-desktop {
      max-width: 32.625rem;
      margin: 0 0 0 7.5rem;
    }
  }
  &__header {
    --heading-title-color: var(--c-link);
    --heading-title-font-weight: var(--font-weight--bold);
    --heading-title-font-size: var(--h3-font-size);
    --heading-padding: 0;
    margin: 0 var(--spacer-sm);
    display: flex;
    justify-content: space-between;
    @include for-desktop {
      --heading-title-font-weight: var(--font-weight--semibold);
      margin: 0 auto;
    }
  }
  &__drag-icon {
    animation: moveicon 1s ease-in-out infinite;
  }
  &__price-and-rating {
    margin: 0 var(--spacer-sm) var(--spacer-base);
    align-items: center;
    @include for-desktop {
      display: flex;
      justify-content: space-between;
      margin: var(--spacer-sm) 0 var(--spacer-lg) 0;
    }
  }
  &__count {
    @include font(
      --count-font,
      var(--font-weight--normal),
      var(--font-size--sm),
      1.4,
      var(--font-family--secondary)
    );
    color: var(--c-text);
    text-decoration: none;
    margin: 0 0 0 var(--spacer-xs);
  }
  &__description {
    color: var(--c-link);
    @include font(
      --product-description-font,
      var(--font-weight--light),
      var(--font-size--base),
      1.6,
      var(--font-family--primary)
    );
  }
  &__add-to-cart {
    margin: var(--spacer-base) var(--spacer-sm) 0;
    @include for-desktop {
      margin-top: var(--spacer-2xl);
    }
  }
  &__guide,
  &__compare,
  &__save {
    display: block;
    margin: var(--spacer-xl) 0 var(--spacer-base) auto;
  }
  &__compare {
    margin-top: 0;
  }
  &__property {
    margin: var(--spacer-base) 0;
    &__button {
      --button-font-size: var(--font-size--base);
    }
  }
  &__additional-info {
    color: var(--c-link);
    @include font(
      --additional-info-font,
      var(--font-weight--light),
      var(--font-size--sm),
      1.6,
      var(--font-family--primary)
    );
    &__title {
      font-weight: var(--font-weight--normal);
      font-size: var(--font-size--base);
      margin: 0 0 var(--spacer-sm);
      &:not(:first-child) {
        margin-top: 3.5rem;
      }
    }
    &__paragraph {
      margin: 0;
    }
  }
  &__gallery {
    flex: 1;
  }
}
.breadcrumbs {
  margin: var(--spacer-base) auto var(--spacer-lg);
}
.notification {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  --notification-border-radius: 0;
  --notification-max-width: 100%;
  --notification-font-size: var(--font-size--lg);
  --notification-font-family: var(--font-family--primary);
  --notification-font-weight: var(--font-weight--normal);
  --notification-padding: var(--spacer-base) var(--spacer-lg);
}

.slide-enter-active,
.slide-leave-active {
  transition: all 0.3s;
}

.slide-enter {
  transform: translateY(40px);
}

.slide-leave-to {
  transform: translateY(-80px);
}
@keyframes moveicon {
  0% {
    transform: translate3d(0, 0, 0);
  }
  50% {
    transform: translate3d(0, 30%, 0);
  }
  100% {
    transform: translate3d(0, 0, 0);
  }
}
</style>
