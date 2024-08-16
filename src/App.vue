<template>
  <div id="app">
    <!-- 头部书签 todoList -->
    <header-component/>
    <!-- 中间的白色框  包含了三个子组件-->
    <container-component :todoList="rawTodoList" :activeCnt="activeCnt" :mood="mood" />
    <!-- 底部的footer，签名 -->
    <footer-conponent/>
  </div>
</template>

<script>

import { v4 as uuid } from 'uuid';

import ContainerComponent from './components/ContainerComponent.vue'
import FooterConponent from './components/FooterConponent.vue'
import HeaderComponent from './components/HeaderComponent.vue'

export default {
  name: 'App',
  components: {
    HeaderComponent,
    ContainerComponent,
    FooterConponent

  },
  data() {
      return {
        //实现浏览器的本地数据存储
        rawTodoList: JSON.parse(localStorage.getItem('rawTodoList')) || [//代办列表
          //id uuid生成，title代表代办 completed代表是否完成 isEditing代表是否被双击正在输入
          {id:'001', title:'🏪 To buy products', completed:true, isEditing:false},
          {id:'002', title:'🧽 To clean the house', completed:true, isEditing:false},
          {id:'003', title:'🐕 To feed the dog', completed:false, isEditing:false},
        ],
        mood:2//all 2; active 1 completed 0
      }  
    },
    methods: {//传入子组件，子组件调用函数时回到这里，用 :todoList="rawTodoList" 和 props:['insertTodo']的方式传入
      //新增一个新代办
      insertTodo(d) {
        this.rawTodoList.unshift({id:uuid(), title:d, completed:false, isEditing:false})
      },
      //删除一个代办
      deleteTodo(d) {
        this.rawTodoList = this.rawTodoList.filter(item => item.id !== d)
      },
      //点击按钮时切换展示模式 all active completed props无法直接修改，因此在这修改
      changeMood(d) {
        this.mood=d
      },
      //清除已完成的待办
      clearCompleted(){
        this.rawTodoList = this.rawTodoList.filter(t => !t.completed);
      }
    },
    computed:{
      //当前剩余还未完成的待办数
      activeCnt(){
        return this.rawTodoList.filter(t => !t.completed).length;
      }
    },
    watch:{
      rawTodoList:{//实现浏览器的本地数据存储
        deep:true,//深度监视可以监测到值变化
        handler(value){
          localStorage.setItem('rawTodoList',JSON.stringify(value))
        }
      }
    },
    mounted(){
      this.$bus.$on('insertTodo', this.insertTodo)
      this.$bus.$on('deleteTodo', this.deleteTodo)
      this.$bus.$on('changeMood', this.changeMood)
      this.$bus.$on('clearCompleted', this.clearCompleted)
    },
    beforeDestroy() {
      this.$bus.$off('insertTodo')
      this.$bus.$off('deleteTodo')
      this.$bus.$off('changeMood')
      this.$bus.$off('clearCompleted')
    },
}
</script>

<style>

  /* 全局通用简单样式设置 */
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  li {
    list-style-type: none;
  }


  button {
  /* 去除边框 */
  border: none;
  /* 去除背景 */
  background: transparent;
  /* 去除内边距 */
  padding: 0;
  /* 去除外边距 */
  margin: 0;
  /* 重置字体样式 */
  font: inherit;
  /* 去除轮廓，防止点击时出现轮廓 */
  outline: none;
  /* 去除按钮默认的圆角 */
  border-radius: 0;
  /* 允许自定义样式 */
  cursor: pointer;
}

  /* 自定义样式设计 */
  body {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    font-family: 'Helvetica', 'Arial', sans-serif;
    font-size: 16px; /* 或使用相对单位如1rem */
    line-height: 1.5;
    background-color: #e5e4e8;
  }

  #app {
    width: 600px;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
  }

div {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }


  /* input框美化 */
  input {
     border: none;
     outline: none;
     background-color: #f8f8f8;
     padding: 0;
     margin: 0;
   }

   input:focus {
     outline: none;
   }
  

</style>
