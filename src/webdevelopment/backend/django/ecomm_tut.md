# Ecomm tut

[Link](https://www.youtube.com/watch?v=Yg5zkd9nm6w&list=WL&index=70)
## Setup and install Django

    create repo
    clone repo
    create virtualenv: virtualenv env
    source env/bin/activate
    pip install django
    pip install django-rest-framework
    pip install django-cors-headers
    pip install djoser
    pip install pillow < -- image library
    pip install stripe
    django-admin startproject djackets_django
add to setting.py -> installed apps:

    'rest_framework',
    'rest_framework.authtoken',
    'corsheaders',
    'djoser',

configure cors (add to settings.py):

    CORS_ALLOWED_ORIGINS = [
    	"http://localhost:8080",
    ]

configure middleware:

    'corsheaders.middleware.CorsMiddleware',
    #has to be above commonmiddleware

configure urls:

    from django.urls import include
    urlpatterns = [
    ...
    path('api/v1/', include('djoser.urls')),
    path('api/v1/', include('djoser.urls.authtoken')),

makemigrations - migrate - creatsuperuser - Done!

## Install and setup vue
install vue on the pc if necessary:

    npm install -g @vue/cli

start project:

    vue create djackets_vue

Install packages:

    cd djackets_vue
    npm install axios <- make API calls
    npm install bulma <- CSS framework

add FontAwesome to public/index.html:

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.2/css/all.min.css">

replace style in app.vue with:

    @import  '../node_modules/bulma';
    and replace navbar according to vid @18:30

## Create Django app

python manage.py startapp product

create models ...
### Creating thumbnails of images:
To create thumbnails of images, you need to import the Pillow library, BytesIO and the File wrapper:

    from django.core.files import File
    from io import BytesIO
    from PIL import Image

and add this function to the corresponding model:

    def make_thumbnail(self, image, size=(300, 200)):
	    img = Image.open(image)
	    img.convert('RGB')
	    img.thumbnail(size)
	    thumb_io = BytesIO()
	    img.save(thumb_io, 'JPEG', quality=85)
	    thumbnail = File(thumb_io, name=image.name)
	    return thumbnail
and add media to settings.py:

    MEDIA_URL = '/media/'
    MEDIA_ROOT = BASE_DIR / 'media'
 and to urls.py:

    from django.conf import settings
    from django.conf.urls.static import static
    ...
    
    urlpatterns = [
	...
    ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

### creating the API view


    from rest_framework.views import APIView
    from rest_framework.response import Response
    
    from .models import Product
    from .serializers import ProductSerializer
    
    class LatestProductsList(APIView):
        def get(self, request, format=None):
            products = Product.objects.all()[:4]
            serializer = ProductSerializer(products, many=True)
            return Response(serializer.data)
and add that view to products/urls.py (keep links for apps seperated):

    from django.urls import path, include
    from product import views
    
    urlpatterns = [
        path('latest-products/' ,views.LatestProductsList.as_view())    
    ]
and don't forget to add a link to the apps urls into the projects urls.py:

    urlpatterns = [
    	...
        path('api/v1/', include('product.urls')),
    ] ...

## Create Vue frontpage
Create the page and import the data with axios. For that:

 1. Create an empty data array
 2. create a method with an axios get request to consume the api
 3. create a mounted lifecycle hook to import the data

```js
<script>
import axios from 'axios' //<- import the module
export default {
  name: 'Home',
  data() {
    return {
      latestProducts: [] //empty Array
    }
  },
  components: {
  }
  mounted() { //<-- Lifecycle hook calling the method when DOM is mounted
    this.getLatestProducts()
  },
  methods: {
    getLatestProducts() { //<- the axios get request
      axios
        .get('/api/v1/latest-products')
        .then(response => {
          this.latestProducts = response.data
        })
        .catch(error => {
          console.log(error)
        })
    }
  }
}
</script>
```
It won't work yet because the URL is unknown and axios needs to be imported into main.js:

    import axios from 'axios'
    
    axios.defaults.baseURL = 'http://127.0.0.1:8000'
    
    createApp(App).use(store).use(router, axios).mount('#app')

### Product detail page
**Backend**
First, create the API endpoint for the product detail on the backend. views.py:
```python
class ProductDetail(APIView):
    def get_object(self, category_slug, product_slug):
        try:
            return Product.objects.filter(category__slug=category_slug).get(slug=product_slug)
        except Product.DoesNotExist:
            raise Http404
    
    def get(self, category_slug, product_slug, format=None):
        product = self.get_object(category_slug, product_slug)
        serializer = ProductSerializer(product)

        return Response(serializer.data)
 ```
 **Frontend**
 add product.vue to vews folder. Check template [here](https://github.com/SteinOveHelset/djackets_vue/blob/master/src/views/Product.vue) 
 and add the products page to the view router -> index.js:
 ```js
 import  Product  from  '../views/Product.vue'
const routes = [
...
  {
    path: '/:category_slug/:product_slug',
    name: 'Product',
    component: Product
  }
]
```
### Slugs
The way the links in the URL work is like this:
They start in the backend with the models get_absolute_url method.
This function creates a string out of the category and the name and transforms it into slugs and into a link. 
The link i ś then shown on the front page which loops through every unique object. If the link is clicked, the slug gets taken in from the router and is then passed on as an argument in the detail view. The detail view calls the detail backend API using the slugs to identify the item to the backend. The backend delivers further details to the product view with an axios call.

 1. Backend function get_absolute_url in model to create a url to the db entry made out of slugs
 2. Give a list of all objects to the main page in Vue, loop through this list and create a card with a link containing the frontend url out of the slugs of each object
 3. If a link is clicked, the product page is displayed. Here, the vue router gives the slugs from the url to the vue-view. The view method calls the backend api using the same slugs to create the link.
 4. The backend API gives the details of that single object to the vue-view which uses it to create the product detail page.
 
## State management with vue x
[VueX](https://vuex.vuejs.org/#what-is-a-state-management-pattern)
Create the store functionality in store/index.js:
```js
import { createStore } from 'vuex'

export default createStore({
  state: {
    cart: {
      items: [],
    },
    isAuthenticated: false,
    token: '',
    isLoading: false
  },
  mutations: {
    initializeStore(state) {
      if (localStorage.getItem('cart')) {
        state.cart = JSON.parse(localStorage.getItem('cart'))
      } else {
        localStorage.setItem('cart', JSON.stringify(state.cart))
      }
    },
    addToCart(state, item) {
      const exists = state.cart.items.filter(i => i.product.id === item.product.id)

      if (exists.length) {
        exists[0].quantity = parseInt(exists[0].quantity) + parseInt(item.quantity)
      } else {
        state.cart.items.push(item)
      }

      localStorage.setItem('cart', JSON.stringify(state.cart))
    }
  },
  actions: {
  },
  modules: {
  }
})
```

and add the store to the App.vue:
```js
<script>
  export default{
    data (){
      return {
	...
        cart: {
          items: [],
        }
      }
    },
    beforeCreate() {
      this.$store.commit('initializeStore') //commit calls mutations defined in the store file
    }
    
  }
</script>
```
to make it possible to add an item to the cart, this needs to be implemented in the Product page. First, a button is added with the functionality:
```js
...
	<div class="control">
	    <a class="button is-dark" @click="addToCart()">Add to cart</a>
	</div>
...


<script>
...
export default {
    name: 'Product',
    data() {
        return {
            product: {},
            quantity: 1
        }
    },
...
    methods: {
...
        addToCart() {
            if (isNaN(this.quantity) || this.quantity < 1) {
                this.quantity = 1
            }
            const item = {
                product: this.product,
                quantity: this.quantity
            }
            this.$store.commit('addToCart', item)
        }
    }
}
</script>
```
and make sure the cart is initialised as variable in app.vue:
```js
<script>
...
    mounted() {
      this.cart = this.$store.state.cart
    },
    computed: {
      cartTotalLength() {
        let totalLength = 0

        for (let i = 0; i < this.cart.items.length; i++) {
          totalLength += this.cart.items[i].quantity
        }

        return totalLength 
      }
    }
    
  }
</script>
```
To give the add to cart button some responsibility, we will use a "toast" popup that shows that an item has been added. Bulma has such an extension that can be installed with:

    $ npm install bulma-toast
and add the toast functionality to the product view:
```js
<script>
...
import { toast } from 'bulma-toast'
export default {
	...
    methods: {
		...
            toast({
                message: 'The product was added to the cart',
                type: 'is-success',
                dismissible: true,
                pauseOnHover: true,
                duration: 2000,
                position: 'bottom-right',
            })
...
</script>
```

### add a loading bar while FE communicates with BE server
in the store/index.js add a state isLoading and the functionailty to use it:
```js
import { createStore } from 'vuex'

export default createStore({
  state: {
	...
    isLoading: false
  },
  mutations: {
	...
    setIsLoading(state, status) {
      state.isLoading = status
    }
   ```
and set that status to true as soon as soon as the product page requests the details from the server (views/products.vue). To make sure that loading false is not called before the axios call is actually finished, make sure to set the function to **async** and add **await** to the axios call:
```js
<script>
...
export default {
...
    methods: {
        async getProduct() {
            this.$store.commit('setIsLoading', true)
		...await axios call
            this.$store.commit('setIsLoading', false)
        },
```
and add the loading symbol to the app.vue:
```js
    <div class="is-loading-bar has-text-centered" :class="{'is-loading': $store.state.isLoading}">
      <div class="lds-dual-ring"></div>
    </div>
```

### Create a category view
To create the category view we go the same way as for the detail view:

 - Create view file Category.vue
 - Within, create the template, the axios call etc.
 - Create the entry in the router/index.js

Important: 
If we now change from Winter to Summer, the page will  not change as they have a similar url ('category-slug') so the mounted lifecycle hook will not be called. To make navigation between similiarly named, dynamically created routes possible there is an option called "watch". This has to be added to the "Category.vue":
```js
<script>
...
export default {
	...
    watch: {
        $route(to, from) {
            if (to.name === 'Category') {
                this.getCategory()
            }
        }
    },
```

## Add search function
### Backend
First, we add an Endpoint in our backend for the search functionality. In our views.py we add:
```py
from django.db.models import Q #Improves query functionality (& and , | or )
from rest_framework.decorators import api_view #the decorator to only use this function with a POST request
....
@api_view(['POST'])
def search(request):
    query = request.data.get('query', '')

    if query:
        products = Products.objects.filter(Q(name__icontains=query) | Q(description__icontains=query))
        serializer = ProductSerializer(products, many=True)
        return Response(serializer.data)
    else:
        return Response({'products': []})
```
and add the entry to the urls.py file:
```py
urlpatterns = [
...
    path('products/search', views.search),
    ...
]
```
### Frontend
add a searchbar to the navbar in App.vue:
```js
<form method="get" action="/search">
  <div class="field has-addons">
    <div class="control">
      <input type="text" class="input" placeholder="What are you looking for?" name="query">
    </div>
    <div class="control">
      <button class="button is-success">
        <span class="icon">
          <i class="fas fa-search"></i>
        </span>
      </button>
    </div>
  </div>
</form>
```
and create a landing page for that site that displays the results:
```js
<template>
... 
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox.vue'

    export default {
        name: 'Search',
        components: {
            ProductBox
        },
        data() {
            return {
                products: [],
                query: '',
            }
        },
        mounted() {
            document.title = 'Search | Djackets '

            let uri = window.location.search.substring(1) // I don't get it, to look into
            let params = new URLSearchParams(uri) //don't get it neither

            if (params.get('query')) {
                this.query = params.get('query')

                this.performSearch()
            }
        },
        methods: {
            async performSearch () {
                this.$store.commit('setIsLoading', true)

                await axios
                    .post('/api/v1/products/search/', {'query': this.query})
                    .then(response => {
                        this.products = response.data
                    })
                    .catch(error => {
                        console.log(error)
                    })
                
                this.$store.commit('setIsLoading', false)
            }
        }
    }
</script>
```
add that page to the router:
```js
import Search from '../views/Search.vue'

const routes = [
	...
  {
    path: '/search',
    name: 'Search',
    component: Search
  },
]
```
Done!

## Add a cart
Kind of the same thing by now. Add a view and a router entry. In this instance, we want to display each item in a particular way so it makes sense to create a component for this too. The most important parts are the functionality sections of the site and the component. Cart.vue:
```js
<template>
    <div class="page-cart">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Cart</h1>
            </div>

            <div class="column is-12 box">
                <table class="table is-fullwidth" v-if="cartTotalLength">
                    <thead>
                        <tr>
                            <th>Product</th>
                            <th>Price</th>
                            <th>Quantity</th>
                            <th>Total</th>
                            <th></th>
                        </tr>
                    </thead>

                    <tbody>
                        <CartItem
                            v-for="item in cart.items"
                            v-bind:key="item.product.id"
                            v-bind:initialItem="item"
                            v-on:removeFromCart="removeFromCart" />
                    </tbody>
                </table>

                <p v-else>You don't have any products in your cart...</p>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">Summary</h2>

                <strong>${{ cartTotalPrice.toFixed(2) }}</strong>, {{ cartTotalLength }} items

                <hr>

                <router-link to="/cart/checkout" class="button is-dark">Proceed to checkout</router-link>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import CartItem from '@/components/CartItem.vue'
export default {
    name: 'Cart',
    components: {
        CartItem
    },
    data() {
        return {
            cart: {
                items: []
            }
        }
    },
    mounted() {
        this.cart = this.$store.state.cart
    },
    methods: {
        removeFromCart(item) {
            this.cart.items = this.cart.items.filter(i => i.product.id !== item.product.id)
        }
    },
    computed: {
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        },
        cartTotalPrice() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.product.price * curVal.quantity
            }, 0)
        },
    }
}
</script>
```
and the component CartItem.vue:
```js
<template>
    <tr>
        <td><router-link :to="item.product.get_absolute_url">{{ item.product.name }}</router-link></td>
        <td>${{ item.product.price }}</td>
        <td>
            {{ item.quantity }}
            <a @click="decrementQuantity(item)">-</a>
            <a @click="incrementQuantity(item)">+</a>
        </td>
        <td>${{ getItemTotal(item).toFixed(2) }}</td>
        <td><button class="delete" @click="removeFromCart(item)"></button></td>
    </tr>
</template>

<script>
export default {
    name: 'CartItem',
    props: {
        initialItem: Object
    },
    data() {
        return {
            item: this.initialItem
        }
    },
    methods: {
        getItemTotal(item) {
            return item.quantity * item.product.price
        },
        decrementQuantity(item) {
            item.quantity -= 1
            if (item.quantity === 0) {
                this.$emit('removeFromCart', item)
            }
            this.updateCart()
        },
        incrementQuantity(item) {
            item.quantity += 1
            this.updateCart()
        },
        updateCart() {
            localStorage.setItem('cart', JSON.stringify(this.$store.state.cart))
        },
        removeFromCart(item) {
            this.$emit('removeFromCart', item)
            this.updateCart()
        },
    },
}
</script>
```
Done!

## Signup
### Frontend
Create a sign-up page on the frontend. This one has forms with simple validation and a connection to the backend:
```js
<template>
    <div class="page-sign-up">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Sign up</h1>

                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Username</label>
                        <div class="control">
                            <input type="text" class="input" v-model="username">
                        </div>
                    </div>

                    <div class="field">
                        <label>Password</label>
                        <div class="control">
                            <input type="password" class="input" v-model="password">
                        </div>
                    </div>

                    <div class="field">
                        <label>Repeat password</label>
                        <div class="control">
                            <input type="password" class="input" v-model="password2">
                        </div>
                    </div>

                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-dark">Sign up</button>
                        </div>
                    </div>

                    <hr>

                    Or <router-link to="/log-in">click here</router-link> to log in!
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'
export default {
    name: 'SignUp',
    data() {
        return {
            username: '',
            password: '',
            password2: '',
            errors: []
        }
    },
    methods: {
        submitForm() {
            this.errors = []
            if (this.username === '') {
                this.errors.push('The username is missing')
            }
            if (this.password === '') {
                this.errors.push('The password is too short')
            }
            if (this.password !== this.password2) {
                this.errors.push('The passwords don\'t match')
            }
            if (!this.errors.length) {
                const formData = {
                    username: this.username,
                    password: this.password
                }
                axios
                    .post("/api/v1/users/", formData)
                    .then(response => {
                        toast({
                            message: 'Account created, please log in!',
                            type: 'is-success',
                            dismissible: true,
                            pauseOnHover: true,
                            duration: 2000,
                            position: 'bottom-right',
                        })
                        this.$router.push('/log-in')
                    })
                    .catch(error => {
                        if (error.response) {
                            for (const property in error.response.data) {
                                this.errors.push(`${property}: ${error.response.data[property]}`)
                            }
                            console.log(JSON.stringify(error.response.data))
                        } else if (error.message) {
                            this.errors.push('Something went wrong. Please try again')
                            
                            console.log(JSON.stringify(error))
                        }
                    })
            }
        }
    }
}
</script>
```
the log in page will be almost identical.
For the state management on the fron end side (logged in , logged out) we have to add an option to initializeStore to set the isAuthenticated state in the store/index.js file that checks if a session token is present. Also, we need a function to set the token. store/index.js:
```js
  mutations: {
    initializeStore(state) {
	...
      if (localStorage.getItem('token')) {
        state.token = localStorage.getItem('token')
        state.isAuthenticated = true
      } else {
        state.token = ''
        state.isAuthenticated = false
      }
    },
    ...
    setToken(state, token) {
      state.token = token
      state.isAuthenticated = true
    }
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTczMTY3Nzg2XX0=
-->