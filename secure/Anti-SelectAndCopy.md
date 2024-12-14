# Anti-SelectAndCopy 反选择和复制

# web

## 1、CSS 实现禁用选择和复制

```css
/* 使用方法：在网页head中添加样式表代码即可
/* 策略：禁用弹出菜单和文选择：*/
/* 有效情况：打开开发者工具，选择element*/
<style>
        #content_views pre{
            -webkit-touch-callout: none;
            -webkit-user-select: none;
            -khtml-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none; 
            user-select: none; 
        }
        #content_views pre code{
            -webkit-touch-callout: none;
            -webkit-user-select: none;
            -khtml-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none; 
            user-select: none; 
        }
</style>
```
