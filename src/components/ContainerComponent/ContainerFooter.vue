<template>
  <div class="footer">
    <div>{{ activeCnt }} item(s) left</div>
    <div class="moodChange">
      <button :class="{ active: 2 === mood }" @click="changeMood(2)">all</button>
      <button :class="{ active: 1 === mood }" @click="changeMood(1)">active</button>
      <button :class="{ active: 0 === mood }" @click="changeMood(0)">completed</button>
    </div>
    <button class="clearBtn" @click="clearCompleted">Clear completed</button>

  </div>
</template>

<script>
export default {
    name:'HeaderComponent',
    props:['activeCnt', 'mood'],
    methods:{
      changeMood(mood){
        this.$bus.$emit('changeMood', mood)
      },
      clearCompleted(){
        this.$bus.$emit('clearCompleted')
      }
    }
}
</script>

<style scoped>
  .footer {
    display: flex;
    flex-direction: row;
    /* align-items: center; */
    justify-content: space-between;
    height: 50px;
    width: 540px;
    /* background-color: orange; */
    font-size: 12px;
  }

  .moodChange {
    width: 200px;
    display: flex;
    flex-direction: row;
    /* align-items: center; */
    justify-content: space-between;
  }

  .moodChange button {
    padding: 8px 16px 8px 16px;
  }

  /* button被选中时有边框 */
  .active {
    border: 1px solid silver;
    border-radius: 8px;
  }

</style>