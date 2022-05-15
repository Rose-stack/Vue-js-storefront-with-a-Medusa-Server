<template>
  <div class="mt-10 mb-10">
        <div class="row">
              <img
                v-for="image in product.images"
                :key="image.id"
                width="150"
                alt=""
                :src="image.url"
                class="cursor-pointer"
                @click="imageToShow = image.id"
              >
          </div>
        <div class="row">
          <div class="col-md-12 mb-10 mt-10">
                <h1 class="font-semibold text-3xl">
                {{ product.title }}
                </h1>
        
                <p class="font-light">
                {{ product.description }}
                </p>

                <p class="font-light">
                  From {{ lowestPrice.amount/100 }} {{ lowestPrice.currency_code.toUpperCase() }}
                </p>    
            </div>
        </div>
      </div>
</template>

<script>
import Axios from 'axios';
export default {
  
  name: 'ProductDetail',
  data () {
    return {
      showDetails: false,
      imageToShow: 'default_image',
      product: {
        id: 1,
        title: 'Medusa Coffee Mug',
        description: 'Every programmer\'s best friend.',
        thumbnail: '',
        variants: [{ prices: [{ amount: 0, currency_code: 'EUR' }] }],
        images: [
          { id: 'default_image', url: 'https://picsum.photos/600/400' },
          { id: 'another_image', url: 'https://picsum.photos/600/400?id=100' }
        ]
      }
    }
  },
  computed: {
    lowestPrice () {
      const lowestPrice = this.product.variants.reduce((acc, curr) => {
        return curr.prices.reduce((lowest, current) => {
          if (lowest.amount > current.amount) {
            return current
          }
          return lowest
        })
      }, { amount: 0 })

      return lowestPrice || { amount: 10, currency_code: 'usd' }
    },

  },
  async fetch () {
  try {
    const { data:{product} } = await Axios.get(`http://localhost:9000/store/products/${this.$route.params.id}`)
    this.product = product
    this.imageToShow = this.product.images[0].id
  } catch (e) {
    // eslint-disable-next-line no-console
    console.log('The server is not responding')
  }
}
}
</script>