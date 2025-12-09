🕒 Java 编程时间戳工具（C# WinForms）
一个基于 C# WinForms 开发的小工具，用于从网络 NTP 服务器获取当前时间，根据用户输入的姓名与时间偏移自动生成格式化提示信息。常用于课堂教学、考试提示、实验系统辅助等场景。

📘 功能简介
获取网络时间（NTP）
通过访问 ntp.aliyun.com 获得精准的网络时间。

姓名输入与智能格式化
自动拆分姓名 → “姓 + 名”。

支持输入时间偏移（天）
可将当前时间往前偏移 N 天，用于模拟历史时间、考试试卷回溯等用途。

生成统一格式化提示文本
包含日期、时间、完整时间戳及温馨提示。

图形化界面（WinForms）
配合背景图和图标，视觉整洁美观。

🖼 界面展示（示例）
你可运行后自行添加截图
示例：
![preview](./screenshot.png)

📥 使用方法
打开程序。

在输入框填写：

姓名

偏移天数（整数，可选）

点击 “时 间 戳” 按钮。

程序会：

请求 NTP 网络时间

按输入进行时间偏移

自动格式化输出信息，例如：

yaml
复制代码
姓张名三 2025-03-21
温馨提示现在是
张三14:21:54作答《Java编程》
2025-03-21 14:21:54 请劳逸结合
🔧 核心功能逻辑说明
1. NTP 时间获取
通过 UDP 访问 NTP 服务器，解析 48 字节 NTP 数据包：

csharp
复制代码
byte[] ntpData = new byte[48];
ntpData[0] = 27;
client.Connect(ipAddress, 123);
await client.SendAsync(ntpData, ntpData.Length);
ntpData = (await client.ReceiveAsync()).Buffer;
解析时间戳：

csharp
复制代码
ulong intPart = BitConverter.ToUInt32(ntpData, 40);
ulong fractPart = BitConverter.ToUInt32(ntpData, 44);
DateTime networkTime = new DateTime(1900, 1, 1)
    .AddSeconds(intPart)
    .AddMilliseconds(fractPart * 1000.0 / 4294967296.0)
    .ToLocalTime();
📂 项目结构
bash
复制代码
/
├── Form1.cs                 # 主窗体逻辑：输入、按钮事件、NTP 时间处理
├── Form1.Designer.cs        # WinForms 自动生成布局文件
├── Properties/
│   ├── Resources.resx       # 图片、图标资源
│   ├── Resources.Designer.cs
│   ├── Settings.settings
├── Resources/
│   ├── BackgroundImage.png  # 程序背景图（示例）
│   ├── clock.ico            # 程序图标
├── README.md                # 项目说明文件（本文件）
🛠 使用到的技术
C# (.NET Framework)

Windows Forms (WinForms)

异步编程 async/await

UDP + NTP 协议解析

Resources 资源管理

📈 未来可扩展
自定义 NTP 服务器选择

显示网络延迟/同步状态

导出生成的提示结果到 .txt 或 .csv

UI 美化 / 换用 WPF

多语言界面支持

