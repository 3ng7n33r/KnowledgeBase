# Vue 

## Installation
### Manual
Import script tag:
```html
<script  src="https://unpkg.com/vue@3.0.0-beta.12/dist/vue.global.js"></script>
```
Organize the instances within your html with:
```html
<!-- Insert the componen (the main data is directly accessible) -->
<component-name 
  :dataItemBoolean="dataItemBoolean" <!--data to transmit from main to component --> 
  @first-component-function="firstFunction" <!-- listens to componentfunction and triggers main function -->
  @second-component-function="secondFunction">
</component-name>

<!-- Import App -->
<script  src="./main.js"></script>

<!-- Import Components -->
<script  src="./components/ComponentName.js"></script>

<!-- Mount App -->
<script>
	const  mountedApp = app.mount('#app')
</script>
```
inside main.js:
```js
const app = Vue.createApp({
    data() {
        return {
            dataItemArray: [],
            dataItemBoolean: true
        }
    },
    methods: {
        firstFunction(userInput) {
            this.dataItemArray.push(userInput)
        },
        secondFunction(userInput) {
            const index = this.dataItemArray.indexOf(userInput)
                if (userInput > -1) {
                    this.dataItemArray.splice(userInput, 1)
                }
        }
    }
})
```

inside components.js:
```js
app.component('component-name', {
  props: { //outside variables emitted with .$emit and bound by :
    dataItemBoolean: {
      type: Boolean,
      required: true
    }
  },
  template: //the html implementation of the component
  /*html*/
  `<div class="css-class1">
    <div class="css-class2">
      <div class="css-class3">
	HTML implementation
      </div>
    </div>
  </div>`,
  data() {
    return {
        product: 'Socks',
        //the components data, arrays,booleans etc.
        ]
    }
  },
  methods: {
      firstComponentFunction() {
          this.$emit('first-component-function', this.variants[this.selectedVariant].userInput)
      },
      secondComponentFunction() {
        this.$emit('second-component-function', this.variants[this.selectedVariant].userInput)
      },
  },
  computed: { //data that has been worked with
      title() {
          return this.part1 + ' ' + this.part2
      }
  }
})
```

### Vue CLI

```bash 
npm install -g @vue/cli
vue create 

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc3NDE2NzkyMiw4ODE2NDU2NDgsMTkwNz
AwMTkwOSwtMTU5Mjk5OTU2N119
-->