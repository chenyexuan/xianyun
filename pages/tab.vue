<template>
  <div class="tab">
      <span 
      v-for="(item,index) in ['体育','新闻','娱乐']" 
      :key="index" 
      @click="toggle(index)"
      :class="{Red:current===index}"
      >{{item}}</span>
      
      <ul v-show="current===0">
          <li>体育</li>
          <li>体育</li>
          <li>体育</li>
      </ul>
      <ul v-show="current===1">
          <li>新闻</li>
          <li>新闻</li>
          <li>新闻</li>
      </ul>
      <ul v-show="current===2">
          <li>娱乐</li>
          <li>娱乐</li>
          <li>娱乐</li>
      </ul>
  </div>
</template>

<script>
export default {
    data(){
        return{
            current:0
        }
    },
    methods:{
        toggle(index){
            this.current=index
        }
    }
}
</script>

<style lang='less' scoped>
    .tab{
        margin:200px 100px;
        span{
            cursor: pointer;
        }
    }
    .Red{
        color: #f00;
    }
</style>