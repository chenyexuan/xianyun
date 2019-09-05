<template>
  <div class="search-form">
    <el-row type="flex" class="search-tab">
      <span
        v-for="(item,index) in tabs"
        :key="index"
        @click="handleSearchTab(index)"
        :class="{active:index===currentTab}"
      >
        <i :class="item.icon"></i>
        {{item.name}}
      </span>
    </el-row>
    <el-form class="search-form-content" ref="form" label-width="80px">
      <el-form-item label="出发城市">
        <el-autocomplete
          class="inline-input"
          v-model="form.departCity"
          :fetch-suggestions="queryDepartSearch"
          placeholder="请输入内容"
          @select="handleDepartSelect"
        ></el-autocomplete>
      </el-form-item>
      <el-form-item label="到达城市">
        <el-autocomplete
          class="inline-input"
          v-model="form.destCity"
          :fetch-suggestions="queryDestSearch"
          placeholder="请输入内容"
          :trigger-on-focus="false"
          @select="handleDestSelect"
        ></el-autocomplete>
      </el-form-item>
      <el-form-item label="出发时间" class="block">
        <el-date-picker
          v-model="form.departDate"
          :picker-options="pickerOptions"
          value-format="yyyy-MM-dd"
          type="date"
          format="yyyy 年 MM 月 dd 日"
          placeholder="选择出发日期"
        ></el-date-picker>
      </el-form-item>
      <el-form-item>
        <el-button style="width:100%" type="success" icon="el-icon-search" @click="handleSubmit">搜索</el-button>
      </el-form-item>
      <div class="reverse">
        <span @click="handleReverse">换</span>
      </div>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tabs: [
        { icon: "iconfont icondancheng", name: "单程" },
        { icon: "iconfont iconshuangxiang", name: "往返" }
      ],
      currentTab: 0,
      form: {
        departCity: "",
        departCode: "",
        destCity: "",
        destCode: "",
        departDate: ""
      },
      value1: "",
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() < Date.now();
        }
      },
      newData: [],
      flightsData: {}
    };
  },
  methods: {
    handleSearchTab(item, index) {
      this.currentTab = index;
    },
    queryDepartSearch(value, cb) {
      if (!value) {
        cb([]);
        return;
      }
      this.$axios({
        url: "/airs/city",
        params: {
          name: value
        }
      }).then(res => {
        // console.log(res.data.data)
        const { data } = res.data;
        const newData = [];
        data.forEach(v => {
          v.value = v.name.replace("市", "");
          newData.push(v);
        });
        // console.log(newData)
        this.form.departCity = newData[0].value;
        this.form.departCode = newData[0].sort;
        cb(newData);
      });
    },
    queryDestSearch(value, cb) {
      if (!value) {
        cb([]);
        return;
      }
      this.$axios({
        url: "/airs/city",
        params: { name: value }
      }).then(res => {
        const { data } = res.data;
        const newData = [];
        data.forEach(v => {
          v.value = v.name.replace("市", "");
          newData.push(v);
        });
        // console.log(newData)
        this.form.destCity = newData[0].value;
        this.form.destCode = newData[0].sort;
        cb(newData);
      });
    },
    handleDepartSelect(value) {
      //   console.log(value);
      this.form.departCity = value.value;
      this.form.departCode = value.sort;
      //   console.log(this.form);
    },
    handleDestSelect(value) {
      this.form.destCity = value.value;
      this.form.destCode = value.sort;
      //   console.log(this.form);
    },
    handleSubmit() {
      //   console.log(this.form);
      if (!this.form.departCity) {
        this.$alert("请输入出发地点", "提示");
        return;
      } else if (!this.form.destCity) {
        this.$alert("请输入目的地点", "提示");
        return;
      } else if (!this.form.departDate) {
        this.$alert("请输入出发时间", "提示");
        return;
      }
      this.$axios({
        url: "/airs",
        params: this.form
      }).then(res => {
        this.flightsData = res.data;
        console.log(this.flightsData);
      });
      this.$router.push({
        path: "/air/flights",
        query: this.form
      });
    },
    handleSearchTab(index) {
      if (index === 1) {
        this.$alert("目前暂时不支持往返", "提示");
      }
    },
    handleReverse() {
      const { departCity, departCode, destCity, destCode } = this.form;
      this.form.departCity = destCity;
      this.form.departCode = destCode;
      this.form.destCity = departCity;
      this.form.destCode = departCode;
    }
  },
  mounted() {}
};
</script>

<style lang="less" scoped>
.search-form {
  border: 1px #ddd solid;
  border-top: none;
  width: 360px;
  height: 350px;
  box-sizing: border-box;
}
.search-tab {
  span {
    display: block;
    flex: 1;
    text-align: center;
    height: 48px;
    line-height: 42px;
    box-sizing: border-box;
    border-top: 3px #eee solid;
    background: #eee;
  }
  .active {
    border-top-color: orange;
    background: #fff;
  }
  i {
    margin-right: 5px;
    font-size: 18px;
    &:first-child {
      font-size: 16px;
    }
  }
}
.search-form-content {
  padding: 15px 50px 15px 15px;
  position: relative;
  .el-autocomplete {
    width: 100%;
  }
}
.reverse {
  position: absolute;
  top: 35px;
  right: 15px;
  &:after,
  &:before {
    display: block;
    content: "";
    position: absolute;
    left: -35px;
    width: 25px;
    height: 1px;
    background: #ccc;
  }
  &:after {
    top: 0;
  }
  &:before {
    top: 60px;
  }
  span {
    display: block;
    position: absolute;
    top: 20px;
    right: 0;
    font-size: 12px;
    background: #999;
    color: #fff;
    width: 20px;
    height: 20px;
    line-height: 18px;
    text-align: center;
    border-radius: 2px;
    cursor: pointer;
    &:after,
    &:before {
      display: block;
      content: "";
      position: absolute;
      left: 10px;
      width: 1px;
      height: 20px;
      background: #ccc;
    }
    &:after {
      top: -20px;
    }
    &:before {
      top: 20px;
    }
  }
}
</style>