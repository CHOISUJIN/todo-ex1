<template>
    <div class="category-wrap">
        <div class="category-add__btn" v-on:click="addCategory">
            <span>
                <i class="fa fa-plus"></i>
                목록추가
            </span>
        </div>
        <ul>
            <li v-for="(categoryItem, index) in propscategory" v-bind:key="index">
                <i class="fa fa-align-justify"></i>
                <span class="category__item" v-on:click="selectCategory(index)">{{categoryItem}}</span>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props : ['propscategory'],
    methods: {
        addCategory() {
            
        },
        selectCategory(index){
            this.$emit("changeCategory", index);
        }
    },
    created() {
        
    }


}
</script>

<style scoped>
ul{list-style-type: none;padding-left: 0; margin-top: 0; text-align: left;}
li{min-height: 50px;height: 50px;line-height: 50px;margin-bottom: 0.5rem;padding: 0 1.9rem;}
.category-wrap{background-color: #fff;height: 100%;}
.category-add__btn{padding: 20px 0;cursor: pointer;}
.category__item{cursor: pointer;}
</style>

