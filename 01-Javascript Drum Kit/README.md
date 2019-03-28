简介
第一天的练习是用JS制作一个爵士鼓的页面，通过敲击键盘上不同的字母，会发出不同的声音，并且页面上会伴随着敲击的动画

##### 大致实现思路和解决方案
检测到键盘上什么键被按下--监听keydown事件
在按键被按下的时候，播放音效--audio.play()
在按键被按下的同时，播放动画--Element.classList.add('className')
在动画结束后，移除动画，不然之后再点击不会有任何效果--Element.classList.remove('className')

页面主要HTML设置实现

- 使用 data-* 属性来嵌入自定义数据
页面里通过data-key将页面展示的内容和audio关联起来。使用方法如下介绍：
```
<ul>
<li data-animal="bird">Owl</li>
<li data-animal="fish">Salmon</li> 
<li data-animal="spider">Tarantula</li> 
</ul>
```
① data-* 属性用于存储页面或应用程序的私有自定义数据。 ② data-* 属性赋予我们在所有 HTML 元素上嵌入自定义 data 属性的能力。 ③ 属性名不应该包含任何大写字母，并且在前缀 "data-" 之后必须有至少一个字符 ④ 属性值可以是任意字符串

语法: <element data-*="somevalue">

页面主要 CSS 设置
```
.keys {
  display: flex;
  flex: 1;
  min-height: 100vh;
  align-items: center;
  justify-content: center;
}

.key {
  border: .4rem solid black;
  border-radius: .5rem;
  margin: 1rem;
  font-size: 1.5rem;
  padding: 1rem .5rem;
  transition: all .07s ease;
  width: 10rem;
  text-align: center;
  color: white;
  background: rgba(0,0,0,0.4);
  text-shadow: 0 0 .5rem black;
}

.playing {
  transform: scale(1.1);
  border-color: #ffc600;
  box-shadow: 0 0 1rem #ffc600;
}
```
主要属性有以下几个:
```
transform: scale(1.1);--该属性在键盘被点击时将该元素缩放至原来的1.1倍
.key{border: .4rem solid black;} .playing{border-color: #ffc600;}--这两条属性在按键点击的时候改变边框颜色
.key{text-shadow: 0 0 .5rem black;} .playing{box-shadow: 0 0 1rem #ffc600;}--这两条属性在按键点击的时候改变阴影的效果
transition: all .07s ease;--定义以上动画在0.07秒内完成
我们注意到我们定义了.palying类，在按键按下的时侯为该元素添加playing类，在结束后移除playing类
```

##### 按键监听&音乐播放&添加动画
关键 JS 代码
```
  function playAudio(e){
    const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    if(!audio) return;
    const key = document.querySelector(`div[data-key="${e.keyCode}"]`);
    if(!key) return;
    audio.currentTime = 0;
    audio.play();
    key.classList.add('playing');
}
window.addEventListener('keydown',playAudio);
```
- 监听页面的keydown事件，触发playAudio函数, 怎么检测按下了哪个按键？用 keyCode 来判断  (keyCode查询网站)[http://keycode.info/]
- 我们注意到audio.play();前面一行是audio.currentTime = 0;，这是因为，如果没有在播放音效前将该音乐重置，会发生以下情况，当我连续点击某一按键的时候，只有第一次点击会响，第二次第三次连续的点击可能没声音。所以在每一次点击之前重置音效是很有必要的
- key.classList.add('playing');可以在按键点击的同时为该元素添加playing类，展示小动画
- if(!audio) return; if(!key) return;因为并不是每一个按键都有音效，当用户点击了非绑定音效按键，及时退出函数是很好的习惯

##### 动画结束后移除动画
监听每一个按键元素的transitionend事件，当按键元素的动画结束后会触发stopTransition函数