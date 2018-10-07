<template>
  <div>
    <h2 class="header"> Vue - todoList </h2>
    <div>
      <h3 class="holder">请输入事项</h3>
      <input v-model="inputValue" @keyup.enter="handleSubmit">
      <button @click="handleSubmit">提交</button>
    </div>
    <hr>
    <h3>正在进行</h3>
    <ul>
      <li v-if="item.status==1" v-for="(item,key) of list"><button @click="changeStautsDone(key)" :key="key">完成事项</button>{{item.inputValue}} <button class="delete" @click="deleteData(key)">彻底删除</button></li>
    </ul>
    <hr>
    <h3>已经完成</h3>
    <ul>
      <li class="done" v-if="item.status==2" v-for="(item,key) of list"><button @click="changeStatusDoing(key)" :key="key">恢复进行</button>{{item.inputValue}} <button class="delete" @click="deleteData(key)">彻底删除</button></li>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      inputValue: '',
      list: [],
    }
  },

  methods: {
    handleSubmit () {
      var obj = {
        inputValue: this.inputValue,
        status: 1
      }
      if((this.inputValue).length > 0) {
        // 从 localStorage 获取 todoList 数据
        var todolist = JSON.parse(localStorage.getItem('todolist'))

        // 如果有数据，那么拿到数据，然后把新数据和原数据拼接再重新写入 localStroage
        if(todolist) {
          todolist.push(obj)
          localStorage.setItem('todolist',JSON.stringify(todolist))
        } else {

        // 如果没有数据，则直接写入 localStorage
          var arr = []
          arr.push(obj)
          localStorage.setItem('todolist',JSON.stringify(arr))
        }

        this.list.push(obj)
        this.inputValue = ''
      } else {
        alert('事件无法为空，请输入事件')
      }
    },
    changeStautsDone (key) {
      this.list[key].status = 2
      localStorage.setItem('todolist',JSON.stringify(this.list))
    },
    changeStatusDoing (key) {
      this.list[key].status = 1
      localStorage.setItem('todolist',JSON.stringify(this.list))
    },
    deleteData (key) {
      this.list.splice(key,1)
      localStorage.setItem('todolist',JSON.stringify(this.list))
    }
  },

  // mounted 钩子
  mounted () {
    var todolist = JSON.parse(localStorage.getItem('todolist'))
    if(todolist) {
      this.list = todolist
    }
  }
}
</script>

<style>
  .header {
    height: 30px;
    line-height: 30px;
    padding: 10px;
    background: orange;
    text-align: center;
  }
  li {
    list-style: none;
    width: 300px;
  }

  button {
    margin-right: 5px;
  }

  .delete {
    float: right;
  }

  .done {
    text-decoration: line-through;
  }

  .holder {
    display: inline-block;
  }
</style>
