<template>
  <div>
    <div class="input-con" :style="'width:' + width" v-clickOutside="clickOutside">
      <div class="input-con-item" @click="isselect = !isselect">
        <a-tag v-for="(item,index) in choosename" @close="closetag(item,index)"  :key="item.id" closable>{{item.name}}</a-tag>
      </div>
			<div class="input-select" v-if="isselect">
				<tree v-if="list.length > 0" :list="list" :chooseid="chooseid" :openid="openid" @checkeditem="checkeditem" @itemopen="itemopen"></tree>
				<a-empty v-else :image="simpleImage"/>
			</div>
    </div>
  </div>
</template>

<script>
import { Empty } from 'ant-design-vue';
import tree from './components/tree.vue'
export default {
	props:{
		width:{
			type:String,
			default:()=>{
				return '400px'
			}
		},
		list:{
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
		}
	},
  watch:{
    chooseid:{
      handler:function(newv,oldv){
        if(newv.length > 0){
          this.choosename = []
          newv.map(item => {
            let name = this.depname(this.list,item)
            this.choosename.push({id:item,name})
          })
        }
      },
      deep:true
    }
  },
	components:{
		tree,
	},
	data(){
		return{
			simpleImage : Empty.PRESENTED_IMAGE_SIMPLE,
      openid:[],
      choosename:[],
      isselect:false
		}
	},
  directives: { clickOutside:{
    // 初始化指令
    bind(el, binding, vnode) {
      function clickHandler(e) {
        // 这里判断点击的元素是否是本身，是本身，则返回
        if (el.contains(e.target)) {
          return false;
        }
        // 判断指令中是否绑定了函数
        if (binding.expression) {
          // 如果绑定了函数 则调用那个函数，此处binding.value就是handleClose方法
          binding.value(e);
        }
      }
      // 给当前元素绑定个私有变量，方便在unbind中可以解除事件监听
      el.__vueClickOutside__ = clickHandler;
      document.addEventListener("click", clickHandler);
    },
    unbind(el, binding) {
      // 解除事件监听
      document.removeEventListener("click", el.__vueClickOutside__);
      delete el.__vueClickOutside__; // 删除属性
    }
  } },
  methods:{
    itemopen(openid){
      this.openid = openid.filter(res=>{return res!="undefined"})
    },
    clickOutside(){
      this.isselect = false
    },
    depname(arr,id){
      let name = ''
      arr.map(item => {
        if(item.id == id){
          name = item.name
        }else{
          if(item.child){
            let str = this.depname(item.child,id)
            if(str){
              name = str
            }else{
              return ''
            }
          }else{
            return ''
          }
        }
      })
      return name
    },
    checkeditem(item){
      if(item.id == 5){
        if(this.chooseid.indexOf(5) != -1){
          this.chooseid.splice(0,this.chooseid.length)
          this.choosename.splice(0,this.choosename.length)
        }else{
          this.chooseid.splice(0,this.chooseid.length)
          this.chooseid.push(item.id)
          this.choosename.splice(0,this.choosename.length)
          this.choosename.push({id:item.id,name:item.name})
        }
      }else{
        if(this.chooseid.indexOf(5) != -1){
          this.chooseid.splice(this.chooseid.indexOf(5),1)
          this.choosename.map((it,index) => {
            if(it.id == 5){
              this.choosename.splice(index,1)
            }
          })
        }
        if(this.chooseid.includes(item.id)){
          this.chooseid.splice(this.chooseid.indexOf(item.id),1)
          this.choosename.map((it,index) => {
            if(it.id == item.id){
              this.choosename.splice(index,1)
            }
          })
        }else{
          this.chooseid.push(item.id)
          this.choosename.push({id:item.id,name:item.name})
        }
      }
      this.$emit('checkeditem',this.chooseid)
    },
    closetag(item,index){
      this.choosename.splice(index,1)
      this.chooseid.splice(this.chooseid.indexOf(item.id),1)
      this.$emit('checkeditem',this.chooseid)

    }
  }
}
</script>

<style lang='less' scoped>
	.input-con{
		cursor: text;
		min-height: 30px;
		box-sizing: border-box;
		background-color: #fff;
		border: 1px solid #d9d9d9;
		border-top: 1.02px solid #d9d9d9;
		border-radius: 4px;
		outline: none;
		transition: all .3s cubic-bezier(.645,.045,.355,1);
		user-select: none;
		position: relative;
    .input-con-item{
      min-height: 30px;
      padding:3px 4px;
      display: flex;
      flex-wrap: wrap;
    }
		.input-select{
			position: absolute;
			left: 0;
			top: 100%;
			right: 0;
			z-index: 100;
			box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
			min-height: 30px;
      max-height: 500px;
      border-radius: 4px;
      overflow-y: scroll;
		}
	}
	.input-con:hover{
		border-color: #40a9ff;
	}
</style>