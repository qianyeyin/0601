# JS

1. 关于`JS`的数据类型

    - 简单数据类型保存在栈中, 每次赋值的时候都是整体赋值
    ```js
    var a = 3; //除了Number, 还有String, Boolean, undefined, null和地址(复杂对象保存着地址, 实际上就是数字)
    var b = a; //这时候b也是3, 且a和b是相互独立的

    var c = { name: '张三' }
    // Array也是对象, 此时 真实数据保存在 堆中 而c中只保存着指向真实数据的地址
    var d = c; //此时d只获得了c中的地址, 所以修改d中的真实数据, c中的真实数据也会修改
    ```
    | 栈(保存着地址) |      | 堆(保存着数据)          |
    | -------- | ---- | ----------------- |
    | c        | →    | {name:'zhangsan'} |
    | d        | ↗    |                   |

    - 函数是特殊的对象

2. 操作`DOM`

    - 原生`JS` (一定是对象)

        - 创建节点 `document.createElement('p')` 相当于一个空 `p` 标签的**对象**, 而不是字符串

        - 获得节点 `document.getElementById('id')` `document.getElementsByName('属性name值')` `document.getElementsByTagName('标签名')` `document.getElementsByClassName('class名')`  `document.querySelectorAll('选择器')` `document.querySelector('选择器')` 第一个和最后一个获得的是唯一的DOM对象 而其他的都是获取一个伪数组 伪数组里面保存着DOM对象
          `element.children` `element.parentNode`
        - 操作属性
          一般的属性通过 `element.setAttribute('属性名','属性值')` 来设置,
          通过 `element.getAttribute('属性名')` 来获取
          特殊的属性:
          ①类名: 一次性修改获取: `element.className`是一个字符串, 类名通过空格分割.
          单个修改:`element.classList.addClass('class名')` `element.classList.removeClass('class名')`
          ②样式: 通过`element.style`修改 本质是一个对象, 上面有很多对应的属性, 例如`element.style.backgroundColor`
        - 移动添加节点
          `element.appendChild(子节点)`
        - 删除节点
          `父节点.removeChild(子节点)`
    - `jQuery`
        - 创建节点(无)
        - 获得节点 `$('选择器')` 一定是一个jQuery对象, 里面用数组的方式保存着一堆DOM对象
        - 操作属性 `$().attr('属性名')` `$().attr('属性名','')`
          特殊属性: 样式`$().css('样式名','样式值')`或者
        ```js
        $('#id').css({
            heigth: 30,
            backgroundColor: 'red'
        })
        // 这样的写法很常见, 可以一次性修改多个样式
        ```
        如果`$('')`有多个对象, 那么设置属性会一次性都设置, 获取属性则会获取第一个元素的属性
        - 移动添加节点
          `$().append(DOM对象或者标签字符串)`
        ```js
        $('#id').append(document.createElement('p'));
        $('#id').append('<p style="color:red">我是文字</p>');
        ```
        - 删除节点
          `$().remove()`

3. 绑定事件
    - 加载完成事件
    ```js
    window.onload = function(){} //原生JS

    $(function(){}) //jQuery
    $(document).ready(function(){}) //同上一种
    ```
    - 元素事件
    ```js
    element.onclick = function(){} //原生JS
    element.addEventListener('click',function(){}) //原生JS

    $(element).on('click',function(){}) //jQuery
    ```

    - 事件参数e 在执行事件时,会默认携带参数
    ```js
    element.onclick= = function(e) {
        // 处理兼容性
        e = e || window.event
        // e里面包含很多内容, 例如鼠标位置, 等等很多参数
        // 事件触发有三个过程: 捕获->目标->冒泡
        // e.target: 真正触发的DOM对象
        // e.currentTarget 相当于this 当前元素
        // e.clientX 浏览器宽度
        // e.clientY 浏览器高低压
        // e.pageX 鼠标距离页面顶部的距离
        // e.pageY 鼠标距离页面左边的距离
        // e.preventDefault() 阻止浏览器默认事件
        // e.stopPropagation() 阻止事件冒泡
    }
    ```
    - 滚动参数
    ```js
    document.body.scrollTop || document.documentElement.scrollTop // 原生JS

    $('body').scrollTop // jQuery
    ```

4. 尝试:
    > 写出一个三栏的tab栏  
    >
    > 点击tab栏可以做到切换效果, 特别的如果是第三个Tab栏, 把字体加粗或者变色
    ```html
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
        <style>
            * {
                margin: 0;
                padding: 0;
            }

            ul,
            ol {
                list-style: none;
            }

            .box {
                width: 300px;
                height: 500px;
                border: 1px solid #000;
                position: relative;
                margin: 50px;
                overflow: hidden;
            }

            .title {
                position: absolute;
                width: 100%;
                height: 50px;
                line-height: 50px;
                border-bottom: 1px solid #000;
                display: flex;
            }

            .tab {
                width: 33.33%;
                height: 100%;
                text-align: center;
                cursor: pointer;
            }

            .tab:hover {
                background-color: orange;
                color: #FFF;
            }

            .tab.on {
                background-color: orangered;
                color: #FFF;
            }

            .tab:not(:last-child) {
                border-right: 1px solid #000;
            }

            .content {
                width: 300%;
                height: 100%;
                padding-top: 50px;
                box-sizing: border-box;
                display: flex;
            }

            .tabContent {
                width: 33.33%;
                height: 100%;

            }

            .tabContent1 {
                background: linear-gradient(90deg, #000, #555);
            }

            .tabContent2 {
                background: linear-gradient(90deg, #555, #aaa);
            }

            .tabContent3 {
                background: linear-gradient(90deg, #aaa, #fff);
            }
        </style>
    </head>

    <body>
        <div class="box">
            <ul class="title">
                <li class="tab tab1 on">Tab1</li>
                <li class="tab tab2">Tab2</li>
                <li class="tab tab3">Tab3</li>
            </ul>
            <ul class="content">
                <li class="tabContent tabContent1"></li>
                <li class="tabContent tabContent2"></li>
                <li class="tabContent tabContent3"></li>
            </ul>
        </div>
    </body>

    </html>
    ```