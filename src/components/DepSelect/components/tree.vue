<template>
  <ul class="tree-ul-con">
    <li v-for="item in list" :key="item.id">
      <span>
        <a-icon v-if="item.child" :class="openid.includes(item.id) ? 'tree-jt':'tree-jt tree-jt-open'" @click="itemopen(item)" type="caret-down"></a-icon>
        <span class="tree-null-icon" v-else></span>
      </span>
      <a-checkbox class="tree-checked" @click="checkedItem(item)" :checked="chooseid.includes(item.id)"></a-checkbox>
      <span class="tree-name" @click="checkedItem(item)">{{item.name}}</span>
      <tree v-if="item.child && openid.includes(item.id)" :openid="openid" :chooseid="chooseid" @itemopen="itemopen" @checkeditem="checkedItem" :list="item.child"></tree>
    </li>
  </ul>
</template>

<script>
export default {
  name:'tree',
  props:{
    list:{
      type:Array,
      default:()=>{
        return []
      }
    },
    openid:{
      type:Array,
      default:()=>{
        return []
      }
    },
    chooseid:{
      type:Array,
      default:()=>{
        return []
      }
    },
  },
  data(){
    return {
    }
  },
  methods:{
    itemopen(item){
      if(this.openid.includes(item.id)){
        this.openid.splice(this.openid.indexOf(item.id),1)
      }else{
        this.openid.push(item.id)
      }
      this.$emit('itemopen',this.openid)
    },
    checkedItem(item){      
      this.$emit('checkeditem',item)
    }
  }
}
</script>

<style lang="less">
  .tree-ul-con{
    // display: flex;
    list-style: none;
    padding: 0 2px;
    li{
      margin: 8px 0;
      i{
      transition: all .2s;

      }
      .tree-jt{
        font-size: 12px;
        transform: scale(.83333333) rotate(0deg);
        cursor: pointer;
      }
      .tree-jt-open{
        transform: scale(.83333333) rotate(-90deg);
      }
      .tree-checked{
        margin-left: 4px;
      }
      .tree-null-icon{
        display: inline-block;
        width: 12px;
        height: 17px;
      }
      .tree-name{
        display: inline-block;
        width: calc(100% - 32px);
        margin: 0;
        padding: 3px 5px;
        color: rgba(0,0,0,.65);
        text-decoration: none;
        border-radius: 2px;
        cursor: pointer;
        transition: all .3s;
        box-sizing: border-box;
      }
      .tree-name:hover{
        background-color: #e6f7ff;
      }
      ul{
        padding: 0 0 0 18px;
      }
    }
  }
</style>