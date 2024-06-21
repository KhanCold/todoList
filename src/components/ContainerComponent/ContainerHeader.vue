<template>
  <div class="item">
    <input type="text" placeholder="Write your todos here..." @keyup.enter="addTodo" v-model="title">
  </div>
</template>

<script>
export default {
    name:'HeaderComponent',//中间componet的Header部分，此为输入框
    data() {
      return {
        title :''//input框内容
      }
    },
    props:['insertTodo'],
    methods:{
      addTodo(){
        //新增一个代办
        if(this.title == '') return ;//防止插入为空
        this.insertTodo(this.title.trim())
        //清空输入框中内容
        this.title=''
      }
    }
}
</script>

<style scoped>

</style>