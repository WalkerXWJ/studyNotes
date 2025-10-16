# 项目地址
```http
https://github.com/loks666/get_jobs.git
```
## 环境准备
```markdown
1. 安装java jdk21
2. 安装idea工具 visual studio code
   clone项目到idea工具中
3. 复制 src\main\resources\.env_template 为 src\main\resources\.env 并添加ai和推送信息
   - `HOOK_URL`：企业微信机器人推送的链接
   - `BASE_URL`：直连或中转链接地址
   - `API_KEY`：调用的API KEY
   - `MODEL`：需要使用的模型名称
4. 修改 src\main\resources\config.yaml 文件，修改自己各平台的配置信息
5. 打开 src\main\java\boss\Boss.java 执行会下载浏览器和驱动信息，在打开的浏览器中登陆后就可以进行简历投递了
6. cookie文件保存在 src\main\java\boss\cookie.json，当文件不存在会自动创建空文件
```
cookie文件的内容示例，登陆后程序会自动保存cookie信息
```json
[
    {
        "path": "/",
        "expires": 1.791973924318293E9,
        "domain": ".zhipin.com",
        "name": "lastCity",
        "httpOnly": false,
        "secure": false,
        "value": "101010100"
    },
    {
        "path": "/",
        "expires": 1.794997921169771E9,
        "domain": ".hm.baidu.com",
        "name": "HMACCOUNT_BFESS",
        "httpOnly": false,
        "secure": true,
        "value": "5D3816FE9ABEDDE5"
    },
    {
        "path": "/",
        "expires": -1,
        "domain": ".zhipin.com",
        "name": "__g",
        "httpOnly": false,
        "secure": false,
        "value": "-"
    },
    {
        "path": "/",
        "expires": 1.79197394E9,
        "domain": ".zhipin.com",
        "name": "Hm_lvt_194df3105ad7148dcf2b98a91b5e727a",
        "httpOnly": false,
        "secure": false,
        "value": "1760437922"
    },
    {
        "path": "/",
        "expires": -1,
        "domain": ".zhipin.com",
        "name": "HMACCOUNT",
        "httpOnly": false,
        "secure": false,
        "value": "5D3816FE9ABEDDE5"
    },
    {
        "path": "/",
        "expires": -1,
        "domain": ".zhipin.com",
        "name": "__l",
        "httpOnly": false,
        "secure": false,
        "value": "l=%2Fwww.zhipin.com%2Fweb%2Fuser%2F%3Fka%3Dheader-login&s=3&friend_source=0"
    },
    {
        "path": "/",
        "expires": 1.761418800363715E9,
        "domain": ".zhipin.com",
        "name": "wt2",
        "httpOnly": true,
        "secure": false,
        "value": "DUIv2b9LkNPe45Sw6tlmJJLd68kSZDv0bU_dtgctxuf74UehekL5tPJY1Azl52JTRwYHHK_uR4c7KJfgt0EEvIg~~"
    },
    {
        "path": "/",
        "expires": 1.761418800364383E9,
        "domain": ".zhipin.com",
        "name": "wbg",
        "httpOnly": true,
        "secure": false,
        "value": "0"
    },
    {
        "path": "/",
        "expires": 1.761418800364469E9,
        "domain": ".zhipin.com",
        "name": "zp_at",
        "httpOnly": true,
        "secure": false,
        "value": "c-yLZMsqSKRgx-PY9ohG6kyiif_nDCAy_ZT14uDVFFk~"
    },
    {
        "path": "/",
        "expires": 1.794997939048467E9,
        "domain": "www.zhipin.com",
        "name": "ab_guid",
        "httpOnly": false,
        "secure": false,
        "value": "742a0163-a1c5-40dd-9e9b-c625c44d0b95"
    },
    {
        "path": "/",
        "expires": -1,
        "domain": ".zhipin.com",
        "name": "Hm_lpvt_194df3105ad7148dcf2b98a91b5e727a",
        "httpOnly": false,
        "secure": false,
        "value": "1760437940"
    },
    {
        "path": "/",
        "expires": 1.761418800084607E9,
        "domain": ".zhipin.com",
        "name": "bst",
        "httpOnly": false,
        "secure": true,
        "value": "V2Rd8gE-L73lxgXdJvzhkZISOw7DjTxA~~|Rd8gE-L73lxgXdJvzhkZISOw7DvRxA~~"
    },
    {
        "path": "/",
        "expires": -1,
        "domain": ".zhipin.com",
        "name": "__c",
        "httpOnly": false,
        "secure": false,
        "value": "1760437922"
    },
    {
        "path": "/",
        "expires": 1.794997942319903E9,
        "domain": ".zhipin.com",
        "name": "__a",
        "httpOnly": false,
        "secure": false,
        "value": "83749026.1760437922..1760437922.5.1.5.5"
    },
    {
        "path": "/",
        "expires": -1,
        "domain": "ws.zhipin.com",
        "name": "SERVERID",
        "httpOnly": false,
        "secure": false,
        "value": "25d965ce4e8d41713e5cb14924ddc044|1760437943|1760437943"
    },
    {
        "path": "/",
        "expires": 1.760668343E9,
        "domain": ".zhipin.com",
        "name": "__zp_stoken__",
        "httpOnly": false,
        "secure": false,
        "value": "6b6cfOzjDncK3wq7CukMlChMTEAg%2BKzY4LGg%2BOyxEOj87ODY8PTs4Pho7K8OZwrXCkCHDiGHDi0I2KDg2Pzs7QjY6QRg4QsK4NjwuTMK5wpQgw4tjw4c7NlZXC8OBwrslMwvDrMK2PSjCu8K7NjlDNxPCusK5w4QPwrfCtsOCwqPCvMKpw4A5O0E9MDUFWhNZNTtGTFkLSWJLY2FNBVRNUik3Ozw4csK7MDkHBQkFDA4QFBARDw0Rend8enZ6dwkLBwsGMDbCnMK7wp%2FDncKrxJnDqcScwprCqMKaw4LDt8Kpwr%2FCmsKkRcO7wq%2FCiMKZwpHCpcKvwrpRwrBNwrfCsX%2FCocK8UmLCpnTCq2RIw4FkbsOAT8K8wrVPXXdvEGIPWxM6ETjCjsOJ"
    }
]

```