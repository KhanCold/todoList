<template>
  <div class="outside-container">
    <container-header :insertTodo="insertTodo"/>
    <container-body :todoList="todoList" :deleteTodo="deleteTodo" :mood="mood"/>
    <container-footer class="container-footer" :activeCnt="activeCnt" :mood="mood" :changeMood="changeMood" :clearCompleted="clearCompleted"/>
  </div>
</template>

<script>
import ContainerBody from './ContainerComponent/ContainerBody.vue'
import ContainerFooter from './ContainerComponent/ContainerFooter.vue'
import ContainerHeader from './ContainerComponent/ContainerHeader.vue'
export default {
  components: { ContainerHeader,ContainerBody,ContainerFooter },
    name:'ContainerComponent',
    props:['todoList', 'insertTodo', 'deleteTodo', 'activeCnt', 'mood', 'changeMood', 'clearCompleted']

}
</script>

<style>
    .outside-container {
        position: relative;
        justify-content: flex-start;
        align-items: flex-start;
        width: 100%;
        padding: 30px 30px 100px 30px;
        background-color: #f8f8f8;
        border-radius: 7px;
        box-shadow: rgba(0, 0, 0, 0.3) 0px 0px 10px;
    }

    .container-footer {
        position: absolute;
        bottom: 30px;
    }

    /* 所有contanier之中的小元素样式（下border等样式） */
    .item{
        flex-direction: row;
        justify-content: flex-start;
        height: 50px;
        width: 100%;
        padding: 10px;
        border-bottom: 1px solid;
        border-color: silver;
    }

</style>