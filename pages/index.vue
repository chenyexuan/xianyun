<template>
  <div class="container">
    <el-carousel height="700px">
      <el-carousel-item v-for="(item,index) in banners" :key="index">
        <div class="banner-image"
        :style="`
        background:url(${$axios.defaults.baseURL+item.url}) center center no-repeat;
        background-size:contain contain
        `"
        ></div>
      </el-carousel-item>
    </el-carousel>
  </div>
</template>

<script>
export default {
  data(){
    return{
      banners:[]
    }
  },
  async mounted () {
    let res =await this.$axios({
      url:'/scenics/banners'
    })
    console.log(res)
    this.banners = res.data.data
  }
};
</script>

<style lang="less">
  .container{
    min-width:1000px;
    margin:0 auto;
    position:relative;
    .banner-image{
        width:100%;
        height:100%;
    }
  }
</style>