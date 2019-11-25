# vue-input-search


![vue search input](vue-input-search.gif)

## Installation
```
npm i vue-input-search
```

### Default import
Install all the components:
```javascript

import 'vue-input-search/dist/vue-search.css'
import VueSearch from 'vue-input-search/dist/vue-search.common'

const app = new Vue({
    el: '#app',
    components:{
        'vue-search':VueSearch
    }
}
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
