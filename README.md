# 抖音直播监控工具

一款基于 Python + Tkinter 的抖音直播实时监控桌面应用，支持视频画面播放、弹幕实时展示、提词器、数据导出等功能。

---

## 界面预览

```
┌──────────────────┬──────────────────────────────────────┐
│                  │  直播地址输入栏                         │
│                  │  [URL 输入框] [▶Start] [⏹Stop] [📝]   │
│                  │  [📋 Stream URL 推流地址]  [Copy]       │
│   左侧视频区      ├──────────────────────────────────────┤
│   525 × 935      │  评论栏（550px）                        │
│                  │  💬 进播 关注 点赞 礼物（过滤复选框）     │
│  [FFplay/OpenCV  │  弹幕实时滚动显示                        │
│   /FFmpeg]       ├──────────────────────────────────────┤
│  🔊 音量控制      │  提词器栏（自适应）                      │
│  码率/分辨率/帧率 │  [B][📌][👻][🎨][A-][A+] [📤 Send]    │
│                  │  提词内容输入框                          │
│                  ├──────────────────────────────────────┤
│                  │  状态栏                                 │
└──────────────────┴──────────────────────────────────────┘
```

---

## 功能特性

### 🎥 视频播放
- 支持三种播放模式：**FFplay**（推荐）、**OpenCV**、**FFmpeg**
- FFplay 模式：无窗口音频播放 + OpenCV 视频显示，音画同步最佳
- 视频区固定尺寸 525×935（竖屏直播适配）
- 右下角实时叠加显示直播间名称
- 码率 / 分辨率 / 帧率实时显示

### 🔊 音量控制
- 音量加减按钮（步长 10%）
- 一键静音 / 恢复

### 💬 弹幕评论区
- 实时显示所有弹幕类型：
  - 💬 用户评论（白色）
  - 🎁 礼物赠送（橙色）
  - 👋 用户进入直播间（绿色）
  - 👍 点赞（红色）
  - ➕ 关注主播（蓝色）
- 关键词高亮（红底白字）
- 消息过滤复选框：**进播 / 关注 / 点赞 / 礼物**，可按需显示
- 消息计数统计
- 一键清空评论
- 停止监控时自动清空评论缓存

### 📊 数据导出
- 支持将弹幕、礼物、进场记录导出为 **CSV 文件**
- 字段：时间 / 类型 / 用户 / 内容 / 礼物名 / 数量

### 🔑 关键词管理
- 添加 / 删除监控关键词
- 评论区命中关键词自动高亮标红

### 📝 提词器
- 内置提词文本输入框
- 支持弹出独立提词器窗口
- 功能按钮：粗体（B）、置顶（📌）、隐身（👻）、颜色（🎨）、字体大小（A- / A+）、发送

---

## 系统要求

| 项目 | 要求 |
|------|------|
| 操作系统 | Windows 10 / 11（64位） |
| Python | 3.9 及以上（打包版无需安装） |
| FFplay / FFmpeg | 需安装并加入系统 PATH |

---

## 安装与运行

### 方式一：直接运行 EXE（推荐）

1. 从 [Releases](../../releases) 下载 `抖音直播监控.exe`
2. 确保系统已安装 FFmpeg（下载：https://ffmpeg.org/download.html）
3. 双击运行即可

> ⚠️ EXE 首次启动较慢（约 5~10 秒），属正常现象（单文件模式解压）

### 方式二：从源码运行

```bash
# 1. 克隆仓库
git clone https://github.com/yourname/douyin-live-monitor.git
cd douyin-live-monitor

# 2. 创建虚拟环境
python -m venv .venv
.venv\Scripts\activate

# 3. 安装依赖
pip install -r requirements.txt

# 4. 运行
python douyin_live_monitor.py
```

### 方式三：自行打包

```bash
pip install pyinstaller
pyinstaller --onefile --windowed --name "抖音直播监控" ^
  --hidden-import=douyin_live_chat ^
  --hidden-import=websocket ^
  --hidden-import=cv2 ^
  --hidden-import=PIL ^
  --hidden-import=psutil ^
  --hidden-import=betterproto ^
  douyin_live_monitor.py
```

打包完成后 EXE 位于 `dist\抖音直播监控.exe`

---

## 依赖列表

```
requests
websocket-client
opencv-python
Pillow
psutil
douyin-live-chat
betterproto
```

> 安装命令：`pip install -r requirements.txt`

---

## 使用方法

1. **启动程序**，在右上角「直播地址输入栏」粘贴抖音直播链接（支持分享短链接或直播间 URL）
2. 点击 **▶ Start** 开始监控
3. 左侧自动播放直播视频，右侧实时显示弹幕
4. 使用**过滤复选框**（进播 / 关注 / 点赞 / 礼物）控制评论区显示内容
5. 在**提词器**区域输入播报文案，点击 Send 发送到提词器窗口
6. 点击 **📥 Export** 导出弹幕数据为 CSV
7. 点击 **⏹ Stop** 停止监控

---

## 目录结构

```
douyin-live-monitor/
├── douyin_live_monitor.py   # 主程序
├── requirements.txt         # Python 依赖
├── README.md                # 项目文档
└── dist/
    └── 抖音直播监控.exe      # 打包后的可执行文件
```

---

## 常见问题

**Q：启动报错 `No module named 'requests'`**  
A：执行 `pip install requests` 安装缺失依赖。

**Q：视频无法播放**  
A：确认系统已安装 FFmpeg 并添加到 PATH 环境变量，命令行执行 `ffplay -version` 验证。

**Q：弹幕无法显示**  
A：确认直播间 URL 正确，且直播间正在开播状态。

**Q：EXE 被杀毒软件拦截**  
A：PyInstaller 打包的 EXE 可能触发误报，添加白名单即可，程序不含任何恶意代码。

**Q：关闭窗口后进程未完全退出**  
A：程序使用 psutil 强制清理所有子进程，如仍有残留可在任务管理器手动结束。

---

## 技术架构

```
主线程（Tkinter GUI）
  ├── StreamPlayer（视频播放线程）
  │     ├── OpenCV 视频帧读取 → Canvas 显示
  │     └── FFplay 子进程（无窗口音频）
  ├── ChatFetcher（弹幕抓取线程）
  │     └── DouyinLiveWebFetcher（WebSocket 连接）
  │           └── _wsOnMessage（全消息类型处理，types.MethodType 注入）
  └── DataStorage（数据存储与导出）
```

---

## 免责声明

本工具仅供学习研究使用，请勿用于任何商业用途或违反抖音平台服务协议的行为。使用本工具产生的一切法律责任由使用者自行承担。

---

## License

MIT License
