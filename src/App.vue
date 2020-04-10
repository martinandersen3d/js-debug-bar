<script>
// https://medium.com/@gilfink/quick-tip-creating-an-xmlhttprequest-interceptor-1da23cf90b76
// import MyComponent from '@/components/file.vue'
// import { MyMixin } from '@/components/file.vue'
// import Vue from 'vue'
// import {eventBus} from '@/main.js'
// import router from '@/router'
// import fetchIntercept from 'fetch-intercept';
const FetchInterceptor = require('fetch-interceptor');
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
        if (n !== undefined && n !== null) {
            this.tabActive = n
        }

        // FETCH --------------------------------------

        // Register interceptor hooks
        FetchInterceptor.register({
            onBeforeRequest(request) {
                // Hook before request
            },
            onRequestSuccess(response, request) {
                // Hook on response success
                var responseClone = response.clone();
                // let data = responseClone.text().then(data => {
                let data = responseClone.json().then(data => {

                    console.log('FetchInterceptor: ', data)
                })
                // console.log('NANAN: ', data)
                return response

            },
            onRequestFailure(response, request) {
                // Hook on response failure
            }
        });
        // HXR --------------------------------------

  (function() {
    var XHR = XMLHttpRequest.prototype;
    var send = XHR.send;
    var open = XHR.open;
    XHR.open = function(method, url) {
        this.url = url; // the request url
        return open.apply(this, arguments);
    }
    XHR.send = function() {
        this.addEventListener('load', function() {
           // if (this.url.includes('<url-you-want-to-intercept>')) {
             let response = JSON.parse(this.response);
            //  console.log('XRX Intercept: ',  response)
             console.log('XRX Intercept: ',  response)
                // var dataDOMElement = document.createElement('div');
                // dataDOMElement.id = '__interceptedData';
                // dataDOMElement.innerText = this.response;
                //dataDOMElement.style.height = 0;
               //dataDOMElement.style.overflow = 'hidden';
                // document.body.appendChild(dataDOMElement);
            //}               
        });
        return send.apply(this, arguments);
    };
  })();



        // --------------------------------------

    },
    // watch: {},
    // computed: {},

    methods: {
        setTab(n) {
            this.tabActive = n
            localStorage.setItem('JSDEBUGBAR.TABINDEX', this.tabActive)
        },

        fetchGet() {
            fetch('https://jsonplaceholder.typicode.com/todos/1')
                .then(response => response.json())
                .then(json => {
                    console.log('Fetch > getDate:', json)
                })
        },

        hxrGet() {
            let req = new XMLHttpRequest();
            req.open("GET", 'https://jsonplaceholder.typicode.com/todos/1', true);
            req.send();
            req.onload = function () {
                let json = JSON.parse(req.responseText);
                console.log('hxrGet: ', json)
            };
        },

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
        <button class='JSD_BTN' @click="fetchGet()">Fetch GET</button>
        <button class='JSD_BTN' @click="hxrGet()">Hxr GET</button>
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

        <div v-if="tabActive === 1" id="Paris" class="jsd_tabcontent">
            <h3>Paris</h3>
            <p>Paris is the capital of France.</p>
        </div>

        <div v-if="tabActive === 2" id="Tokyo" class="jsd_tabcontent">
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
    background-color: rgba(0, 0, 0, 0.25);
    border-radius: 100%;
    width: 76px;
    height: 80px;
    color: white;

    align-items: center;
    justify-content: center;
    text-align: center;
    display: flex;
    cursor: pointer;

    transition: background-color 0.15s ease;

}

.jsd_circle:hover {
    background-color: rgba(0, 0, 0, 0.7);

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

.JSD_BTN {
    padding: 20px;
}
</style>
