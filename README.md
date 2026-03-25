# **Leave-School**

##### 今日校园模拟请假软件(带核验码)

---



##  **使用须知**

##### 本软件使用[Uni-App](https://uniapp.dcloud.io/)编写

基于[Agonery133/jinrixiaoyuan](https://github.com/Agoney133/jinrixiaoyuan) 进行的二次开发

##### 软件仅供学习研究使用，请勿用于任何商业非法用途，否则造成任何后果作者概不负责！！！

请大家严格遵守学校和国家的相关规定，软件仅供学习研究使用，请勿用于商业及非法用途。





##  优化更新

2021-03-04 更新内容：

- 解决了右滑返回上一级的反人性化操作。
- 修复时间处理错误问题。
- 去除模拟签到功能。



2020-01-17 原作者版本未更新核验码功能，本次二开做出了以下优化：

- 优化使用界面，更加还原真实度。
- 增加核验码功能。



##  平台使用

支持安卓/IOS。





## 软件截图

<img src="images/2541610871202_.pic_hd-0871338.jpg" alt="2541610871202_.pic_hd" style="zoom:20%;" />

<img src="images/2551610871203_.pic_hd.jpg" alt="2551610871203_.pic_hd" style="zoom:20%;" />

<img src="images/2561610871204_.pic_hd.jpg" alt="2561610871204_.pic_hd" style="zoom:20%;" />

---

## 新增：智能体页面如何预览

你问的“怎么预览”，最直接有两种方式：

### 方式 1：HBuilderX（推荐）

1. 用 HBuilderX 打开本项目目录 `leave-school`。  
2. 顶部菜单选择 **运行 -> 运行到浏览器 -> Chrome**（或其他浏览器）。  
3. 启动后访问智能体页面路由：`/pages/agent/agent`。  
4. 在页面里填入你的 `API 地址 / API Key / 模型名` 后即可测试对话。

### 方式 2：直接打开已构建 H5 产物（快速查看）

仓库里已存在构建目录：`unpackage/dist/build/h5/`。  
如果只是看界面，可用本地静态服务指向这个目录进行预览。

示例（Python）：

```bash
cd unpackage/dist/build/h5
python3 -m http.server 8080
```

然后浏览器打开：`http://127.0.0.1:8080`，再进入对应页面路径进行查看。
