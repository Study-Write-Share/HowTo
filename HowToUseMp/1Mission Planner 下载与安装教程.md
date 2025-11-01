# Mission Planner 下载与安装教程

Mission Planner 是一款用于 ArduPilot 开源自动驾驶仪的地面站软件，广泛应用于无人机、无人车等设备的配置和飞行控制。

## 系统要求

- **操作系统**: Windows 7/8/10/11 (推荐 Windows 10)
- **.NET Framework**: 4.8 或更高版本
- **存储空间**: 至少 500MB 可用空间

## 下载步骤

### 方法一：官方网站下载（推荐）

1. 访问 Mission Planner 官方网站：https://ardupilot.org/planner/
2. 点击页面中的 **"Download Mission Planner"**按钮
3. 选择适合的版本：**稳定版**(Stable)：适合大多数用户**测试版**(Beta)：包含最新功能，但可能存在不稳定因素

### 方法二：GitHub 下载

1. 访问 Mission Planner 的 GitHub 发布页面：https://github.com/ArduPilot/MissionPlanner/releases
2. 找到最新版本，下载以下文件之一：`MissionPlanner-x.x.xx.zip`（便携版）`MissionPlanner-x.x.xx.msi`（安装版）

## 安装步骤

### 使用安装程序 (.msi)

1. **运行安装程序**双击下载的 `.msi`文件如果出现安全警告，点击"运行"
2. **遵循安装向导**选择安装语言接受许可协议选择安装目录（默认即可）点击"安装"开始安装过程
3. **完成安装**安装完成后，点击"完成"Mission Planner 将自动启动

### 使用便携版 (.zip)

1. **解压文件**右键点击下载的 zip 文件选择"全部解压缩"选择目标文件夹并解压
2. **运行程序**进入解压后的文件夹双击 `MissionPlanner.exe`启动程序

## 首次运行设置

1. **选择语言**首次启动时，选择界面语言（支持中文）
2. **配置连接**在主界面选择连接方式（USB、蓝牙、TCP等）设置正确的端口和波特率
3. **校准传感器**（可选但推荐）按照向导进行加速度计、罗盘等传感器的校准

## 常见问题解决

### .NET Framework 错误

如果出现 .NET Framework 相关错误：

- 下载并安装最新版 .NET Framework：https://dotnet.microsoft.com/download/dotnet-framework

### 连接问题

- 确保已安装正确的 USB 驱动程序
- 检查数据线是否支持数据传输（不只是充电）

### 杀毒软件误报

某些杀毒软件可能误报 Mission Planner，请将其添加到白名单。

## 更新 Mission Planner

Mission Planner 支持自动更新：

- 帮助菜单 → 检查更新
- 或重新下载最新版本进行覆盖安装

## 注意事项

1. **管理员权限**：建议以管理员身份运行，避免权限问题
2. **防火墙设置**：确保防火墙不阻止程序运行
3. **数据备份**：定期备份任务计划和日志文件

## 获取帮助

- **官方文档**: https://ardupilot.org/planner/
- **用户论坛**: https://discuss.ardupilot.org/
- **GitHub Issues**: https://github.com/ArduPilot/MissionPlanner/issues

