<template>
    <div class="item-row">
      <div class="select-all" v-if="allName">
        {{allName}}
      </div>
      <div class="item-row items-row">
        <span v-for="(item,index) of items" class="item-span" :class="item.selected?'active':''" :key="index" @click="changeStatus(index,items)">{{item.name}}<i v-if="item.selected" class="el-icon-close"></i></span>
        <!-- <span class="item-span ">器视</span>
        <span class="item-span active">器视<i class="el-icon-close"></i></span> -->
      </div>
    </div>
</template>

<script>
export default {
  props:{
    items: Array,
    allName:String
  },
  data(){
    return{
      // items: ["机器视觉","机器视觉","机器视觉","机器视觉","机器视觉","机器视觉","机器视觉","机器视觉","机器视觉","机器视觉","机器视觉","机器视觉","机器视觉","机器视觉",]
    }
  },
  methods: {
    changeStatus(index,items){
      let vm = this;
      // debugger
      // console.log(items);
      vm.$emit("change", {index:index, list:items});
    }
  }
}
</script>

<style>
.item-row{
  display: flex;
  align-items: flex-start;
}
.items-row{
  flex-wrap:wrap;
  align-items: center;

}
.item-span,.select-all{
  padding: 5px 10px;
}
.item-span:hover{
  cursor: pointer;
}
.item-span.active{
  color: #5d78ff;
  background: #eff2ff;
  border-radius: 5px;
  line-height: 14px;
}
.item-span.active .el-icon-close{
  color: #9899a1;
}
.select-all{
  /* margin-right: 20px; */
  width: 60px;
}
</style>

