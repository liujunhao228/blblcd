# blblcd

基于 [bilibili-API-collect](https://github.com/SocialSisterYi/bilibili-API-collect) 开发的哔哩哔哩视频评论下载工具。

## ✨ 主要特点

* 🚀 **使用便捷**：单文件可执行程序，无需安装其他依赖
* 📥 **灵活下载**：
  * 支持单个/多个视频评论下载
  * 可按热评/时间顺序排序
  * 支持下载指定UP主视频评论
  * 可按投稿时间/收藏/播放量排序
* 🔍 **评论全面**：
  * 支持下载子评论和隐藏评论
  * 支持下载"楼中楼"评论
  * 支持下载评论中的图片
* 📊 **数据分析**：
  * 支持评论统计并输出地图可视化
* 💻 **跨平台支持**：
  * Windows
  * MacOS
  * Linux

## ⚠️ 使用须知

* 这是一个命令行工具，没有图形界面
* 子评论会自动爬取，控制台会显示相应页码
* 如果只获取到少量评论（10-20条）而实际应该有更多，很可能是 cookie 已失效
* CSV 文件使用 UTF-8 编码保存，建议使用文本编辑器或代码编辑器打开（不要用 Microsoft Office）
* 为避免触发反爬，程序限制了请求频率，所以爬取速度较慢
* 目前以维护现有功能为主，暂不接受新功能开发请求
* 欢迎通过 issues 提交 bug 报告

## 📝 评论数据字段

```
Uname          - 用户名称
Sex            - 性别
Content        - 评论内容
Rpid           - 评论ID
Oid            - 评论区ID
Bvid           - 视频BV号
Mid            - 发送者ID
Parent         - 父级评论
Fansgrade      - 粉丝标签
Ctime          - 评论时间戳
Like           - 点赞数
Following      - 关注状态
Current_level  - 当前等级
Location       - 位置信息
```

## 📖 使用指南

### 准备工作

* **Cookie**：必需
* **MID**：UP主ID（搜索UP主视频时必需）
* **BVID**：视频ID（下载单个/多个视频评论时必需）

### 获取必要信息

#### 1. 获取 Cookie
1. 登录[哔哩哔哩](https://www.bilibili.com/)
2. 按 `F12` 打开开发者工具
3. 切换到"网络"标签
4. 点击任意请求（最好是 XHR 或 Fetch 类型）
5. 找到并复制 cookie 值
6. 保存为 `cookie.text` 文件（建议放在与 blblcd 相同目录下）


#### 2. 获取 UP主 MID
在UP主个人空间URL中可以找到MID。例如：`https://space.bilibili.com/112233445` 中的 `112233445` 就是MID。


#### 3. 获取视频 BVID
在视频页面URL中可以找到BVID。例如：`https://www.bilibili.com/video/BV1Cm421T7Zg` 中的 `BV1Cm421T7Zg` 就是BVID。


### 命令示例

查看所有命令：
```bash
./blblcd -h
```


#### 下载视频评论

基础用法：
```bash
blblcd video BV1VJ4m1jk34K
```

多个视频：
```bash
blblcd video BV1VJ4m1jk34K BV1sdfVJ4m1jksdf
```

按回复排序：
```bash
blblcd video BV1VJ4m1jk34K -corder 2
```

指定cookie文件：
```bash
blblcd video BV1VJ4m1jk34K -cookie /path/to/cookiefile.text -corder 2
```

自定义输出路径：
```bash
blblcd video BV1VJ4m1jk34K -corder 2 -output path/to/output
```

#### 下载UP主视频评论

基础用法（默认获取前三页，每页30条视频）：
```bash
blblcd up 123344555
```

指定cookie：
```bash
blblcd up 123344555 -cookie /path/to/cookiefile.text
```

按收藏排序：
```bash
blblcd up 123344555 -skip 3 -pages 5 -vorder stow
```

固定页数：
```bash
blblcd up 123344555 -pages 5
```

跳过指定页数：
```bash
blblcd up 123344555 -skip 3 -pages 5
```

自定义输出路径：
```bash
blblcd up 123344555 -output output/path
```

设置并发数：
```bash
blblcd up 123344555 -workers 10
```

#### 生成地图模板

将 `geo-template.geojson` 放在程序目录下（release包中已包含）：
```bash
blblcd video BV1VJ4m1jk34K --mapping
```

## 📜 声明

* 源代码仅供学习交流使用
* 请遵守哔哩哔哩相关规定
* blblcd 不会保存或泄露 cookie
