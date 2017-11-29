<template>
  <div id="app">
    <h1 v-text="title"></h1>
    <input v-model="newItem" v-on:keyup.enter="addNew">
    <ul>
      <li v-for="item in items" v-on:click="toggleFinish(item)" v-bind:class="{finished: item.isFinished}">
          {{item.label}}
      </li>
    </ul>
  </div>
</template>

<script>
import Store from './store'

export default {
  name: 'app',
  data (){
    return {
      title : 'this is a todo list',
      items : Store.fetch(),
      newItem : ''
    }
  },
  methods :{
    toggleFinish : function(item){
      item.isFinished = !item.isFinished;
    },
    addNew : function(){
      this.items.push({
        label : this.newItem,
        isFinished: false
      });
      this.newItem = '';
    }
  },
  watch:{
    items : {
      handler: function(items){
        Store.save(items);
      },
      deep: true
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

html {
  height: 100%;
}

.finished {
  text-decoration: underline;
}
</style>
