<template>
  <div>
    <div class="wrap-input">
      <input type="text" v-model.trim="msg" @keyup.enter = 'addTask()' placeholder="请添加任务..."/>
      <button @click="addTask()">添加任务</button>
    </div>
    <transition-group tag="ul">
      <li v-for="(item,index) in list"  :key="index">
        <p v-text="item.msg" @click="isFinished(index)" :class="{finish: item.isFinish}"></p>
        <button v-if="item.isFinish" @click="deleteitem(index)">删除任务</button>
      </li>
    </transition-group>
  </div>
</template>

<script>
  import storage from '../../static/js/storage.js'
  export default {
    data(){
      return {
        list: [],
        msg: '',
        isFinish: false
      }
    },
    methods: {
      /*添加数据*/
      addTask() {
        this.list = this.list || [];
        if(this.msg){
          this.list.push({msg: this.msg, isFinish: this.isFinish});
          this.msg = '';
          storage.set('list',this.list);
        }
      },
      /*判断任务是否完成*/
      isFinished(index) {
        this.list[index].isFinish = !this.list[index].isFinish;
        storage.set('list',this.list);
      },
      /*删除数据*/
      deleteitem(index){
        this.list.splice(index, 1);
        storage.set('list',this.list);
      }
    },
    mounted: function () {
      /*取缓存*/
      this.list = storage.get('list');
    }
  }
</script>

<style lang="scss" scoped>
  .wrap-input{
    margin-top: 1rem;
    input{
      width: 65%;
      padding: .15rem;
      border: 1px solid #ddd;
      border-radius: 8px;
      margin-right: .2rem;
      font-size: .35rem;
      vertical-align: middle;
    }
    button{
      background-color: pink;
      color: #fff;
      border-radius: .12rem;
      font-size:.36rem;
      vertical-align: middle;
      padding: .1rem .3rem;
    }
  }
  ul{
    margin: .5rem .6rem;
    li{
      /*list-style: circle;*/
      text-align: left;
      font-size: .36rem;
      margin-bottom: .3rem;

      .finish{
        text-decoration: line-through;
        display: inline-block;
        width: 74%;
      }
      button{
        float: right;
        font-size: .3rem;
        border-radius: .12rem;
        padding: .04rem .2rem;
        background-color: pink;
        color: #fff;
        opacity: .7;
        &:active{
          opacity: .5;
        }
      }
    }

  }
  .v-enter-active, .v-leave-active {
    transition: all 1s;
  }
  .v-enter, .v-leave-to
    /* .list-leave-active for below version 2.1.8 */ {
    opacity: 0;
    transform: translateX(-50px) ;
  }

</style>


