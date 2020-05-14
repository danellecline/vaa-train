<template>
  <div id="app">
    <h1>Annotation App</h1>
    <input type="text" v-model="concept" placeholder="Annotation concept">
    <input type="text" v-model="annotator" placeholder="Annotation annotator">
    <input type="top" v-model="top" placeholder="Annotation top">
    <input type="left" v-model="left" placeholder="Annotation left">
    <input type="width" v-model="width" placeholder="Annotation width">
    <input type="height" v-model="height" placeholder="Annotation height">
    <button v-on:click="createAnnotation">Create Annotation</button>
    <div v-for="item in annotations" :key="item.id">
      <h3>{{ item.concept }}</h3>
      <p>{{ item.annotator }}</p>
      <p>{{ item.top }}</p>
      <p>{{ item.left }}</p>
      <p>{{ item.width }}</p>
      <p>{{ item.height }}</p>
    </div>
  </div>
</template>

<script>
import { API } from 'aws-amplify';
import { createAnnotation } from './graphql/mutations';
import { listAnnotations } from './graphql/queries';

export default {
  name: 'App',
  async created() {
    this.getAnnotations();
  },
  data() {
    return {
      concept: '',
      annotator: '',
      top: 0.,
      left: 0.,
      width: 0.,
      height: 0.,
      annotations: []
    }
  },
  methods: {
    async createAnnotation() {
      const { concept, annotator, top, left, width, height } = this;
      if (!concept || !annotator) return;
      const annotation = { concept, annotator, top, left, width, height };
      this.annotations = [...this.annotations, annotation];
      await API.graphql({
        query: createAnnotation,
        variables: {input: annotation},
      });
      this.top = 0.;
      this.left = 0.;
      this.width = 0.;
      this.height = 0.;
      this.concept = '';
      this.annotator = '';
    },
    async getAnnotations() {
      const annotations = await API.graphql({
        query: listAnnotations
      });
      this.annotations = annotations.data.listAnnotations.items;
    }
  }
}
</script>