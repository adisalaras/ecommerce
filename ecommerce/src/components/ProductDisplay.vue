<script>
export default{
    name: 'ProductDisplay',
    data(){
        return {
            isLoading: false,
            index: 0,
            isItemAvailable:false,
            item: {}
        }
    },
    // call api
    methods: {
        async callAPI() {
            const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
            const result = await response.json();
            return result;
        },
        //get product by index
        async getSingleItem() {
            this.isLoading = true;

            if (this.index !== 20) {
                this.index++
            } else {
                this.index = 1;
            }

            //data get category mens and womens clothings==isItemavailable, other is unavailable
            let data = await this.callAPI()
            if (data.category === "men's clothing" || data.category === "women's clothing") {
                //membungkus data di dalam item
                this.item = { data }
                this.isItemAvailable = true;
            } else {
                this.isItemAvailable = false;
            }

            this.isLoading = false;
        }
    },
    mounted(){
            this.getSingleItem();
        },
    
}
</script>

<template>
    <div class="container1">
        <div v-if="isLoading" class="loader"></div>
        <div v-else class="container" :class="!isItemAvailable ? 'bg-gray' : item.data.category === 'men\'s clothing' ? 'bg-blue' : 'bg-pink'">
            <div class="overlay">
                <img src="../assets/bg-shape.svg" alt="background overlay">
            </div>
            <div class="card">
                <div v-if="!isItemAvailable" class="item-unavailable-container">
                    <div class="overlay">
                        <img src="../assets/bg-sad.svg" alt="background unavailable item" srcset="">
                    </div>
                    <div class="item-details">
                        <p>This item is unavailable to show</p>
                        <div class="cta">
                            <button type="button" @click="getSingleItem()" class="cta-next">Next item</button>
                        </div>
                    </div>
                </div>
                <div v-else class="item-container">
                    <div class="item-image">
                        <img :src="item.data.image" alt="">
                    </div>
                    <div class="item-details">
                        <div class="top">
                            <h3 :class="item.data.category === 'men\'s clothing' ? 'font-navy' : 'font-magenta'" class="title">{{ item.data.title }}</h3>
                            <div class="sub-title">
                                <span>{{ item.data.category }}</span>
                                <div class="rating">
                                    <span>{{ item.data.rating.rate }}/5</span>
                                    <div class="rating">
                                        <span :class="item.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="item.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="item.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="item.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                        <span :class="item.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="circle"></span>
                                    </div>
                                </div>
                            </div>
                            <div class="description">
                                <p>{{ item.data.description }}</p>
                            </div>
                        </div>
                        <div class="bottom">
                            <span :class="item.data.category === 'men\'s clothing' ? 'font-navy' : 'font-magenta'" class="price">${{ item.data.price }}</span>
                            <div class="cta">
                                <button type="button" :class="item.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'" class="cta-buy">Buy Now</button>
                                <button type="button" @click="getSingleItem()" :class="item.data.category === 'men\'s clothing' ? 'border-navy font-navy' : 'border-magenta font-magenta'" class="cta-next">Next Product</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>



<style >
@import '../assets/style/page.css';
</style>
