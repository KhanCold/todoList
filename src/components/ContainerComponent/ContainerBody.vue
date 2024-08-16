<template>
  <ul>
    <!-- debug用！！！ -->
    <!-- <li class="item"><button @click="showAllTodo">点击输出</button></li>  -->
    <li v-for="t in filTodoList" :key="t.id" class="item">
      <input type="checkbox" v-model="t.completed">
      
      <div v-if="!t.isEditing" class="dbInput" :style="t.completed ? { textDecoration: 'line-through' } : { textDecoration: 'none' }" @dblclick="toggleInput(t)">{{ t.title }}</div>
      <input v-else class="dbInput" v-model="t.title"  @blur="endEdit(t)" @keyup.enter="endEdit(t)"/>
      <button class="btn" @click="delTodo(t.id)">❌</button>
    </li>
  </ul>
</template>

<script>
export default {
    name:'HeaderComponent',
    props:['todoList', 'mood'],
    computed:{
      filTodoList(){//渲染的是这个，根据all active completed三个动态改变
        return this.todoList.filter((p) => {
                    if(!this.mood) {//此时渲染completed
                      return p.completed
                    }
                    else if(this.mood == 1) {//此时渲染 active
                      return !p.completed
                    }
                    else return true; //此时渲染all
                })
      },
    },
    methods:{
      showAllTodo(){//输出当前列表中的全部信息，debug专用
        console.log(JSON.parse(JSON.stringify(this.todoList)));
      },
      //删除列表中的信息
      delTodo(d){
        this.$bus.$emit('deleteTodo', d)
      },
      //双击时候切换成input框t
      toggleInput(t) {
        t.isEditing = true
      },
      //结束输入时切换回div
      endEdit(t)
      {
        t.isEditing = false
      }
    }
}
</script>

<style scoped>
ul {
  width: 100%;
  min-height: 240px;
}
li{
  display: flex;
}

.dbInput {
  margin-left: 5px;
  font-family: 'Helvetica', 'Arial', sans-serif;
    font-size: 16px; /* 或使用相对单位如1rem */
    line-height: 1.5;
}

.btn {
  display:none;
  position:absolute;
  right: 50px;
}

li:hover button {
  display:block;
}

</style>