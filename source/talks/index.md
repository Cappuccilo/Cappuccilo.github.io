---
title: 说说
date: 2024-01-16 17:10:16
---
<!-- 存放说说的容器 -->
<div id="artitalk_main"></div>
<script data-swup-reload-script type='text/javascript'>
    var script = document.createElement('script');
    script.setAttribute('type', 'text/javascript');
    script.setAttribute('src', 'https://cdn.bootcdn.net/ajax/libs/artitalk/3.3.4/js/artitalk.min.js');
    document.head.appendChild(script);
    //加载成功
    script.addEventListener('load', function () {
        new Artitalk({
            appId: 'qhUyBfoGEZQN0AA2CtHOfsqc-MdYXbMMI',
            appKey: 'G8K6FdVEHULsRbe21O25GRo5',
            serverURL: 'https://api.cappuccilo.top',
            pageSize: 50, //每页评论数量
            shuoPla: '', //评论框里显示，可以不填
            motion: 1, //加载动画的开关 0（关闭），1（开启）
            atComment: 1, //评论功能的开关 0（关闭），1（开启）
            bgImg: '', //评论框里的背景，可以不填
            color1: '#ffffff', //自定义颜色，有几种方式
            color2: '#ffffff',
            color3: '#3b9a9c',
        })
    });
</script>