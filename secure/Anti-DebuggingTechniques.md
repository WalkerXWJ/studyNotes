# Anti-debugging techniques 反调试技术

## javascript 反调试

### 1、重定向到空白页面

```javascript
/**
 * 使用vue实现的项目可以使用此方法
 * 策略：通过检测开启开发者，重定向到about:blank
 * 有效情况：通过中间人替换删除init代码或配置拦截对应console-ban的脚本文件加载请求，即可绕过反调试限制
 * 使用步骤：创建console-ban.man.js，并在网页中引入
 */
//第一步：console-ban.man.js文件内容
/*!
 * console-ban v4.1.0
 * (c) 2020-2022 fz6m
 * Released under the MIT License.
 */
!function (e, t) {
    "object" == typeof exports && "undefined" != typeof module ? t(exports) : "function" == typeof define && define.amd ?
        define(["exports"], t) : t((e = "undefined" != typeof globalThis ? globalThis : e || self).ConsoleBan = {})
}(this, (function (e) {
    "use strict";
    var t = function () {
            return t = Object.assign || function (e) {
                for (var t, i = 1, n = arguments.length; i < n; i++)
                    for (var o in t = arguments[i]) Object.prototype.hasOwnProperty.call(t, o) && (e[o] = t[
                        o]);
                return e
            }, t.apply(this, arguments)
        },
        i = {
            clear: !0,
            debug: !0,
            debugTime: 3e3
        },
        n = 2,
        o = function (e) {
            return ~navigator.userAgent.toLowerCase().indexOf(e)
        },
        r = function (e, t) {
            t !== n ? location.href = e : location.replace(e)
        },
        c = 0,
        a = 0,
        f = function (e) {
            var t = 0,
                i = 1 << c++;
            return function () {
                (!a || a & i) && 2 === ++t && (a |= i, e(), t = 1)
            }
        },
        l = function (e) {
            var t = /./;
            t.toString = f(e);
            var i = function () {
                return t
            };
            i.toString = f(e);
            var n = new Date;
            n.toString = f(e), console.log("%c", i, i(), n);
            var o, r, c = f(e);
            o = c, r = new Error, Object.defineProperty(r, "message", {
                get: function () {
                    o()
                }
            }), console.log(r)
        },
        u = function () {
            function e(e) {
                var n = t(t({}, i), e),
                    o = n.clear,
                    r = n.debug,
                    c = n.debugTime,
                    a = n.callback,
                    f = n.redirect,
                    l = n.write;
                this._debug = r, this._debugTime = c, this._clear = o, this._callback = a, this._redirect = f,
                    this._write = l
            }
            return e.prototype.clear = function () {
                this._clear && (console.clear = function () {})
            }, e.prototype.debug = function () {
                if (this._debug) {
                    var e = new Function("debugger");
                    setInterval(e, this._debugTime)
                }
            }, e.prototype.redirect = function (e) {
                var t = this._redirect;
                if (t)
                    if (0 !== t.indexOf("http")) {
                        var i, n = location.pathname + location.search;
                        if (((i = t) ? "/" !== i[0] ? "/".concat(i) : i : "/") !== n) r(t, e)
                    } else location.href !== t && r(t, e)
            }, e.prototype.callback = function () {
                if ((this._callback || this._redirect || this._write) && window) {
                    var e, t = this.fire.bind(this),
                        i = window.chrome || o("chrome"),
                        r = o("firefox");
                    if (!i) return r ? ((e = /./).toString = t, void console.log(e)) : void
                    function (e) {
                        var t = new Image;
                        Object.defineProperty(t, "id", {
                            get: function () {
                                e(n)
                            }
                        }), console.log(t)
                    }(t);
                    l(t)
                }
            }, e.prototype.write = function () {
                var e = this._write;
                e && (document.body.innerHTML = "string" == typeof e ? e : e.innerHTML)
            }, e.prototype.fire = function (e) {
                this._callback ? this._callback.call(null) : (this.redirect(e), this._redirect || this.write())
            }, e.prototype.ban = function () {
                this.callback(), this.clear(), this.debug()
            }, e
        }();
    e.init = function (e) {
        new u(e).ban()
    }, Object.defineProperty(e, "__esModule", {
        value: !0
    })
}));
// 步骤2:网页引入console-ban.man.js，并添加如下代码
<script>
    ConsoleBan.init({
        redirect: "about:blank",
    });
</script>
```


