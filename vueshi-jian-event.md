最简单的点击事件

```
<button v-on:click="increase">Click me</button>
<p>{{ counter }}</p>
```

JS

```
<script>
	var app = new Vue({
  		el: '#app',
  		data: {
               counter:0,
               },
  		methods: {
                increase: function() {
                  this.counter++;
                 }
</script>
```

第二种 passing own arguments with events

HTML

```
<button v-on:click="increase(2, $event)">Click me</button>
<p>{{ counter }}</p>
```

```
<script>
	var app = new Vue({
  		el: '#app',
  		data: {
               counter:0,
               },
  		methods: {
                increase: function(step, event) {
                  this.counter += step;
                 }
</script>
```



