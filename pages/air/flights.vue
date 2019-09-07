<template>
  <section class="contianer">
    <el-row type="flex" justify="space-between">
      <!-- 顶部过滤列表 -->
      <div class="flights-content">
        <!-- 过滤条件 -->
        <div></div>

        <!-- 航班头部布局 -->
        <FightsListHead />

        <!-- 航班信息 -->
        <FlightsItem v-for="(item,index) in dataList" :key="index" :data="item" />
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage4"
          :page-sizes="[5, 10, 15, 20]"
          :page-size="5"
          layout="total, sizes, prev, pager, next, jumper"
          :total="flightsData.length"
        ></el-pagination>
      </div>

      <!-- 侧边栏 -->
      <div class="aside">
        <!-- 侧边栏组件 -->
      </div>
    </el-row>
  </section>
</template>

<script>
import moment from "moment";
import FightsListHead from "../../components/air/flightsListHead";
import FlightsItem from "@/components/air/flightsItem";
export default {
  components: {
    FightsListHead,
    FlightsItem
  },
  mounted() {
    // console.log(this.$route.query)
    this.$axios({
      url: "/airs",
      params: this.$route.query
    })
      .then(res => {
        // console.log(res);
        this.flightsData = res.data.flights;
        this.dataList = this.flightsData.slice(0,this.pageSize);
      })
      .catch(err => {
        console.log(err);
      });
  },
  data() {
    return {
      dataList: [],
      flightsData: [],
      currentPage4:1,
      pageSize:5
    };
  },
  methods:{
      handleSizeChange(val){
          this.pageSize=val
        this.dataList = this.flightsData.slice(0,val)
      },
      handleCurrentChange(val){
        //   console.log(val)
        this.dataList = this.flightsData.slice((val-1)*this.pageSize,val*this.pageSize)
      }
  }
};
</script>

<style scoped lang="less">
.contianer {
  width: 1000px;
  margin: 20px auto;
}

.flights-content {
  width: 745px;
  font-size: 14px;
}

.aside {
  width: 240px;
}
</style>