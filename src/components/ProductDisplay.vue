<template>
    <div class="container">
        <div v-if="isShow" class="card">
            <div class="product-container"></div>
            <div class="detail">
                <div class="detail-top"></div>
                <div class="detail-bottom"></div>
            </div>
        </div>
        <div v-else class="container" :class="!productShow ? 'bg-shadow-gray' : product.data.category === 'men\'s clothing' ? 'bg-pale-aqua' : 'bg-ping'">
            <div class="overlay">
                <img src="" alt="">
            </div>
            <div class="card">
                <div v-if="!productShow" class="product-unavailable">
                    <div class="overlay">
                        <img src="" alt="">
                    </div>
                    <div class="detail-product">
                        <p>This product is unavailable to show</p>
                        <div class="animation">
                            <button type="button" @click="theProduct()" :class="product.data.category === 'men\'s clothing' ? 'border-dark-sapphire' : 'border-byzantium font-byzantium'" class="button-next" >Next Product</button>
                        </div>
                    </div>
                </div>
                <div v-else class="product-container">
                    <div class="thumbnail">
                        <img :src="product.data.image">
                    </div>
                    <div class="detail-product">
                        <div class="top">
                            <h2 :class="product.data.category === 'men\'s clothing' ? 'font-dark-sapphire' : 'font-byzantium'" class="title">{{ product.data.title }}</h2>
                            <div class="sub-title">
                                <div class="rate">
                                    <span></span>
                                    <span></span>
                                    <span></span>
                                    <span></span>
                                    <span></span>
                                    <span></span>
                                </div>
                            </div>
                        </div>
                        <div class="description">
                            <p>{{ product.data.description }}</p>
                        </div>
                    </div>
                    <div class="bottom">
                        <span :class="product.data.category === 'men\'s clothing' ? 'font-dark-sapphire' : 'font-byzantium'" class="price">${{ product.data.price }}</span>
                        <div class="animation">
                            <button type="button" :class="product.data.category === 'men\'s clothing' ? 'bg-dark-sapphire' : 'bg-byzantium'" class="animation-buy">Buy Now</button>
                            <button type="button" :class="product.data.category === 'men\'s clothing' ? 'border-dark-sapphire font-dark-sapphire' : 'border- byzantium font-byzantium'" class="animation-next">Next Product</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ProductDisplay',
    data(){
        return{
            product:{},
            index:0,
            isShow: false,
            productShow: false
        }
    },

methods: {
    async callAPI(){
        const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
        const result = await response.json();
        return result;
    },
    async theProduct() {
        this.isShow = true;

        if (this.index !== 20){
            this.index++
        } else {
            this.index = 1
        }

        let data = await this.callAPI()
        if (data.category === "men's clothing" || data.category === "women's clothing") {
            this.product = {data}
            this.productShow = true;
        } else {
            this.productShow = false;
        }

        this.isShow = false;
    },
},
    mounted() {
        this.theProduct();
    },
}
</script>