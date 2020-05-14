# vaa-train

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


### Mutations
```
query AllAnnotations {
  listAnnotations {
    items {
      id
      concept
      annotator
    }
  }
}

mutation createAnnotations {
  ano1: createAnnotation(input: {concept: "Scotoplanes globosa", annotator: "DCline", top: 0, left: 0, width: 0 height:0 }) {id, concept, annotator }
  ano2: createAnnotation(input: {concept: "Cystechinus loveni", annotator: "DCline", top: 0, left: 0, width: 0 height:0 }) {id, concept, annotator }
}

query ConceptContainsScoto {
  listAnnotations(filter: {concept: {contains: "Scotoplanes"} }) {
    items {
      id
      concept
      annotator
    }
  }
}
```