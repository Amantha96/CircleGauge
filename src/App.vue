<template>
  <div id="app">
    <router-view>
      <CircleViewVue/>
    </router-view>
  </div>
</template>

<script lang="ts" type="module">
import  CircleViewVue from './views/CircleView.vue';

export default {
    name: 'App',
    components: { CircleViewVue }
};
</script>
