# IamLihua.github.io
my website page

##### 博客地址

[I am LiHua](https://IamLihua.github.io)

#### 依赖：

1. node
2. hexo

运行`./setup.sh`配置依赖



#### 添加建站时间

需要自己手动添加

在主题`themes`对应的文件夹下的`footer.swig`最下面加入这样几行：

```html
<!--从这里开始到最下面都需要手工加入-->
<div><span>部分文章参考自网络，如有侵权请联系我们删除</span></div>
<div><span id="timeDate">载入天数...</span><span id="times">载入时分秒...</span></div>
<script>
    var now = new Date(); 
    function createtime() { 
        var grt= new Date("03/19/2023 16:00:00");//在此处修改你的建站时间，格式：月/日/年 时:分:秒
        now.setTime(now.getTime()+250); 
        days = (now - grt ) / 1000 / 60 / 60 / 24; dnum = Math.floor(days); 
        hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum); hnum = Math.floor(hours); 
        if(String(hnum).length ==1 ){hnum = "0" + hnum;} minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum); 
        mnum = Math.floor(minutes); if(String(mnum).length ==1 ){mnum = "0" + mnum;} 
        seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum); 
        snum = Math.round(seconds); if(String(snum).length ==1 ){snum = "0" + snum;} 
        document.getElementById("timeDate").innerHTML = "本站已安全运行 "+dnum+" 天 "; 
        document.getElementById("times").innerHTML = hnum + " 小时 " + mnum + " 分 " + snum + " 秒"; 
    } 
setInterval("createtime()",250);
</script>
```

#### 动态线条背景

在`layout/layout.swig`里面加入这几段

```html
<!--动态线条背景-->
<script type="text/javascript"
color="150,150,220" opacity='0.7' zIndex="-2" count="75" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js">
</script>
```

> 其中：
>
> - color：表示线条颜色，三个数字分别为(R,G,B)，默认：（0,0,0）
> - opacity：表示线条透明度（0~1），默认：0.5
> - count：表示线条的总数量，默认：150
> - zIndex：表示背景的z-index属性，css属性用于控制所在层的位置，默认：-1
