
<template>
  　　<div id="app">
  　　　　<zi @newNodeEvent="parentLisen" />
  　　</div>
</template>

<script>
  import zi from './zi'
  export default {
    name: 'app',
    'components': {
      zi
    },
    methods: {
      parentLisen(evtValue) {
        //evtValue 是子组件传过来的值
        alert(evtValue)
      }
    }
  }
</script>
