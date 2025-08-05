[官网文档](https://learn.microsoft.com/zh-cn/microsoft-edge/extensions/landing/)
概念：
- Microsoft Edge 扩展也称为 加载项，是开发人员用来添加或修改 Microsoft Edge 的功能以改进用户的浏览体验的小型应用。
- 扩展应至少包含以下功能:
	- 包含基本平台信息的应用程序清单 JSON 文件。
	- 定义该函数的 JavaScript 文件。
	- 定义用户界面的 HTML 和 CSS 文件。
- Microsoft合作伙伴中心仅接受新的清单 V3 Microsoft Edge 扩展提交。[讨论](https://github.com/microsoft/MicrosoftEdge-Extensions/discussions/27)
- V3 指 [Manifest V3](https://developer.chrome.com/docs/extensions/develop/migrate/checklist?hl=zh-cn)
## 入门
### 浏览器工作原理
- 浏览器的tab是独立线程
	- 每个浏览器选项卡都与其他每个选项卡隔离。每个选项卡在与其他浏览器选项卡和线程隔离的单独线程中运行。
- 每个选项卡处理一个 GET 请求
	- 每个选项卡处理一个 GET 请求。 每个选项卡使用 URL 获取单个数据流，该数据流通常是 HTML 文档。 该单个流或页面包括 JavaScript 等指令，包括标记、图像引用、CSS 引用等。 所有资源都下载到该选项卡页，然后在选项卡中呈现该页。
- 每个选项卡和远程服务器之间发生通信
	- 每个选项卡和远程服务器之间发生通信。 每个选项卡在独立环境中运行。 每个选项卡仍连接到 Internet，但每个选项卡都与其他选项卡隔离。 选项卡可以运行 JavaScript 来与服务器通信。 服务器是输入到选项卡 URL 栏中的第一个 GET 请求的发起服务器。
- 通信模型
	- 扩展模型使用不同的通信模型。 与选项卡页类似，扩展在独立于其他选项卡页线程的单个线程中运行。 选项卡将单个 GET 请求发送到远程服务器，然后呈现页面。 但是，扩展的工作方式类似于远程服务器。 在浏览器中安装扩展会在浏览器中创建独立的 Web 服务器。 该扩展与所有选项卡页隔离。
### 扩展体系结构
- 扩展 Web 服务器捆绑包
	- 扩展是 Web 资源的捆绑包。 Web 资源类似于 (Web 开发人员) 发布到 Web 服务器的其他资源。 生成扩展时，可将 Web 资源捆绑到 zip 文件中。
	- zip 文件包括 HTML、CSS、JavaScript 和图像文件。 zip 文件的根目录中还需要一个文件。 另一个文件是名为 的 `manifest.json`清单文件。 清单文件是扩展的蓝图，包括扩展的版本、标题、扩展运行所需的权限等。
- 启动扩展服务器
	- Web 服务器包含 Web 捆绑包。 浏览器导航到服务器上的 URL，并下载文件以在浏览器中呈现。 浏览器使用证书、配置文件等进行导航。 如果指定了文件 `index.html` ，该文件将存储在 Web 服务器上的特殊位置。
	- 使用扩展时，浏览器的选项卡页将使用扩展运行时转到扩展的 Web 捆绑包。 扩展运行时为 URL `extension://{some-long-unique-identifier}/index.html`中的文件提供服务，其中 `{some-long-unique-identifier}` 是安装期间分配给扩展的唯一标识符。 每个扩展使用不同的唯一标识符。 每个标识符都指向浏览器中安装的 Web 捆绑包。
- 与选项卡和浏览器工具栏通信
	- 扩展可与选项卡和浏览器工具栏通信。 扩展可以与浏览器的工具栏交互。 每个扩展在单独的线程中管理正在运行的选项卡页，并且每个选项卡页上的 DOM是隔离的。 扩展使用扩展 API 在扩展页和选项卡页之间进行通信。 扩展 API 提供额外的功能，包括通知管理、存储管理等。
	- 与 Web 服务器一样，扩展在浏览器打开时等待通知。 扩展页和选项卡页在彼此隔离的线程中运行。 若要允许扩展与任何选项卡页一起使用，请使用扩展 API 并在清单文件中设置权限。
- 安装时选择加入权限
	- 扩展在安装时提供选择加入权限。 在 文件中指定扩展权限 `manifest.json` 。 当用户安装扩展时，将显示有关该扩展所需的权限的信息。 根据所需的权限类型，扩展可以从浏览器提取和使用信息。
### 官方示例项目-图片查看器弹出网页
拉取项目：
```bash
git clone https://github.com/microsoft/MicrosoftEdge-Extensions.git
```
加载示例扩展：管理扩展中打开开发者，选择加载解压文件，选择目录`picture-viewer-popup-webpage`，即可完成。
#### 项目文件清单
目录中的 `/picture-viewer-popup-webpage/` 目录和文件：
```d
/icons/
   extension-icon16x16.png
   extension-icon32x32.png
   extension-icon48x48.png
   extension-icon128x128.png
/images/
   stars.jpeg
/popup/
   popup.html
manifest.json
```
- 目录 `/icons/` 包含用于表示浏览器地址栏附近的扩展名的文件版本 `.png` 。
- 目录 `/images/` 包含 `stars.jpeg`，显示在扩展的弹出窗口中。
- 目录 `/popup/` 包含 `popup.html`，用于定义扩展弹出窗口中显示的网页内容。
- `manifest.json` 包含有关扩展的基本信息。
#### 清单文件 (`manifest.json`)
每个扩展包必须在根目录中有一个 `manifest.json` 文件。 清单提供了扩展、扩展包版本以及扩展名称和说明的详细信息。
`manifest.json` 包含以下行：
```json
{
  "name": "Picture viewer pop-up webpage",
  "version": "0.0.0.1",
  "manifest_version": 3,
  "description": "A browser extension that displays an image in a pop-up webpage.",
  "icons": {
      "16": "icons/extension-icon16x16.png",
      "32": "icons/extension-icon32x32.png",
      "48": "icons/extension-icon48x48.png",
      "128": "icons/extension-icon128x128.png"
  },
  "action": {
      "default_popup": "popup/popup.html"
  }
}
```
#### 用于启动扩展的图标
- 目录 `/icons/` 包含图标图像文件。 这些图标用作单击以启动扩展的按钮的背景图像。
- 当扩展正在运行时，其中一个图标显示在工具栏上的地址栏旁边。
- 若要关闭扩展，请单击工具栏上的扩展图标，或单击**扩展** (![扩展”图标](https://learn.microsoft.com/zh-cn/microsoft-edge/extensions/getting-started/picture-viewer-popup-webpage-images/extensions-icon.png)) 按钮。
- 图标建议：
	- 使用`PNG`格式，但也可以使用 `BMP`、 `GIF` 、`ICO` 或 `JPEG` 格式。
	- 如果提供单个图标文件，请使用 128 x 128 像素，如有必要，浏览器可以调整其大小。
#### 弹出对话框 (`popup.html`)
单击图标以启动扩展时，`/popup/popup.html`被加载执行，显示为模式对话框。
`popup.html` 包含以下代码，用于显示标题和星形图像：
```html
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <title>Picture viewer pop-up webpage</title>
    </head>
    <body>
        <div>
            <img src="/images/stars.jpeg" alt="Stars" />
        </div>
    </body>
</html>
```
弹出网页在manifest.json中需要声明，声明如下：
```json
{
    "action": {
        "default_popup": "popup/popup.html"
    }
}
```
### 官方示例项目-使用内容脚本的图片插入器
