# Vue - Axios

Axios handles API calls within vue. It can be installed vie the vue-ui within the project dependencies.
 It is preferred to have a single Axios point managing all the API calls (Instead of each session creating a new instance). Therefore, there should be an ./services/APIServices.js file that handles the session for all views and looks somewhat like this:
```js
import axios from 'axios'

const apiClient = axios.create({
  baseURL: 'Full root URL of API Server',
  withCredentials: false,
  headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json'
  }
})

export  default {
  getEvents() {
    return apiClient.get('/events')
  },
  getEvent(id) {
    return apiClient.get('/events/' + id)
  }
}
```
In the view, this would need to be imported and handled like this:
```js
<script>
import APIService from '@/services/APIService.js'

export default {
  data() {
    return {
      events: null
    }
  },
  created() { //<- A Lifecycle hook that fetches the data when the view is created
    APIService.getEvents() //<-- calling the method we wrote in our js file
      .then(response => { //<-- if the call is successful put the data into "events"
        this.events = response.data
      })
      .catch(error => { //<-- log an unseccessful call to the console
        console.log(error)
      })
  }
}
</script>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUwNTQxMDExMCwtMTMzNjE5NjI0NF19
-->