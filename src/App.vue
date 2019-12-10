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
                v-model="input2">
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
              v-model="value1"
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
          <condition-row :items="firstIndustry" allName="全部" @change="handleSelect"></condition-row>
        </div>
        <div class="my-row condition-row">
          <div class="condition-label">
            已选条件：
          </div>
          <!-- <condition-row :items="[...selectedLabel]" @change=";"></condition-row> -->
          <div class="item-row">
            <div class="item-row items-row">
              <span v-for="(item,index) of selectedLabel" class="item-span" :class="item.selected?'active':''" :key="index" @click="changeStatus(index,items)">{{item.name}}<i class="el-icon-close"></i></span>
              <!-- <span class="item-span ">器视</span>
              <span class="item-span active">器视<i class="el-icon-close"></i></span> -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ConditionRow from './ConditionRow.vue';
import {firstIndustryList} from './testData';

export default {
  data(){
    return{
      firstIndustry: firstIndustryList,    // 第一行业
      // selectedLabel: new Set(),     // 已选条件,eg: "a","b"
      selectedLabel: [],     // 已选条件,eg: {name:"a", list:[], index:1}
    }
  },
  methods: {
    // 变化选中状态
    handleSelect(item){
      let vm = this;
      // console.log(index,firstIndustry);
      console.log(item);
      let selected = item.list[item.index].selected;
      // 如果之前是选中，就取消
      if(selected){
        item.list[item.index].selected = false;
        // if(vm.selectedLabel.has(item.list[item.index].name)){
        //   vm.selectedLabel.delete(item.list[item.index].name);
        // }
        // TODO 
        let selectedLabel = vm.selectedLabel;
        for(let i = 0; i < selectedLabel.length; i++){
          let list = selectedLabel[i].list;
          if(list && list[selectedLabel[i].index] == item.list[item.index]){
            // 删除
            list.splice(i, 1);
          }
        }
      }else{
        item.list[item.index].selected = true;
        // if(vm.selectedLabel.has(item.list[item.index].name)){
        //   vm.selectedLabel.add(item.list[item.index].name);
        // }
        vm.selectedLabel.
      }


    }
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
</style>
