CSS animation 属性是 `animation-name`，`animation-duration` 持续时间, `animation-timing-function` ，`animation-delay`，`animation-iteration-count` 迭代次数，`animation-direction` 方向，`animation-fill-mode` 填充模式 和 `animation-play-state` 播放状态 属性的一个简写属性形式。


```css
/* @keyframes duration | timing-function | delay |
   iteration-count | direction | fill-mode | play-state | name */
animation: 3s ease-in 1s 2 reverse both paused slidein;
/* @keyframes duration | timing-function | delay | name */
animation: 3s linear 1s slidein;
/* @keyframes duration | name */
animation: 3s slidein;
```

```css
Given the following animation:
@keyframes slidein {
  from { transform: scaleX(0); }
  to   { transform: scaleX(1); }
}
```

哪些属性是可动画的？值得注意的是，此描述也适用于CSS变换。

- 初始值  
as each of the properties of the shorthand:  
作为速记的每个属性：
  + animation-name: none
  + animation-duration: 0s
  + animation-timing-function: ease
  + animation-delay: 0s
  + animation-iteration-count: 1
  + animation-direction: normal
  + animation-fill-mode: none
  + animation-play-state: running
  
- 适用元素	
  + all elements, `::before` and `::after` pseudo-elements(伪元素)
- 是否是继承属性	**否**  
- 计算值	
  as each of the properties of the shorthand:
  + animation-name: as specified
  + animation-duration: as specified
  + animation-timing-function: as specified
  + animation-delay: as specified
  + animation-direction: as specified
  + animation-iteration-count: as specified
  + animation-fill-mode: as specified
  + animation-play-state: as specified
- Animation type	
  - discrete
## 语法
animation 属性用来指定一组或多组动画，每组之间用逗号相隔。

每组动画规定的属性如下：

- 以下属性出现0次或1次：
  + `<single-transition-timing-function>`
  + `<single-animation-iteration-count>`
  + `<single-animation-direction>`
  + `<single-animation-fill-mode>`
  + `<single-animation-play-state>`
- animation 的 name 值可能是：`none`，`<custom-ident>`， `<string>`
- `<time>` 可能会出现0、1 或 2 次  
  
每个动画定义中的属性值的顺序很重要：可以被解析为 `<time>` 的第一个值被分配给animation-duration， 第二个分配给 animation-delay。

每个动画定义中的值的顺序，对于区分 animation-name 值与其他关键字也很重要。解析时，对于除 animation-name 之外的有效的关键字，必须被前面的简写中没有找到值的属性所接受。此外，在序列化时，animation-name 与以及其他属性值做区分等情况下，必须输出其他属性的默认值。

Values
<single-animation-iteration-count>
动画播放的次数。该值必须是animation-iteration-count可用的值之一。
<single-animation-direction>
动画播放的方向。该值必须是animation-direction可用的值之一。
<single-animation-fill-mode>
确定动画在执行之前和之后这两个阶段应用的样式。该值必须是animation-fill-mode可用的值之一。
<single-animation-play-state>
确定动画是否正在播放。该值必须是animation-play-state中可用的值之一。
语法
<single-animation>#
where 
<single-animation> = <time> || <timing-function> || <time> || <single-animation-iteration-count> || <single-animation-direction> || <single-animation-fill-mode> || <single-animation-play-state> || [ none | <keyframes-name> ]

where 
<timing-function> = linear | <cubic-bezier-timing-function> | <step-timing-function>
<single-animation-iteration-count> = infinite | <number>
<single-animation-direction> = normal | reverse | alternate | alternate-reverse
<single-animation-fill-mode> = none | forwards | backwards | both
<single-animation-play-state> = running | paused
<keyframes-name> = <custom-ident> | <string>

where 
<cubic-bezier-timing-function> = ease | ease-in | ease-out | ease-in-out | cubic-bezier(<number <a href="/zh-CN/docs/CSS/Value_definition_syntax#Brackets" title="Brackets: enclose several entities, combinators, and multipliers to transform them as a single component">[0,1]>, <number>, <number <a href="/zh-CN/docs/CSS/Value_definition_syntax#Brackets" title="Brackets: enclose several entities, combinators, and multipliers to transform them as a single component">[0,1]>, <number>)
<step-timing-function> = step-start | step-end | steps(<integer>[, <step-position>]?)

where 
<step-position> = jump-start | jump-end | jump-none | jump-both | start | end


范例
赛隆人之眼(赛隆人是一个虚构的生化人种族,出自科幻电视系列剧星际大争霸系列)
<div class="view_port">
  <div class="polling_message">
    Listening for dispatches
  </div>
  <div class="cylon_eye"></div>
</div>
.polling_message {
  color: white;
  float: left;
  margin-right: 2%;
}

.view_port {
  background-color: black;
  height: 25px;
  width: 100%;
  overflow: hidden;
}

.cylon_eye {
  background-color: red;
  background-image: linear-gradient(to right,
      rgba(0, 0, 0, .9) 25%,
      rgba(0, 0, 0, .1) 50%,
      rgba(0, 0, 0, .9) 75%);
  color: white;
  height: 100%;
  width: 20%;

  -webkit-animation: 4s linear 0s infinite alternate move_eye;
          animation: 4s linear 0s infinite alternate move_eye;
}

@-webkit-keyframes move_eye { from { margin-left: -20%; } to { margin-left: 100%; }  }
        @keyframes move_eye { from { margin-left: -20%; } to { margin-left: 100%; }  }


更多示例请参阅使用CSS动画。

潜在的问题
眨眼和闪烁的动画对于有认知问题的人来说是有问题的，比如注意力缺陷多动障碍(ADHD)。此外，某些动画效果可以触发前庭神经紊乱、癫痫、偏头痛和暗点敏感性。

考虑提供一种暂停或禁用动画的机制，以及使用 Reduced Motion Media Query（简约运动媒体查询），为那些表示不喜欢动画的用户创建一个良好的体验。

Designing Safer Web Animation For Motion Sensitivity · An A List Apart Article 
An Introduction to the Reduced Motion Media Query | CSS-Tricks
Responsive Design for Motion | WebKit
MDN Understanding WCAG, Guideline 2.2 explanations
Understanding Success Criterion 2.2.2  | W3C Understanding WCAG 2.0
规范
Specification	Status	Comment
CSS Animations
animation	Working Draft	Initial definition
浏览器兼容性
Report problems with this data on GitHub
desktop	mobile
Chrome	Edge	Firefox	Internet Explorer	Opera	Safari	WebView Android	Chrome Android	Firefox Android	Opera Android	iOS Safari	Samsung Internet
animation
Full support43Open	Full support12Open	Full support16Open	Full support10	Full support30Open	Full support9Open	Full support43Open	Full support43Open	Full support16Open	Full support30Open	Full support9Open	Full support4.0Open
Legend
Quantum CSS notes
Gecko有一个bug，当你在屏幕上对屏幕外的元素使用带有指定延时的动画时，Gecko不会在某些平台上重绘，例如Windows bug 1383239。这个问题已经在Firefox新的并行CSS引擎(也称为Quantum CSS 或者 Stylo，计划在Firefox 57中发布)中得到了解决。
另外一个Gecko bug，当我们激活<details>元素的动画效果时，即使通过 open 属性也不能将其展示(bug 1382124)。Quantum CSS会将其修复了。
另一个bug是，由于动画使用的是em单位，所以即使我们改变父元素的font-size属性也不会影响该动画元素(bug 1254424)，而它们原本应该受到影响。Quantum CSS会将其修复了。
更多
Using CSS animations
JavaScript AnimationEvent API