#### 03-Update CSS variable with JS
第三天的练习是用 JavaScript 和 CSS3 实现当改变CSS属性的值时，能够实时调整图片的内边距、模糊度、背景颜色，同时标题中 JS 两字的颜色也随图片背景颜色而变化的效果
##### 大致思路和解决方案
- 先在 CSS 中定义变量，并在页面样式中对页面变量进行关联
- 使用 JS 实时获取变量的值，并更新 CSS 属性的值，实时改变页面的效果

##### JS 实现部分
- 为每一个节点监听 mousemove 事件，可以在改滑块的值变化时实时的改变 CSS 全局变量
- this.dataset.sizing返回data-sizing属性的值，dataset可以返回该元素所有自定义的data-*属性，该例中data-sizing保存的是单位，因此this.dataset.sizing可返回该属性的单位
- 通过this.value获取的值不能直接设置为CSS属性，so 设置了 data-sizing 的值,可以使用 this.dataset.sizing，在该实例中,data-sizing 存放的是 px，so 在取到目标的值之后，还需要加上 px 值 对于没有单位的元素，设置为空，避免报错
- document.documentElement表示文档的根元素，对于HTML文档来说就是<html>
- 之所以又监听元素change事件，是因为color，当改变了颜色的值时，只监听mousemove事件，无法实时的改变颜色，直到鼠标从color颜色块上滑过，添加change事件就可以当改变过后实时的改变