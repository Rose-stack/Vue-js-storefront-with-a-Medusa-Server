<template>
<div class="col-12 col-md-3 mt-3 mb-3">
    <div class="card">
        <nuxt-link :to="`/products/${item.id}`">
            <img class="card-img-top" :src="item.thumbnail" style="height:150px" alt="Card image cap">
            <div class="card-body">
                <h5 class="card-title">{{item.title}}</h5>
                <p class="card-text">From {{ lowestPrice.amount }} {{ lowestPrice.currency_code }} </p>
            </div>
        </nuxt-link>
    </div>
</div>  
</template>
<script>
export default {
    name:"ProductCard",
    props: {
        item: {
            type: Object,
            // default () {
            //     return {
            //         id: 1,
            //         title: 'Dummy Product',
            //         thumbnail: 'https://picsum.photos/600/600',
            //         variants: [{ prices: [{ amount: 0 }] }]
            //     }
            // }
        }
    },
    computed:{
        lowestPrice(){
            const lowestPrice = this.item.variants.reduce((acc, curr) => {
                return curr.prices.reduce((lowest, current) => {
                    if (lowest.amount > current.amount) {
                        return current
                    }
                    return lowest
                })
            }, { amount: 0 });
            return lowestPrice || { amount: 10, currency_code: 'EUR' };
        }
    }

}
</script>