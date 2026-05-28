# Navi-Link - 高德地图导航悬浮窗

一个为高德地图设计的 Android 导航辅助悬浮窗应用，提供实时导航信息显示、红绿灯倒计时、巡航模式等功能。

## 📱 功能特性

### 核心功能
- **双模式支持**
  - 🚗 巡航模式：显示当前车速和道路名称
  - 🧭 导航模式：完整导航指引（转向图标、距离、路名、进度条等）

- **双样式切换**
  - 📋 常规风格：完整信息展示，适合大屏设备
  - 🏝️ 灵动岛风格：精简紧凑设计，节省屏幕空间

- **实时数据同步**
  - 接收高德地图导航广播数据
  - 实时更新速度、距离、转向提示
  - 红绿灯倒计时显示（含方向指示）
  - 剩余路程和时间预估

- **个性化配置**
  - 🎨 8种主题颜色可选
  - 🔍 0.5x - 2.0x 缩放调节
  - 📍 悬浮窗可拖拽和位置锁定
  - 💾 配置自动保存

- **智能管理**
  - 导航超时自动切换回巡航模式
  - 看门狗机制防止界面卡死
  - 前台服务保障稳定运行

## 🏗️ 技术架构

### 项目结构
```
Navi-Link/
├── app/src/main/java/com/navi/link/
│   ├── MainActivity.java              # 主配置界面
│   ├── AutoMapService.java            # 前台服务
│   ├── FloatingWindowManager.java     # 悬浮窗管理器（核心）
│   └── AmapNaviReceiver.java          # 高德广播接收器
├── app/src/main/res/layout/
│   ├── activity_main.xml                      # 主界面布局
│   ├── layout_floating_cruise.xml             # 巡航模式悬浮窗
│   ├── layout_floating_navi.xml               # 常规导航悬浮窗
│   ├── layout_floating_navi_minimal.xml       # 灵动岛导航悬浮窗
│   └── layout_floating_traffic_light_group.xml # 红绿灯胶囊组件
└── ...
```

### 核心组件说明

#### 1. MainActivity
- 提供悬浮窗配置UI
- 管理悬浮窗权限申请
- 启动/停止前台服务
- 实时显示悬浮窗运行状态

#### 2. AutoMapService
- 前台服务，保持应用后台运行
- 注册高德地图广播接收器
- 创建通知渠道和通知
- 管理悬浮窗生命周期

#### 3. FloatingWindowManager（单例）
- 悬浮窗的创建、显示、隐藏
- 模式切换（巡航 ↔ 导航）
- 样式切换（常规 ↔ 灵动岛）
- 窗口拖拽、缩放、主题色应用
- 超时管理和状态监控

#### 4. AmapNaviReceiver
- 接收高德地图广播 `AUTONAVI_STANDARD_BROADCAST_SEND`
- 解析导航数据（keyType: 10001）
- 解析红绿灯数据（keyType: 60073）
- 分发数据到悬浮窗更新

### 数据流
```
高德地图 App
    ↓ (广播)
AmapNaviReceiver
    ↓ (解析数据)
FloatingWindowManager
    ↓ (更新UI)
悬浮窗视图
```

## 📋 系统要求

- **Android 版本**: Android 7.0+ (API 24+)
- **目标版本**: Android 15 (API 36)
- **Java 版本**: Java 11
- **必需权限**:
  - 悬浮窗权限 (`SYSTEM_ALERT_WINDOW`)
  - 前台服务权限 (`FOREGROUND_SERVICE`)
  - 通知权限 (`POST_NOTIFICATIONS`)

## 🚀 快速开始

### 编译项目

```bash
# 克隆项目
git clone <repository-url>
cd Navi-Link

# 使用 Gradle 编译
./gradlew assembleDebug
```

### 安装运行

1. 在 Android Studio 中打开项目
2. 连接 Android 设备或启动模拟器
3. 点击 Run 按钮运行应用
4. 授予悬浮窗权限
5. 打开高德地图开始导航

### 使用说明

1. **首次启动**
   - 应用会自动请求悬浮窗权限
   - 授权后悬浮窗将自动显示

2. **配置悬浮窗**
   - 选择样式：常规 / 灵动岛
   - 调整缩放：0.5x - 2.0x
   - 选择主题色：8种颜色可选

3. **操作悬浮窗**
   - 拖动：移动悬浮窗位置
   - 长按：锁定/解锁位置
   - 查看：自动显示导航信息

4. **配合高德地图使用**
   - 打开高德地图 App
   - 开始导航
   - 悬浮窗会自动显示导航信息

## 🎨 界面预览

### 巡航模式
- 显示当前车速（km/h）
- 显示当前道路名称
- 显示前方红绿灯状态（如有）

### 导航模式 - 常规风格
- 转向图标（左转、右转、直行、掉头等）
- 剩余距离（米/公里）
- 目标道路名称
- 路线进度条
- 剩余路程和时间
- 预计到达时间
- 红绿灯倒计时胶囊

### 导航模式 - 灵动岛风格
- 紧凑布局，节省空间
- 车速 + 转向图标 + 距离
- 道路名称
- 红绿灯倒计时

## 🔧 开发指南

### 构建配置

项目使用 Gradle Version Catalogs 管理依赖：

```toml
# gradle/libs.versions.toml
[versions]
agp = "9.2.1"
appcompat = "1.6.1"
material = "1.10.0"
...
```

### 主要依赖

- AndroidX AppCompat
- Material Design Components
- ConstraintLayout
- Activity KTX

### 关键常量

```java
// FloatingWindowManager.java
MODE_CRUISE = 0          // 巡航模式
MODE_NAVI = 1            // 导航模式
NAVI_TIMEOUT_MS = 6000   // 导航超时时间（6秒）
WATCHDOG_TIMEOUT_MS = 5000 // 看门狗超时（5秒）
LIGHT_HIDE_TIMEOUT_MS = 5000 // 红绿灯隐藏超时（5秒）
```

### 高德广播数据类型

- **keyType = 10001**: 导航/巡航信息
  - `ICON`: 转向图标
  - `SEG_REMAIN_DIS_AUTO`: 分段剩余距离
  - `ROUTE_REMAIN_DIS_AUTO`: 全程剩余距离
  - `CUR_SPEED`: 当前车速
  - `CUR_ROAD_NAME`: 当前道路
  - `NEXT_ROAD_NAME`: 下一道路
  
- **keyType = 60073**: 红绿灯数据
  - `trafficLightStatus`: 红绿灯状态
  - `dir`: 方向
  - `redLightCountDownSeconds`: 倒计时
  - `lightsData`: 巡航模式红绿灯数组（JSON）

## 📝 待办事项

- [ ] 支持更多导航 App（百度地图、腾讯地图等）
- [ ] 添加夜间模式自动切换
- [ ] 优化内存占用
- [ ] 添加更多自定义选项
- [ ] 支持横屏适配
- [ ] 添加动画效果

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## 📄 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## ⚠️ 免责声明

- 本应用仅作为导航辅助工具，不构成驾驶建议
- 请在安全驾驶的前提下使用本应用
- 作者不对因使用本应用造成的任何事故负责
- 高德地图为第三方应用，本应用与其无官方合作关系

## 📞 联系方式

如有问题或建议，请提交 Issue 或联系开发者。

---

**Made with ❤️ for safer navigation**
