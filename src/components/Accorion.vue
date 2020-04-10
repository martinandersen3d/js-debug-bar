<script>
// import MyComponent from '@/components/file.vue'
// import { MyMixin } from '@/components/file.vue'
// import Vue from 'vue'
// import {eventBus} from '@/main.js'
// import router from '@/router'
export default {
    // components: {MyComponent},
    // mixins: [MyMixin],
    // use camelCase for names in props, use kebab-case for html-attributes
    props: ['title'],
    // mounted() {},
    // watch: {},
    // computed: {},

    methods: {
        todo() {

        }
    }, //__________________________________________

    data() {
        return {
            open: false,
        };
    },
}
</script>

<template><div class='Accorion '>
<button class="accordion" @click="open = !open" >{{ title }}</button>
<div class="panel" v-if="open">
<slot></slot>
</div>

</div>
</template>

<style lang="css" scoped>
.accordion {
  background-color: #eee;
  color: #444;
  cursor: pointer;
  padding: 18px;
  width: 100%;
  text-align: left;
  border: none;
  outline: none;
  transition: 0.4s;
}

/* Add a background color to the button if it is clicked on (add the .active class with JS), and when you move the mouse over it (hover) */
.active, .accordion:hover {
  background-color: #ccc;
}

/* Style the accordion panel. Note: hidden by default */
.panel {
  padding: 0 18px;
  background-color: white;
  overflow: hidden;
}
</style>
