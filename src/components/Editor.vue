<template>
    <div ref="editorElement" style="height: 500px; width: 100%;" />
  </template>
  
  <script>
  import { ref, onMounted, onUnmounted } from "vue";
  import { createClient } from "@liveblocks/client";
  import { getYjsProviderForRoom } from "@liveblocks/yjs";
  import * as Y from "yjs";
  import { CodemirrorBinding } from "y-codemirror";
  
  import CodeMirror from "codemirror";
  import "codemirror/lib/codemirror.css";
  import "codemirror/mode/javascript/javascript";
  
  export default {
    name: "CollaborativeEditor",
    setup() {
      const editorElement = ref(null);
      const editor = ref(null);
      const binding = ref(null);
      const leave = ref(null);
  
      const client = createClient({
        publicApiKey: "pk_dev_tvVtdlRb54ocXo2bCcC7nz_sx_oF5yfEWbwg5kX39xl2SzWmzZUXGtBtuSt1l_oz",
      });
  
      const { room, leave: leaveRoom } = client.enterRoom("my-room");
      leave.value = leaveRoom;
  
      const yProvider = getYjsProviderForRoom(room);
      const yDoc = yProvider.getYDoc();
      const yText = yDoc.getText("codemirror");
  
      onMounted(() => {
        // 🐛 一定要等 DOM 挂载后再初始化编辑器
        requestAnimationFrame(() => {
          if (!editorElement.value) {
            console.error("DOM 未挂载");
            return;
          }
  
          editor.value = CodeMirror(editorElement.value, {
            value: "",
            mode: "javascript",
            lineNumbers: true,
            theme: "default",
          });
  
          binding.value = new CodemirrorBinding(
            yText,
            editor.value,
            yProvider.awareness
          );
        });
      });
  
      onUnmounted(() => {
        binding.value?.destroy();
        leave.value?.();
      });
  
      return {
        editorElement,
      };
    },
  };
  </script>
  
  <style>
  .CodeMirror {
    height: 100% !important;
  }
  </style>
  