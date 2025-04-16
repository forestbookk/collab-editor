<template>
    <div class="app">
      <h1>Collaborative CodeMirror with Liveblocks</h1>
      <CollaborativeEditor />
    </div>
  </template>
  
  <script>
  import CollaborativeEditor from './components/Editor.vue';
  
  export default {
    components: {
      CollaborativeEditor
    }
  };
  </script>
  