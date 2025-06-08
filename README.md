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
* 基于原作者的基础上修改，主要是为了自己用，因此未经过完整测试
* 相较于原版，移除数据分析（地图相关）功能，增强自定义输出路径
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

### 输出目录说明

#### 基础输出目录
- 使用 `-output` 或 `-o` 参数指定基础输出目录
- 默认值为 `./output`
- 如果不指定其他输出参数，所有文件都会保存在此目录下

#### 评论输出目录
- 使用 `--comment-output` 参数指定评论内容保存路径
- 如果不指定，默认保存在基础输出目录下的视频BV号文件夹中
- 例如：`./output/BV1xx/comments.csv`

#### 图片输出目录
- 使用 `--image-output` 参数指定评论图片保存路径
- 如果不指定，默认保存在评论内容所在目录的 `images` 文件夹中
- 例如：`./output/BV1xx/images/`

#### 目录结构示例

1. **默认情况**（不指定任何输出参数）：

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
# 指定基础输出目录
blblcd video BV1VJ4m1jk34K -output /path/to/output

# 指定评论输出目录
blblcd video BV1VJ4m1jk34K --comment-output /path/to/comments

# 指定图片输出目录
blblcd video BV1VJ4m1jk34K --image-output /path/to/images

# 同时指定所有输出目录
blblcd video BV1VJ4m1jk34K -output /path/to/output --comment-output /path/to/comments --image-output /path/to/images
```

下载评论中的图片：
```bash
blblcd video BV1VJ4m1jk34K -img-download
```

设置最大重试次数：
```bash
blblcd video BV1VJ4m1jk34K -max-try-count 5
```

完整参数示例：
```bash
blblcd video BV1VJ4m1jk34K \
  -cookie /path/to/cookie.text \
  -output /path/to/output \
  --comment-output /path/to/comments \
  --image-output /path/to/images \
  -img-download \
  -workers 5 \
  -max-try-count 3 \
  -corder 1
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
# 指定基础输出目录
blblcd up 123344555 -output /path/to/output

# 指定评论输出目录
blblcd up 123344555 --comment-output /path/to/comments

# 指定图片输出目录
blblcd up 123344555 --image-output /path/to/images

# 同时指定所有输出目录
blblcd up 123344555 -output /path/to/output --comment-output /path/to/comments --image-output /path/to/images
```

设置并发数：
```bash
blblcd up 123344555 -workers 10
```

下载评论中的图片：
```bash
blblcd up 123344555 -img-download
```

完整参数示例：
```bash
blblcd up 123344555 \
  -cookie /path/to/cookie.text \
  -output /path/to/output \
  --comment-output /path/to/comments \
  --image-output /path/to/images \
  -img-download \
  -workers 10 \
  -pages 5 \
  -skip 0 \
  -vorder pubdate \
  -max-try-count 3
```

#### 常用组合示例

1. **下载热门视频评论并按点赞排序**：
```bash
blblcd video BV1VJ4m1jk34K -corder 1 -img-download
```

2. **下载UP主最新发布的视频评论**：
```bash
blblcd up 123344555 -vorder pubdate -pages 5
```

3. **下载UP主最多播放的视频评论**：
```bash
blblcd up 123344555 -vorder click -pages 5
```

4. **下载UP主最多收藏的视频评论**：
```bash
blblcd up 123344555 -vorder stow -pages 5
```

5. **批量下载多个视频评论并保存到指定目录**：
```bash
blblcd video BV1xx BV2yy BV3zz -output /custom/output -img-download
```

6. **下载UP主视频评论并设置高并发**：
```bash
blblcd up 123344555 -workers 10 -pages 5 -img-download
```

7. **下载视频评论并自定义所有输出路径**：
```bash
blblcd video BV1VJ4m1jk34K \
  -output /custom/output \
  --comment-output /custom/comments \
  --image-output /custom/images \
  -img-download
```

8. **下载UP主视频评论并跳过前3页**：
```bash
blblcd up 123344555 -skip 3 -pages 5 -vorder pubdate
```

## 📜 声明

* 源代码仅供学习交流使用
* 请遵守哔哩哔哩相关规定
* blblcd 不会保存或泄露 cookie
