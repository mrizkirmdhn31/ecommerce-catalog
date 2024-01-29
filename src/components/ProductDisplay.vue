<template>
    <div class="container">
        <div v-if="isLoading" class="card">
            <div class="product-container">
                <div class="default-thumbnail"></div>
                <div class="default-details">
                    <div class="details-top"></div>
                    <div class="details-bottom"></div>
                </div>
            </div>
        </div>
        <div v-else class="container" :class="!isProduct ? 'bg-gray' : product.data.category === 'men\'s clothing' ? 'bg-blue' : 'bg-pink'">
            <div class="overlay">
                <img src="../assets/bg-shape.svg">
            </div>
            <div class="card">
                <div v-if="!isProduct" class="product-unavailable-container">
                    <div class="overlay">
                        <img src="../assets/bg-sad.svg">
                    </div>
                    <div class="product-details">
                        <p>This product is unavailable to show</p>
                        <div class="button-action">
                            <button type="button" @click="getProduct()" class="button-next">Next Product</button>
                        </div>
                    </div>
                </div>
                <div v-else class="product-container">
                    <div class="product-thumbnail">
                        <img :src="product.data.image">
                    </div>
                    <div class="product-details">
                        <div class="top">
                            <h3 :class="product.data.category === 'men\'s clothing' ? 'font-navy' : 'font-magenta'" class="title">{{ product.data.title }}</h3>
                            <div class="sub-title">
                                <span>{{ product.data.category }}</span>
                                <div class="rating">
                                    <span>{{ product.data.rating.rate }}/5</span>
                                    <div class="rating">
                                        <!-- <span v-for="(star, index) in stars" :key="index"></span> -->
                                        <span :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                    </div>
                                </div>
                            </div>
                            <div class="description">
                                <p>{{ product.data.description }}</p>
                            </div>
                        </div>
                        <div class="bottom">
                            <span :class="product.data.category === 'men\'s clothing' ? 'font-navy' : 'font-magenta'" class="price">${{ product.data.price }}</span>
                            <div class="button-action">
                                <button type="button" :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="button-buy">Buy Now</button>
                                <button type="button" @click="getProduct()" :class="product.data.category === 'men\'s clothing' ? 'border-navy font-navy' : 'border-magenta font-magenta'" class="button-next">Next Product</button>
                            </div>
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
    data() {
        return {
            product: {},
            isLoading: false,
            index: 0,
            isProduct: false,
            // isRating: false
        }
    },
    methods: {
        async getAPI() {
            const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
            const result = await response.json();
            return result;
        },
        async getProduct() {
            this.isLoading = true;

            if (this.index !== 20) {
                this.index++
            } else {
                this.index = 1;
            }

            let data = await this.getAPI()
            if (data.category === "men's clothing" || data.category === "women's clothing") {
                this.product = { data }
                this.isProduct = true;
            } else {
                this.isProduct = false;
            }

            this.isLoading = false;
        },
        // async getRating(){
            // this.isRating=true;
            // let data = await this.getAPI()
    //     }
    // }
            // stars(rating){
            //     const fullCircle = Math.floor(data);
            //     const halfCircle = data % 1 >= 0.5 ? 1 : 0;
            //     const emtpyCircle = 5 - fullCircle - halfCircle;
            //     return [
            //         ...Array(fullCircle).fill({filled: true}),
            //         ...Array(halfCircle).fill({filled: true, half:true}),
            //         ...Array(emtpyCircle).fill({filled: false})
            //     ]
    },
    mounted() {
        this.getProduct();
    },
}
</script>