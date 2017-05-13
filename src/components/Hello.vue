<template>
  <div class="hello">
    <h1>Origin message is: {{ msg + ' zjfsharp' }}</h1>
    <h1>The reverse message is: {{reverseMsg}}</h1>
    <!-- <h1>{{ msg.split('').reverse().join('') }}</h1> -->
    <h2>Essential Links</h2>
    <p v-if="seen">now see me</p>
    <span v-bind:title="msg2">zjfsharp</span>

    <ol>
      <li v-for="todo in todos">
        <a href="#">{{ todo.text }}</a>
      </li>
    </ol>

    <button v-on:click="newLi">新增</button>
    <button v-on:click="sendMsg">逆转消息</button>
    <br><br>
    <input v-model="msg"/>

    <ol>
      <todo-item v-for="item in groceryList" v-bind:todo='item'></todo-item>
    </ol>

    <span>Full name is: {{fullname}}</span>
    <br>
    <div id="namesetter">
      <input v-model="firstname" placeholder="firstname">
      <input v-model="lastname" placeholder="lastname">
    </div>
    <p>
      Ask a Yes/No question: &nbsp;
      <input v-model="question" placeholder="question">
    </p>
    <p>{{answer}}</p>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Vue.js',
      seen: true,
      msg2: 'loading:' + new Date(),
      firstname: 'zjf',
      lastname: 'sharp',
      todos: [
        {text: '学习 JavaScript'},
        {text: '学习 Vue'},
        {text: '整个牛项目'}
      ],
      groceryList: [
        {text: '奶酪'},
        {text: '蔬菜'},
        {text: '随便其他什么人吃的东西'}
      ],
      question: '',
      answer: 'I cannot give you an answer until you ask a question!'

    }
  },
  watch: {
    question: function (newQuestion) {
      this.answer = 'Waiting for you to stop typing......'
      this.getAnswer()
    }
  },
  computed: {
    reverseMsg: function () {
      return this.msg.split('').reverse().join('')
    },
    fullname: {
      set: function (newName) {
        var names = newName.split(' ')
        this.firstname = names[0]
        this.lastname = names[1]
      },
      get: function () {
        return this.firstname + ' ' + this.lastname
      }
    }
  },
  methods: {
    sendMsg: function () {
      this.msg = this.msg.split('').reverse().join('')
    },
    newLi: function () {
      this.todos.push({text: 'HTML/CSS/JS'})
    },
    getAnswer: _.debounce(
      function () {
        var vm = this
        if (this.question.indexOf('?') === -1) {
          vm.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        vm.answer = 'Thinking...'
        this.$http.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      // 这是我们为用户停止输入等待的毫秒数
      500
    )
  }

}

import Vue from 'vue'
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

#namesetter{
  margin-top: 10px;
}
</style>
