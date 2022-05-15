<template>
<div>
  <div class="row">
    <div class="col-md-12">
      <h4 class="text-center mt-5 mb-5">All Products</h4>
    </div>
  </div>    
    <div
      v-if="products.length"
    >
    <div class="row">
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
import Axios from 'axios';
export default {
  name: 'ProductsIndex',
    data () {
    return {
      products: [{
        id: 1,
        title: 'Dummy Product',
        thumbnail: 'https://picsum.photos/600/600',
        variants: [{ prices: [{ amount: 100, currency_code: 'eur' }] }]
      }]
    }
  },
  async fetch(){
        try{  
          const {data:{products}} =  await Axios.get('http://localhost:9000/store/products');
          this.products = products
        }catch(e){
          console.log('An error occured', e)
        }
      }
}
</script>