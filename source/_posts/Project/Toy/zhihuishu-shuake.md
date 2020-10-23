---
title: 智慧树刷课脚本[已失效]
date: 2019-10-09 19:54:17
updated:
tags:
   - 智慧树
categories:
   - 让生活变得更美好 
---



只切换流畅和1.5倍加速，不使用蓝色标记是为了避免‘有可能’的反制措施。
静音还是使用自己启动阅览器自带的静音功能比较好。

```js
var ti = $("body"); 
var video = $(".catalogue_ul1 li[id*=video-]"); 
var i = 1; 
var v = 1;
<!-- You choice -->
<!-- var howManyYouWant = video.length; -->
<!-- var howManyYouWant = 10; -->
var howManyYouWant = video.length;

<!-- video.css("color", "blue"); --> 
console.log("已选取" + video.length + "个小节"); 
setTimeout(function () 
{ 
    $('.speedTab15').click(); 
    $('.line1bq').click(); 
    console.log("已切换至流畅和1.5倍加速"); 
}, 3000); 

ti.on("DOMNodeInserted", 
    function (e) { 
        if (e.target.textContent == "关闭") { 
            console.log("检测到第" + i + "个弹题窗口"); 
            window.setTimeout(function () { 
                document.getElementById("tmDialog_iframe")
                    .contentWindow.document
                    .getElementsByClassName("answerOption")[0]
                    .getElementsByTagName("input")[0]
                    .click(); 
                $(".popbtn_cancel").click(); 
                console.log("已关闭"); 
            }, 3000); 
            i++; 
        } 
        else if (e.target.textContent == "本节视频,累计观看时间『100%』") 
        { 
            if (v > howManyYouWant){
                
                <!-- nothing -->
            }else{
                console.log("检测到视频观看完成，准备跳到下一节"); 
                $('.next_lesson_bg').find('a').trigger('click'); 
                console.log("已跳转"); 
                setTimeout(function () { 
                    $('.speedTab15').click();
                    $('.line1bq').click(); 
                    console.log("已切换至流畅和1.5倍加速"); 
                    }
                    , 6000); 
                v++; 
                console.log("目前播放了" + v + "个视频"); 
            }
        } 
    }
);
```


