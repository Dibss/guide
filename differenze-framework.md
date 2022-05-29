# Direttiva Show

The show directive shows the specified HTML element if the expression evaluates to true, otherwise the HTML element is hidden.

## Vue

```v-show=""```

## Angular

```ng-show=""```

# APP Init

## Vue 2

```html
<div id="app"> 
</div>
```
```javascript
var app = new Vue({
  el : "#app",
  data : {
    //data
  },
  methods : {
    //functions
  }
})
```

## Vue 3

```html
<div id="app"> 
</div>
```
```javascript
Vue.createApp({
  data() {
    return {
      //data
    }
  },
  methods : {
    //functions
  }
}).mount('#app')
```



## Angular

```html
<div ng-app="app" ng-controll="ctrl"> 
</div>
```
```javascript
var app = angular.module('app', []);
app.controller('ctrl', function($scope){
  $scope.nomeVariabile = '';
})
```




# Repeating HTML elements

## Vue

```v-for=""```

## Angular

```ng-repeat="x in array"```
