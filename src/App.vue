<template>
  <div id="app" class="body">
    <div class="box">
      <div class="header">筛选条件</div>
      <div class="condition">
        <div class="my-row">
          <div class="condition-label">
            关键字：
          </div>
          <div class="condition-content">
            <div class="demo-input-suffix">
              <el-input 
                placeholder="搜索"
                prefix-icon="el-icon-search"
                v-model="keyName">
              </el-input>
            </div>
          </div>
        </div>
        <div class="my-row">
          <div class="condition-label">
            时间范围：
          </div>
          <div class="condition-content">
            <el-date-picker
              v-model="dateTime"
              type="monthrange"
              range-separator="-"
              start-placeholder="开始月份"
              end-placeholder="结束月份">
            </el-date-picker>
          </div>
        </div>
        <div class="my-row condition-row">
          <div class="condition-label">
            一级行业：
          </div>
          <!-- 获取子组件被点击的元素在list的索引（arguments[0]为index），同时将父组件的数组传入handleSelect -->
          <condition-row :items="firstIndustry" allName="全部" @change="handleSelect(firstIndustry, arguments[0])"></condition-row>
        </div>
        <div class="my-row condition-row">
          <div class="condition-label">
            二级行业：
          </div>
          <!-- 获取子组件被点击的元素在list的索引（arguments[0]为index），同时将父组件的数组传入handleSelect -->
          <condition-row :items="secondIndustryList" allName="全部" @change="handleSelect(secondIndustryList, arguments[0])"></condition-row>
        </div>
        <div class="my-row condition-row">
          <div class="condition-label">
            三级行业：
          </div>
          <!-- 获取子组件被点击的元素在list的索引（arguments[0]为index），同时将父组件的数组传入handleSelect -->
          <condition-row :items="thirdIndustryList" allName="不限" @change="handleSelect(thirdIndustryList, arguments[0])"></condition-row>
        </div>
        <div class="my-row condition-row">
          <div class="condition-label">
            已选条件：
          </div>
          <div class="item-row">
            <div class="item-row items-row">
              <span v-for="(item,index) of selectedLabel" class="item-span active" :key="index" 
                @click="changeStatus(item, index)">{{item.list[item.index].name}}<i class="el-icon-close"></i></span>
              <span v-show="selectedLabel.length" class="item-span clear-all" @click="clearAll">清空筛选</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ConditionRow from './ConditionRow.vue';
import {firstIndustryList, secondIndustryList, thirdIndustryList} from './testData';

export default {
  data(){
    return{
      timeout:null,        // 节流用
      keyName: "",      //关键字
      dateTime: "",     //日期
      firstIndustry: firstIndustryList,    // 第一行业
      secondIndustryList: secondIndustryList,    // 第一行业
      thirdIndustryList: thirdIndustryList,    // 第一行业
      selectedLabel: [],     // 已选条件,eg: {list:[], index:1}
    }
  },
  methods: {
    // 变化选中状态
    handleSelect(list, index){
      let vm = this;
      console.log(list, index);
      let selected = list[index].selected;
      if(selected){
        // 如果之前是选中，就取消
        list[index].selected = false;

        for(let i = 0; i < vm.selectedLabel.length; i++){
          // 判断vm.selectedLabel是否有{list:list, index:index}，如果有则进行删除
          if(vm.selectedLabel[i].list == list && vm.selectedLabel[i].index == index){
            vm.selectedLabel.splice(i, 1);
            break;
          }
        }
      }else{
        // 否则进行选中.
        list[index].selected = true;
        // 添加已选条件
        vm.selectedLabel.push({
          list:list,
          index:index
        });
      }
    },
    // 从已选条件中清除已选中的元素
    changeStatus(item, index){
      let vm = this;
      // list置为false
      item.list[item.index].selected = false;
      // 将item从vm.selectedLabel移除
      vm.selectedLabel.splice(index,1);
    },
    // 当筛选条件改变的时候触发搜索
    search(){
      let vm = this;
      // 节流
      if(vm.timeout !== null)   clearTimeout(vm.timeout);        
      vm.timeout = setTimeout(()=>{
        console.log('入参：',vm.keyName, vm.dateTime, vm.selectedLabel);
        // TODO 获取入参，后台确定交互格式，进行请求，然后更新表格数据
      }, 1000);
      
    },
    // 清空所有筛选条件
    clearAll(){
      let vm = this;
      vm.selectedLabel.forEach((item, index)=>{
        // list置为false
        item.list[item.index].selected = false;
      })
      vm.selectedLabel = [];
    }
  },
  watch: {
    // 当筛选条件改变的时候触发搜索
    selectedLabel: function (newValue, oldValue) {
      this.search();
    },
    keyName: function (newValue, oldValue) {
      this.search();
    },
    dateTime: function (newValue, oldValue) {
      this.search();
    },
  },
  components:{
    ConditionRow
  }
}
</script>

<style>
#app {
  font-family: Helvetica, sans-serif;
  text-align: center;
}

*{
  padding: 0;
  margin: 0;
}

body{
  padding: 30px 25px;
  background: #f9f9fc;
  color: #9899a1;
}

.box{
  background: #ffffff;
  padding-left: 30px;
}

.header{
  text-align: left;
  padding: 30px 30px;
  font-size: 20px;
  font-weight: bold;
  color: #48475c;
  border-bottom: 2px solid #f9f9fc;
}

.my-row{
  display: flex;
  align-items: center;
  margin: 25px 0px;
}

.condition-label{
  width: 100px;
}

.my-row.condition-row{
  align-items: flex-start;
}
.condition-row .condition-label{
  padding: 5px 0;
}
.item-span.clear-all{
  margin-left: 30px;
  color: #5d78ff;
}
</style>
