<template>
  <div class="home">
    <a-button @click="type='dep-select'">部门下拉</a-button>
    <a-button @click="type='choose-dep'">选择部门</a-button>
    <a-button @click="type='choose-user'">选择用户</a-button>
    <component :is="type" :list="list" :chooseid="chooseid" @checkeditem='chooseitem'></component>
  </div>
</template>

<script>
// @ is an alias to /src
import DepSelect from '@/components/DepSelect/index.vue'
import ChooseDep from '@/components/ChooseDep/index.vue'
import ChooseUser from '@/components/ChooseUser/index.vue'

export default {
  name: 'Home',
  data(){
    return{
      type:'dep-select',
      list:[
        {
          id:1,
          name:'1',
          child:[
            {
              id:2,
              name:'2',
              child:[
                {
                  id:4,
                  name:'4'
                },
              ]
            },
            {
              id:3,
              name:'3'
            },
          ],
        },
        {
          id:5,
          name:'5'
        },
      ],
      chooseid:[]
    }
  },
  components:{
    DepSelect,
    ChooseDep,
    ChooseUser
  },
  mounted(){
    setTimeout(()=>{
      this.chooseid = [4,3,1]
    },500)
  },
  methods:{
    chooseitem(chooseid){
      this.chooseid = chooseid
      console.log(this.chooseid)
    }
  }
}
</script>
