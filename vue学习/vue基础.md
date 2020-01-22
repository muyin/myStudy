# vue基础知识

## vue必知
1.  vue的生命周期
2.  v-bind指令：可以绑定任何有效的元素属性。该指令还可以使用简写形式`:`来代替`v-bind:`

    eg: 
    ```html
        <img v-bind:src="product.image">
        <img :src="product.image">
    ```

3.  v-text指令：将会以字符串形式展现他所引用的属性。
    
    如果需要在一个较长字符串中显示属性，可以使用Mustache语法`{{property-nam}}`来绑定属性。
    > 提示：vue允许我们在任何数据绑定中使用任何有效的js表达式。但是这在视图中引入了逻辑，
    > 把逻辑留在应用程序或负责视图数据的组件的js代码中几乎总是更好的。 

    eg:
    ```html
        <!-- sitename是data属性中的一个变量 -->
        <h1 v-text="sitename"></h1>
        <p> Welcome to {{ sitename }} </p>
        <!-- 在数据绑定中使用了js表达式 -->
        <p> Welcome to {{ (product.price*0.25) - product.title.substr(4, 4) }} </p>
    ```
4.  v-html指令：将以html形式呈现绑定属性。
    > 应当谨慎使用，并且只在绑定值可以信任时才使用。防止跨站脚本(XSS)攻击。

    eg:
    ```html
        <p v-html="product.description"></p>
        <p v-text="product.price" class="price"></p>
    ```
5.  输出过滤器(Output Filter)函数：就是接收实参，然后执行格式化任务，最后返回格式化输出值的函数。  
    函数写在Vue的filters属性下。与过滤器的绑定的通用形式`{{ property|filter }}`
    > 过滤器不能与v-text绑定语法一起使用
    
    eg:
    ```html
        <p class="price">{{ product.price | formatPrice }}</p>
    ```
    ```js
        var webstore = new Vue({
            el: '#app',
            data: {
                sitename: 'Vue.js Pet Depot',
                product: {
                    price: 2000,
                    description: 'A 25 pound bag of <em>irresistible</em>,' + 
                                'organic goodness for your cat.'
                }
            },
            filters: {
                formatPrice: function(price) {
                    // 如果无法获得整数，立即返回
                    if ( !parseInt(price) ) { return ''; }
                    // 格式化$1000及以上的值
                    if (price > 99999) {
                        var priceString = (price / 100).toFixed(2);
                        // 每三位添加一个逗号（下边这么写更清晰易懂）。例如返回$12,345.67的值
                        var priceArray = priceString.split('').reverse();
                        var index = 3;
                        while (priceArray.length > index + 3) {
                            priceArray.splice(index+3, 0, ',');
                            index += 4;
                        }
                        return '$' + priceArray.reverse().join('');            
                    } else {
                        // 如果小于$1000，则返回格式化的两位小数值
                        return '$' + (price / 100).toFixed(2);  
                    }
                }
            }
        });
    ```
6.  v-on指令：将js片段或函数绑定到DOM元素。他有一种简写形式，用`@`来代替`v-on:`。  
    绑定的方法写在methods属性下。
eg:
```html
    <button class="default" v-on:click="addToCart"> Add to cart </button>
    <button class="default" @click="addToCart"> Add to cart </button>
```
7.  计算属性：计算属性可以像实例中定义的任何其他属性一样绑定到DOM,通常他们提供从应用程序的当前状态派生出的新信息。  
    如fullName可以由firstName和lastName计算出来；矩形面积area可以由width和length计算出来。  
    计算属性的方法写在computed属性下。
    > 由于计算属性通常使用实例数据计算，因此当依赖的数据更改时，他们的返回值会自动更新。绑定到计算属性的任何实体标签都将更新以反映新值
    eg:
    ```js 中定义计算属性
        data: {
            firstName: 'Summer',
            lastName: 'Winters',
            length: 5,
            width: 3
        },
        computed: {
            fullName: function(){
                return [this.firstName, this.lastName].join(' ');   // fullName是计算属性。计算名字全称
            },
            area: function(){
                return this.length * this.width;    // area是计算属性，与data属性功能相同。计算矩形的面积
            }
        }
    ```
    ```html 使用计算属性fullName
        <div id="app">
            <p>{{ fullName }}</p>
            <p>{{ area }}</p>
        </div>
    ```
8.  监视函数(watch function)：通过监视函数来观察实例中的数据何时发生更改，以及通过使用beforeUpdate生命周期构造钩子    （应该仅在数据更改之后执行）来查看生命周期的运作。观察的属性写在watch属性对象下。 
    > * 监视函数与生命周期钩子的工作方式相同，但它们只在更新“被观察”数据时才会被触发。  
    > * 如果改变length属性，监视length的方法会执行，计算属性area的值也会跟着改变，所以监视area的方法会执行。此外因为页面上显示了area，所以页面也会跟着更新，所以监视beforeUpdate的方法也跟着会执行
    > * 如果删除页面中的{{ area }}绑定，由于没有计算属性的输出(显示)，因此不再需要更新，所有没有理由进入更新周期。beforeUpdate函数也不会被执行。也就只有监视length和area的方法执行了。

    eg:
    ```js 监视某些属性的变化
        watch: {
            length: function(newVal, oldVal){
                console.log('the old value of length was:' + oldVal +
                            '\nthe new value of length is:'+ newVal);
            },
            area: function(newVal, oldVal){
                console.log('the old value of area was:' + oldVal +
                            '\nthe new value of area is:'+ newVal);
            },
            beforeUpdate: function(){
                console.log('all those data changes happend before the output gets updated.');
            }
        }
    ```
9.  v-show指令：当值为true时，显示。为false时，隐藏(是通过将css属性display设置为内联样式none来实现)
    > 只要有可能，最好聚合多个对数据有相同反应的元素，以获得更好的性能并减少在更改时忘记保持所有元素最新的可能性。

10. v-if/v-else 指令选择显示,二选一。
    * 添加了v-else标签的元素必须保持与v-if标签**相邻**
    * v-if/v-else与v-show的区别：v-show指令使用css来隐藏或显示元素，而v-if/v-else指令从dom中移除内容。
    * 可以考虑一下是否有想要显示的后背或默认内容。v-show指令最适合没有后备(else分支)的场景。
```html
    <div class="row product" v-show="showMe">
        <!-- v-if与v-else标签必须相邻，否则vue识别不到 -->
        <div v-if="showProduct">...</div>
        <div v-else>...</div>
        <p>中国</p>
    </div>
```