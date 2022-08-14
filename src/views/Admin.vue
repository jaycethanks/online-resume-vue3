<template>
  <div
    v-if="hidden"
    class="w-full h-full z-50 bg-black absolute top-0 bottom-0 left-0 right-0 text-white flex justify-center items-center"
  >
    输入密码:
    <input
      autofocus
      v-model="passwd"
      @keyup.enter="handleEnter"
      class="border-0 p-2 m-2 rounded-lg text-black"
      type="password"
    />
  </div>
  <div class="about w-full h-full" v-if="!hidden">
    <md-editor class="h-full" v-model="text" @onSave="handleSave" />
  </div>
</template>
<script setup lang="ts">
import { ref, inject, toRaw } from 'vue';
import { notify } from 'notiwind';
import axios from 'axios';
let text = ref('');
let hidden = ref(true);
let passwd = ref('');
const handleEnter = () => {
  if (passwd.value.trim() === '1234') {
    hidden.value = false;
    notify(
      {
        group: 'foo',
        title: 'Success',
        text: '密码正确!',
      },
      500,
    );
  }
};
import MdEditor from 'md-editor-v3';
import 'md-editor-v3/lib/style.css';

axios({
  method: 'get',
  url: 'http://localhost:4000/get',
}).then((res) => {
  text.value = res.data.content;
});

const handleSave = (param) => {
  axios({
    method: 'post',
    url: 'http://localhost:4000/save',
    data: {
      context: text.value,
    },
  }).then((res) => {
    notify(
      {
        group: 'foo',
        title: 'Success',
        text: '保存成功!',
      },
      500,
    );
  });
};
</script>
