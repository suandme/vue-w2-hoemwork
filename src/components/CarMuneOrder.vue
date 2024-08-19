<script setup>
import {defineProps,defineEmits} from 'vue'
const props=defineProps(['list-todos'])

//內層傳外層資料
const emit=defineEmits(['emit-addCart']);
emit("emit-addCart");
</script>
<template>
      <div class="list-group" v-for="todos in listTodos" :key="todos.id" >
          <a href="#" @click.prevent="emit('emitAddCart',todos)" class="list-group-item list-group-item-action">
            <div class="d-flex w-100 justify-content-between">
              <h5 class="mb-1">{{todos.name}}</h5>
              <small>${{ todos.price }}</small>
            </div>
            <p class="mb-1">{{todos.description}}</p></a>
        </div>
</template>