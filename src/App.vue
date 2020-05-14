<template>
  <div id="app">
    <h1>Todo App</h1>
    <input type="text" v-model="concept" placeholder="Todo concept">
    <input type="text" v-model="annotator" placeholder="Todo annotator">
    <button v-on:click="createAnnotation">Create Todo</button>
  </div>
</template>
<template>
  <div id="app">
    <h1>App</h1>
     <div v-for="item in todos" :key="item.id">
      <h3>{{ item.concept }}</h3>
      <p>{{ item.annotator }}</p>
    </div>
  </div>
</template>

<script>
import { API } from 'aws-amplify';
import { createAnnotation } from './graphql/mutations';

export default {
  concept: 'app',
  data() {
    return {
      concept: '',
      annotator: ''
    }
  },
  methods: {
    async createAnnotation() {
      const { concept, annotator } = this;
      if (!concept || !annotator) return;
      const todo = { concept, annotator };
      await API.graphql({
        query: createAnnotation,
        variables: {input: todo},
      });
      this.concept = '';
      this.annotator = '';
    }
  }
};
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
