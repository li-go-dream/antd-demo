<template>
  <div>
    <button @click="visible = true">打开</button>
    <a-modal width="600px" v-model="visible" title="选择部门" ok-text="确认" cancel-text="取消" @cancel="cancel" @ok="submit">
       <a-row>
        <a-col :span="12" class="border">
          col-12
        </a-col>
        <a-col :span="12">
          col-12
        </a-col>
      </a-row>
    </a-modal>
  </div>
</template>

<script>
export default {
  data(){
    return {
      visible:false
    }
  },
  methods:{
    submit(){
      this.visible = false
    },
    cancel(){
      this.visible = false
    }
  }
}
</script>

<style lang='less'>
  .border{
    border-right: 1px solid #e8e8e8;
  }
</style>