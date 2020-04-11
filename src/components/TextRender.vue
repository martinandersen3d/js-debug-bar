<script>
// import MyComponent from '@/components/file.vue'
// import { MyMixin } from '@/components/file.vue'
// import Vue from 'vue'
// import {eventBus} from '@/main.js'
// import router from '@/router'
import { flatten } from 'flattenjs'
export default {
   
    // mixins: [MyMixin],
    // use camelCase for names in props, use kebab-case for html-attributes
    props: ['itemProp', 'active'],
    // mounted() {},
    // watch: {},
    computed: {
        arrOfKeys() {
            let data = this.itemProp.data;
            let newObj = flatten(data)
            // console.log(newObj)

            let temp = newObj
            let strArr = []
            for (let key in temp) {
                let value = temp[key];
                strArr.push( this.textRender(key, value) )
            }
            return strArr
        }, 
        state(){
            return parseInt(this.active)
        }
    },

    methods: {
        textRender( k , v ){
            if( this.state === 0 ){ return `${k} = ${v}`  }
            if( this.state === 1 ){ return `'${k}': ${v},`  }
            if( this.state === 2 ){ return `"${k}": "${v}",`  }
            if( this.state === 3 ){ return `let '${k}' = ${v}`  }
            if( this.state === 4 ){ return `"${k}" => ${v}`  }
            if( this.state === 5 ){ return `${k}`  }
            if( this.state === 6 ){ return `${v}`  }
            if( this.state === 7 ){ return `${k} ${v}`  }
            if( this.state === 8 ){ return `${this.textVmodel(k)}`  }
            if( this.state === 9 ){ return `${this.textDotNotation(k)}`  }
            if( this.state === 10 ){ return `${k} || ${v}`  }
            if( this.state === 11 ){ return `${k} || ${v}`  }
            if( this.state === 12 ){ return `${k} || ${v}`  }
            if( this.state === 13 ){ return `${k} || ${v}`  }
            if( this.state === 14 ){ return `${k} || ${v}`  }
            if( this.state === 15 ){ return `${k} || ${v}`  }
            if( this.state === 16 ){ return `${k} || ${v}`  }
            if( this.state === 17 ){ return `${k} || ${v}`  }
            if( this.state === 18 ){ return `${k} || ${v}`  }
            
        }, 
        textVmodel(key){
            let bracketRemoved = this.removeBracketWithNumbers(key);
            let a = bracketRemoved.split('.')
            let newArr = []
            let counter = 0
            for( let item of a){
                
                    newArr.push(`['${item}']`)
                
                counter++;
            }
            console.log(newArr)
            return newArr.join('')
        },
        textDotNotation(key){
            return this.removeBracketWithNumbers(key);
            
        },
        removeBracketWithNumbers( s ){
            
            const regex = /\[[0-9]+]/gi;

            return s.replace(regex, '');
        }
    }, //__________________________________________

    data() {
        return {
            // state: 5
        };
    },
}
</script>

<template><div class='Vmodel '>
   <div v-for="( item, index) in arrOfKeys" :key="index">
          {{ item }} 
   </div>
</div>
</template>

<style lang="css" scoped>

</style>
