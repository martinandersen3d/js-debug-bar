<script>
// https://medium.com/@gilfink/quick-tip-creating-an-xmlhttprequest-interceptor-1da23cf90b76
// import MyComponent from '@/components/file.vue'
// import { MyMixin } from '@/components/file.vue'
// import Vue from 'vue'
// import {eventBus} from '@/main.js'
// import router from '@/router'
export default {
    name: 'App',
    // components: {
    //     // MyComponent
    // },
    // mixins: [MyMixin],
    // use camelCase for names in props, use kebab-case for html-attributes
    // props: {},
    mounted() {
        let n = localStorage.getItem('JSDEBUGBAR.TABINDEX')
        if( n !== undefined && n !== null ){
            this.tabActive = n
        }
        // -------------------
        let oldXHROpen = window.XMLHttpRequest.prototype.open;
        window.XMLHttpRequest.prototype.open = function(method, url, async, user, password) {
         // do something with the method, url and etc.
         this.addEventListener('load', function() {
          // do something with the response text
          console.log('load: ' + this.responseText);
         });

         return oldXHROpen.apply(this, arguments);
        }
    },
    // watch: {},
    // computed: {},

    methods: {
        setTab(n) {
            this.tabActive = n
            localStorage.setItem('JSDEBUGBAR.TABINDEX', this.tabActive)
        }
    }, //__________________________________________

    data() {
        return {
            tabActive: 0,
            test: ""
        };
    },
}
</script>

<template>
<div class='JSDebugBar '>

    <div class='JSTabWrapper'>

        <!-- jsd_tab links -->
        <div class="jsd_tab">
          <button class="jsd_tablinks" @click="tabActive=0" :class="tabActive === 0 ? 'jsd_active' :''">London</button>
          <button class="jsd_tablinks" @click="tabActive=1" :class="tabActive === 1 ? 'jsd_active' :''">Paris</button>
          <button class="jsd_tablinks" @click="tabActive=2" :class="tabActive === 2 ? 'jsd_active' :''">Tokyo</button>
        </div>

        <!-- jsd_tab content -->
        <div v-if="tabActive === 0" id="London" class="jsd_tabcontent">
          <h3>London</h3>
          <p>London is the capital city of England.</p>
        </div>

        <div v-if="tabActive === 1"  id="Paris" class="jsd_tabcontent">
          <h3>Paris</h3>
          <p>Paris is the capital of France.</p>
        </div>

        <div v-if="tabActive === 2"  id="Tokyo" class="jsd_tabcontent">
          <h3>Tokyo</h3>
          <p>Tokyo is the capital of Japan.</p>
        </div>
          
    </div>

    <!-- LEFT DEBUG BUTTON -->
    <div class='jsd_circle_wrapper noselect' id=''>
        <div class='jsd_circle' id=''>
            <div class='jsd_circle_content' id=''>
                ☰
            </div>
        </div>
    </div>
</div>
</template>

<style lang="css" scoped>

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
/* ---------------------------------- */
.jsd_circle {
    background-color: rgba(0, 0, 0, 0.4);
    border-radius: 100%;
    width: 76px;
    height: 80px;
    color: white;

    align-items: center;
    justify-content: center;
    text-align: center;
    display: flex;
    cursor: pointer;

}

.jsd_circle_content {
    margin-top: -4px;
    margin-right: -34px;
    font-size: 22px;
}

.jsd_circle_wrapper {
    position: absolute;
    top: 50%;
    left: -40px;
    margin: auto;

}
/* TABS ------------------------------------------------------- */
/* Style the jsd_tab */
.jsd_tab {
  overflow: hidden;
  border: 1px solid #ccc;
  background-color: #f1f1f1;
}

/* Style the buttons that are used to open the jsd_tab content */
.jsd_tab button {
  background-color: inherit;
  float: left;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 14px 16px;
  transition: 0.3s;
}

/* Change background color of buttons on hover */
.jsd_tab button:hover {
  background-color: #ddd;
}

/* Create an active/current jsd_tablink class */
.jsd_tab button.jsd_active {
  background-color: #ccc;
}

/* Style the jsd_tab content */
.jsd_tabcontent {
  padding: 6px 12px;
  border: 1px solid #ccc;
  border-top: none;
}
/* OTHER ------------------------------------------------------- */
.noselect {
    -webkit-touch-callout: none;
    /* iOS Safari */
    -webkit-user-select: none;
    /* Safari */
    -khtml-user-select: none;
    /* Konqueror HTML */
    -moz-user-select: none;
    /* Old versions of Firefox */
    -ms-user-select: none;
    /* Internet Explorer/Edge */
    user-select: none;
    /* Non-prefixed version, currently
                                  supported by Chrome, Opera and Firefox */
}
</style>
