# Inventory Kamera 开发者文档

本文档旨在帮助开发者理解项目结构并成功编译打包 Inventory Kamera 项目。

## 目录

- [编译指南](#编译指南)
  - [前置要求](#前置要求)
  - [获取源代码](#获取源代码)
  - [安装和配置开发环境](#安装和配置开发环境)
  - [编译项目](#编译项目)
  - [运行和调试](#运行和调试)
  - [发布打包](#发布打包)
  - [常见问题排查](#常见问题排查)
- [项目结构说明](#项目结构说明)
  - [总体架构](#总体架构)
  - [目录结构](#目录结构)
  - [核心组件说明](#核心组件说明)
  - [依赖项说明](#依赖项说明)

---

## 编译指南

本指南将详细说明如何从零开始编译 Inventory Kamera 项目，即使您是刚接触 Visual Studio 的初学者也能顺利完成。

### 前置要求

在开始编译之前，您需要准备以下软件和工具：

#### 1. 操作系统要求
- **Windows 10** 或更高版本（推荐 Windows 10/11）
- 64位操作系统（推荐）

#### 2. Visual Studio 2022（或 2019）

**Visual Studio Community 版本**（免费）即可满足需求。

**下载地址：**
- Visual Studio 2022: https://visualstudio.microsoft.com/zh-hans/downloads/
- 选择 "Community" 版本下载

**安装步骤：**

1. 运行下载的 Visual Studio 安装程序
2. 在安装界面中，选择以下工作负载：
   - ✅ **.NET 桌面开发** （必选）
     - 这将自动安装 .NET Framework 4.7.2 SDK 及相关工具
3. 在 "单个组件" 选项卡中，确保以下组件已选中：
   - ✅ .NET Framework 4.7.2 SDK
   - ✅ .NET Framework 4.7.2 目标包
   - ✅ NuGet 包管理器
4. 点击 "安装" 按钮，等待安装完成（可能需要 30-60 分钟）
5. 安装完成后重启计算机

**验证安装：**
```powershell
# 打开 PowerShell 或命令提示符，运行：
dotnet --version
# 应该显示已安装的 .NET SDK 版本
```

#### 3. .NET Framework 4.7.2 或更高版本

如果您已经安装了 Visual Studio 并选择了 ".NET 桌面开发" 工作负载，.NET Framework 4.7.2 应该已经自动安装。

如果需要单独安装：
- 下载地址: https://dotnet.microsoft.com/zh-cn/download/dotnet-framework/net472
- 下载 "开发人员包"（Developer Pack）

#### 4. Git（可选但推荐）

用于克隆和管理源代码。

**下载地址：** https://git-scm.com/downloads

**安装步骤：**
1. 下载并运行安装程序
2. 安装过程中保持默认选项即可
3. 安装完成后，在命令提示符中运行 `git --version` 验证安装

### 获取源代码

有两种方式获取项目源代码：

#### 方式一：使用 Git 克隆（推荐）

1. 打开命令提示符或 PowerShell
2. 导航到您想要存放项目的目录，例如：
   ```cmd
   cd C:\Projects
   ```
3. 克隆仓库：
   ```cmd
   git clone https://github.com/KokomiSensei/Inventory_Kamera.git
   ```
4. 进入项目目录：
   ```cmd
   cd Inventory_Kamera
   ```

#### 方式二：下载 ZIP 压缩包

1. 访问项目 GitHub 页面：https://github.com/KokomiSensei/Inventory_Kamera
2. 点击绿色的 "Code" 按钮
3. 选择 "Download ZIP"
4. 下载完成后，解压到您选择的目录，例如 `C:\Projects\Inventory_Kamera`

### 安装和配置开发环境

#### 1. 打开项目

1. 启动 Visual Studio 2022
2. 在启动界面中，选择 "打开项目或解决方案"
3. 浏览到项目目录，选择 `InventoryKamera.sln` 文件
4. 点击 "打开"

#### 2. 还原 NuGet 包

项目使用 NuGet 包管理器来管理第三方依赖项。首次打开项目时，需要还原这些包。

**自动还原（推荐）：**
- Visual Studio 通常会自动检测并提示还原 NuGet 包
- 如果看到提示 "部分 NuGet 包丢失"，点击 "还原" 按钮

**手动还原：**
1. 在菜单栏中选择 "工具" > "NuGet 包管理器" > "程序包管理器控制台"
2. 在打开的控制台窗口中，运行：
   ```powershell
   Update-Package -reinstall
   ```
3. 或者在 "解决方案资源管理器" 中，右键点击解决方案，选择 "还原 NuGet 程序包"

**验证包还原：**
- 在 "解决方案资源管理器" 中展开项目
- 查看 "依赖项" > "NuGet" 节点
- 应该能看到所有依赖包，且没有黄色警告图标

#### 3. 配置生成平台

1. 在 Visual Studio 顶部工具栏中，找到解决方案配置下拉菜单
2. 选择配置：
   - **Debug**：用于开发和调试（包含调试符号，未优化）
   - **Release**：用于发布（已优化，体积更小）
3. 选择平台：
   - **Any CPU**：适用于大多数情况
   - **x64**：专门用于 64 位系统
   - **x86**：专门用于 32 位系统

**推荐配置：**
- 开发调试时：**Debug | Any CPU**
- 发布打包时：**Release | Any CPU** 或 **Release | x64**

### 编译项目

#### 方式一：使用 Visual Studio IDE

1. 在 Visual Studio 中打开解决方案（`InventoryKamera.sln`）
2. 选择配置（Debug 或 Release）和平台
3. 编译整个解决方案：
   - 按快捷键 **Ctrl + Shift + B**
   - 或者在菜单栏选择 "生成" > "生成解决方案"
4. 等待编译完成，观察 "输出" 窗口中的信息
5. 如果编译成功，您将看到类似以下的消息：
   ```
   ========== 生成: 成功 1 个，失败 0 个，最新 0 个，跳过 0 个 ==========
   ```

**编译输出位置：**
- Debug 版本：`InventoryKamera\bin\Debug\InventoryKamera.exe`
- Release 版本：`InventoryKamera\bin\Release\InventoryKamera.exe`

#### 方式二：使用 MSBuild 命令行

如果您更喜欢使用命令行，或需要在持续集成环境中编译：

1. 打开 "Developer Command Prompt for VS 2022"（在开始菜单的 Visual Studio 2022 文件夹中）
2. 导航到项目目录：
   ```cmd
   cd C:\Projects\Inventory_Kamera
   ```
3. 还原 NuGet 包：
   ```cmd
   nuget restore InventoryKamera.sln
   ```
4. 编译项目：
   ```cmd
   msbuild InventoryKamera.sln /p:Configuration=Release /p:Platform="Any CPU"
   ```

**参数说明：**
- `/p:Configuration=Release`：编译 Release 版本（可选 Debug）
- `/p:Platform="Any CPU"`：目标平台（可选 x86, x64）
- `/t:Rebuild`：（可选）清理后重新编译

**示例命令：**
```cmd
# 编译 Release 版本
msbuild InventoryKamera.sln /p:Configuration=Release /p:Platform="Any CPU" /t:Rebuild

# 编译 Debug 版本
msbuild InventoryKamera.sln /p:Configuration=Debug /p:Platform="Any CPU"
```

### 运行和调试

#### 在 Visual Studio 中运行

1. 确保项目已成功编译
2. 按 **F5** 键（调试模式）或 **Ctrl + F5**（非调试模式）
3. 应用程序将启动，您可以看到 Inventory Kamera 的主界面

#### 设置断点调试

1. 在代码编辑器中，点击要设置断点的代码行左侧的灰色边栏
2. 会出现一个红色圆点，表示断点已设置
3. 按 **F5** 启动调试
4. 当程序执行到断点处时会暂停
5. 使用调试工具栏进行单步调试：
   - **F10**：单步跳过（Step Over）
   - **F11**：单步进入（Step Into）
   - **Shift + F11**：单步跳出（Step Out）
   - **F5**：继续执行

#### 直接运行已编译的程序

1. 使用文件资源管理器导航到编译输出目录
2. 双击 `InventoryKamera.exe` 运行程序

**注意：** 首次运行时，Windows 可能会显示安全警告，点击 "更多信息" 然后 "仍要运行" 即可。

### 发布打包

#### 方式一：使用 Visual Studio 发布功能

1. 在 "解决方案资源管理器" 中，右键点击 "InventoryKamera" 项目
2. 选择 "发布..."
3. 如果是首次发布，需要配置发布配置文件：
   - 选择目标：**文件夹**
   - 选择特定目标：**文件夹**
   - 选择位置：例如 `bin\Publish\`
   - 点击 "完成"
4. 在发布配置界面：
   - 配置：**Release | Any CPU**
   - 目标框架：**.NET Framework 4.7.2**
   - 部署模式：**独立**（或 "框架依赖"，如果用户已安装 .NET Framework）
5. 点击 "发布" 按钮
6. 发布完成后，在输出文件夹中会包含所有必需的文件

#### 方式二：手动打包

1. 编译 Release 版本（参见前面的编译步骤）
2. 找到编译输出目录：`InventoryKamera\bin\Release\`
3. 创建一个新文件夹，例如 `InventoryKamera_Release`
4. 将以下文件和文件夹复制到新文件夹：
   - `InventoryKamera.exe`（主程序）
   - `InventoryKamera.exe.config`（配置文件）
   - 所有 `.dll` 文件（依赖库）
   - `tessdata` 文件夹（OCR 训练数据）
   - `Item_Special_Kamera.ico`（图标文件，如果存在）
5. （可选）创建一个 ZIP 压缩包以便分发

#### 发布检查清单

在分发打包后的程序之前，请确保：

- [ ] 编译使用 Release 配置
- [ ] 所有依赖的 DLL 文件都已包含
- [ ] `tessdata` 文件夹及其内容已包含
- [ ] 在没有安装 Visual Studio 的干净 Windows 系统上测试运行
- [ ] 确认用户已安装 [Microsoft Visual C++ Redistributable](https://docs.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist)
- [ ] 版本号已在 `AssemblyInfo.cs` 中正确更新

### 常见问题排查

#### 问题 1：NuGet 包还原失败

**症状：**
```
无法还原 NuGet 包
无法连接到 NuGet 源
```

**解决方案：**
1. 检查网络连接
2. 在 Visual Studio 中，转到 "工具" > "选项" > "NuGet 包管理器" > "程序包源"
3. 确保 `nuget.org` 源已启用：`https://api.nuget.org/v3/index.json`
4. 清除 NuGet 缓存：
   ```cmd
   nuget locals all -clear
   ```
5. 重新还原包：
   ```cmd
   nuget restore InventoryKamera.sln
   ```

#### 问题 2：编译错误 - "找不到 .NET Framework 4.7.2"

**症状：**
```
错误：当前项目的目标是".NETFramework,Version=v4.7.2"，但未安装该版本
```

**解决方案：**
1. 安装 .NET Framework 4.7.2 Developer Pack
2. 下载地址：https://dotnet.microsoft.com/download/dotnet-framework/net472
3. 安装后重启 Visual Studio

#### 问题 3：编译错误 - "缺少命名空间或类型"

**症状：**
```
错误 CS0246: 未能找到类型或命名空间名"Tesseract"
错误 CS0246: 未能找到类型或命名空间名"Newtonsoft"
```

**解决方案：**
这通常是因为 NuGet 包未正确还原。
1. 在 "解决方案资源管理器" 中右键点击解决方案
2. 选择 "还原 NuGet 程序包"
3. 等待还原完成后重新编译

#### 问题 4：运行时错误 - "无法加载 DLL"

**症状：**
```
System.DllNotFoundException: 无法加载 DLL 'xxx.dll'
```

**解决方案：**
1. 确保所有依赖的 DLL 文件都在程序目录中
2. 安装 [Microsoft Visual C++ Redistributable](https://aka.ms/vs/17/release/vc_redist.x64.exe)
3. 如果是 Tesseract OCR 相关错误，确保 `tessdata` 文件夹在程序目录中

#### 问题 5：编译成功但无法运行

**症状：**
双击 EXE 文件后程序立即崩溃或无反应

**解决方案：**
1. 检查 Windows 事件查看器中的错误日志
2. 确保已安装 .NET Framework 4.7.2 运行时（不是 SDK）
3. 以管理员权限运行程序
4. 检查杀毒软件是否阻止了程序运行
5. 查看应用程序生成的日志文件：`logging/InventoryKamera.debug.log`

#### 问题 6：Visual Studio 版本兼容性

**症状：**
使用 Visual Studio 2017 或更早版本无法打开项目

**解决方案：**
1. 推荐升级到 Visual Studio 2019 或 2022
2. 如果必须使用旧版本，可能需要手动编辑 `.sln` 和 `.csproj` 文件中的版本号
3. 不推荐这种做法，因为可能导致兼容性问题

#### 问题 7：编译后体积过大

**症状：**
Release 版本的 EXE 文件异常大

**解决方案：**
1. 确保使用 Release 配置而非 Debug
2. 在项目属性中，"生成" 选项卡，取消选中 "生成调试信息"
3. 确保 "优化代码" 选项已启用

#### 获取更多帮助

如果遇到其他问题：

1. **查看日志文件：** `logging/InventoryKamera.debug.log`
2. **搜索现有 Issues：** https://github.com/KokomiSensei/Inventory_Kamera/issues
3. **创建新 Issue：** 如果问题未被报告，请创建新 Issue 并附上：
   - 详细的错误信息
   - 操作系统版本
   - Visual Studio 版本
   - 重现步骤
4. **加入 Discord 社区：** https://discord.gg/zh56aVWe3U

---

## 项目结构说明

### 总体架构

Inventory Kamera 是一个基于 Windows Forms 的 C# 桌面应用程序，使用 OCR（光学字符识别）技术来扫描和识别《原神》游戏中的物品、角色、武器和圣遗物信息。

**核心技术栈：**
- **开发语言：** C# 
- **框架：** .NET Framework 4.7.2
- **UI 框架：** Windows Forms
- **OCR 引擎：** Tesseract OCR
- **图像处理：** Accord.NET
- **JSON 处理：** Newtonsoft.Json
- **输入模拟：** InputSimulator

**应用架构：**
```
┌─────────────────────────────────────────────────┐
│              用户界面层 (UI Layer)                │
│         MainForm, MainUI, ExecutablesForm       │
└───────────────────┬─────────────────────────────┘
                    │
┌───────────────────▼─────────────────────────────┐
│           业务逻辑层 (Business Logic)            │
│        InventoryKamera, GenshinProcessor        │
└───────────────────┬─────────────────────────────┘
                    │
        ┌───────────┼───────────┐
        │           │           │
┌───────▼──────┐ ┌──▼────────┐ ┌▼──────────────┐
│   扫描器层    │ │  游戏层   │ │   数据层      │
│  (Scraping)  │ │  (Game)   │ │   (Data)      │
├──────────────┤ ├───────────┤ ├───────────────┤
│ Artifact     │ │ Navigation│ │ Database      │
│ Character    │ │ Artifact  │ │ GOOD Export   │
│ Weapon       │ │ Character │ │ Inventory     │
│ Material     │ │ Weapon    │ │ OCR Images    │
│ Inventory    │ │           │ │               │
└──────────────┘ └───────────┘ └───────────────┘
        │               │               │
        └───────────────┼───────────────┘
                        │
            ┌───────────▼───────────┐
            │   外部依赖/资源        │
            │ • Tesseract OCR       │
            │ • Game Data (JSON)    │
            │ • Training Data       │
            └───────────────────────┘
```

### 目录结构

```
Inventory_Kamera/
│
├── InventoryKamera.sln              # Visual Studio 解决方案文件
├── README.md                         # 用户使用说明（英文）
├── LICENSE                           # MIT 许可证
├── .gitignore                        # Git 忽略文件配置
├── .gitattributes                    # Git 属性配置
│
└── InventoryKamera/                  # 主项目目录
    │
    ├── InventoryKamera.csproj        # 项目文件（定义编译配置和依赖）
    ├── App.config                    # 应用程序配置文件
    ├── app.manifest                  # 应用程序清单（权限等）
    ├── Program.cs                    # 应用程序入口点
    ├── Settings.cs                   # 用户设置类
    ├── Item_Special_Kamera.ico       # 应用程序图标
    │
    ├── Properties/                   # 项目属性
    │   ├── AssemblyInfo.cs          # 程序集信息（版本号等）
    │   ├── Settings.settings        # 用户设置定义
    │   ├── Settings.Designer.cs     # 自动生成的设置代码
    │   ├── Resources.resx           # 资源文件
    │   ├── Resources.Designer.cs    # 自动生成的资源代码
    │   └── JsonUserSettingsProvider.cs # JSON 格式的用户设置提供程序
    │
    ├── data/                         # 数据管理层
    │   ├── DatabaseManager.cs       # 数据库管理（JSON 数据文件读写）
    │   ├── GOOD.cs                  # GOOD 格式导出（通用原神数据格式）
    │   ├── Inventory.cs             # 库存数据结构
    │   ├── InventoryKamera.cs       # 核心扫描逻辑
    │   ├── OCRImageCollection.cs    # OCR 图像集合管理
    │   └── Rect.cs                  # 矩形区域定义（用于 OCR 区域）
    │
    ├── game/                         # 游戏实体和导航
    │   ├── Artifact.cs              # 圣遗物数据模型
    │   ├── Character.cs             # 角色数据模型
    │   ├── Weapon.cs                # 武器数据模型
    │   └── Navigation.cs            # 游戏内导航控制（模拟键盘鼠标输入）
    │
    ├── scraping/                     # 扫描器实现
    │   ├── GenshinProcessor.cs      # 原神进程管理和截图
    │   ├── InventoryScraper.cs      # 物品库存扫描器
    │   ├── ArtifactScraper.cs       # 圣遗物扫描器
    │   ├── CharacterScraper.cs      # 角色扫描器
    │   ├── WeaponScraper.cs         # 武器扫描器
    │   └── MaterialScraper.cs       # 材料扫描器
    │
    ├── ui/                           # 用户界面
    │   ├── UserInterface.cs         # UI 基类和通用功能
    │   ├── ExecutablesForm.cs       # 可执行文件选择窗体
    │   ├── ExecutablesForm.Designer.cs
    │   ├── ExecutablesForm.resx
    │   └── main/                    # 主界面
    │       ├── MainForm.cs          # 主窗体（旧版 UI）
    │       ├── MainForm.Designer.cs
    │       ├── MainForm.resx
    │       ├── MainUI.cs            # 新版主界面
    │       ├── MainUI.Designer.cs
    │       └── MainUI.resx
    │
    └── tessdata/                     # Tesseract OCR 训练数据
        ├── genshin_best_eng.traineddata    # 高精度英文识别模型
        └── genshin_fast_09_04_21.traineddata # 快速识别模型
```

### 核心组件说明

#### 1. 入口点和初始化 (`Program.cs`)

**职责：**
- 应用程序入口
- 配置日志系统（NLog）
- 设置 DPI 感知
- 启动主窗体

**关键代码：**
```csharp
Application.Run(new MainForm());  // 启动主窗体
```

#### 2. 用户界面层 (`ui/`)

**MainForm / MainUI：**
- 应用程序的主窗口
- 提供扫描配置选项（武器、圣遗物、角色等）
- 控制扫描流程的启动和停止
- 显示扫描进度和结果

**ExecutablesForm：**
- 允许用户选择游戏可执行文件
- 支持 GenshinImpact.exe（国际服）和 YuanShen.exe（国服）

**UserInterface：**
- UI 组件的基类
- 提供通用的 UI 功能和状态管理

#### 3. 游戏进程管理 (`scraping/GenshinProcessor.cs`)

**职责：**
- 检测和连接到原神游戏进程
- 获取游戏窗口句柄
- 截取游戏窗口特定区域的屏幕截图
- 管理 OCR 引擎初始化

**关键功能：**
- 窗口检测和激活
- 屏幕截图采集
- 分辨率和 DPI 处理

#### 4. 导航控制 (`game/Navigation.cs`)

**职责：**
- 模拟键盘和鼠标输入
- 在游戏菜单中导航
- 控制物品滚动和页面切换

**关键功能：**
```csharp
- OpenInventory()     // 打开背包
- OpenCharacterScreen() // 打开角色界面
- ScrollUp() / ScrollDown() // 滚动列表
- SelectCharacter()   // 选择角色
```

#### 5. 扫描器层 (`scraping/`)

每个扫描器负责特定类型数据的识别和提取：

**InventoryScraper：**
- 扫描背包中的材料和物品
- 处理不同稀有度的物品过滤

**ArtifactScraper：**
- 扫描圣遗物
- 识别主词条、副词条、等级、星级
- 提取套装信息

**CharacterScraper：**
- 扫描角色信息
- 识别角色等级、命座、天赋等级
- 提取角色元素和武器类型

**WeaponScraper：**
- 扫描武器
- 识别武器等级、精炼、主副属性

**MaterialScraper：**
- 扫描材料物品
- 识别材料数量和类型

#### 6. 数据模型 (`game/`)

**Artifact.cs：**
```csharp
public class Artifact
{
    public string SetName { get; set; }      // 套装名称
    public string Slot { get; set; }         // 部位（花、羽、沙、杯、冠）
    public int Level { get; set; }           // 等级
    public int Rarity { get; set; }          // 稀有度（星级）
    public string MainStat { get; set; }     // 主属性
    public List<SubStat> SubStats { get; set; } // 副属性列表
    // ...
}
```

**Character.cs：**
```csharp
public class Character
{
    public string Name { get; set; }         // 角色名
    public int Level { get; set; }           // 等级
    public int Constellation { get; set; }   // 命座
    public string Element { get; set; }      // 元素
    public Dictionary<string, int> Talents { get; set; } // 天赋等级
    // ...
}
```

**Weapon.cs：**
```csharp
public class Weapon
{
    public string Name { get; set; }         // 武器名
    public int Level { get; set; }           // 等级
    public int Refinement { get; set; }      // 精炼
    public int Rarity { get; set; }          // 稀有度
    // ...
}
```

#### 7. 数据管理 (`data/`)

**DatabaseManager.cs：**
- 管理游戏数据库（JSON 文件）
- 加载武器、圣遗物、角色、材料的名称列表
- 提供数据查询和匹配功能
- 支持自动更新数据库

**数据文件位置：**
```
inventorylists/
├── artifacts.json      # 圣遗物数据
├── characters.json     # 角色数据
├── weapons.json        # 武器数据
└── materials.json      # 材料数据
```

**GOOD.cs：**
- 实现 GOOD（Genshin Open Object Description）格式导出
- GOOD 是一个通用的原神数据交换格式
- 兼容多个在线工具：Genshin Optimizer、SEELIE.me 等

**导出格式示例：**
```json
{
  "format": "GOOD",
  "version": 1,
  "source": "Inventory_Kamera",
  "characters": [...],
  "weapons": [...],
  "artifacts": [...]
}
```

#### 8. OCR 处理 (`data/OCRImageCollection.cs`)

**职责：**
- 管理待识别的图像集合
- 定义 OCR 识别区域（ROI - Region of Interest）
- 协调 Tesseract OCR 引擎进行文字识别

**OCR 区域类型：**
- 物品名称区域
- 属性值区域
- 等级区域
- 数量区域

#### 9. 配置管理 (`Settings.cs`, `App.config`)

**用户可配置项：**
- 扫描选项（武器、圣遗物、角色、材料）
- 最低稀有度过滤
- 最低等级过滤
- 扫描延迟设置
- 键位绑定（背包键、角色界面键）
- 输出路径
- 日志选项

**配置存储：**
- 使用 JSON 格式存储在用户配置文件中
- 位置：`%AppData%\Local\InventoryKamera\`

### 依赖项说明

项目使用 NuGet 管理以下第三方库：

#### 核心依赖

1. **Tesseract (v5.2.0)**
   - OCR 引擎
   - 用于识别游戏内文字
   - 项目包含针对原神优化的训练数据

2. **Accord.Imaging (v3.8.0)**
   - 图像处理库
   - 用于图像预处理、增强、阈值化等
   - 提高 OCR 识别准确率

3. **Newtonsoft.Json (v13.0.2)**
   - JSON 序列化/反序列化
   - 用于数据库文件读写和 GOOD 格式导出

4. **InputSimulator (v1.0.4)**
   - 模拟键盘和鼠标输入
   - 用于游戏内导航控制

5. **NLog (v5.1.1)**
   - 日志框架
   - 记录应用程序运行日志和错误信息

#### 辅助依赖

6. **Octokit (v7.1.0)**
   - GitHub API 客户端
   - 用于检查应用更新和数据库更新

7. **NHotkey.WindowsForms (v2.1.0)**
   - 全局热键支持
   - 允许用户通过热键控制扫描

8. **HtmlAgilityPack.NetCore (v1.5.0.1)**
   - HTML 解析
   - 用于从网页获取游戏数据更新

9. **Microsoft-WindowsAPICodePack-Shell (v1.1.5)**
   - Windows Shell API 包装
   - 提供文件对话框等系统功能

#### 系统依赖

10. **System.Drawing**
    - 图像和图形处理
    - .NET Framework 内置

11. **System.Windows.Forms**
    - Windows Forms UI 框架
    - .NET Framework 内置

### 工作流程说明

#### 完整扫描流程

```
1. 用户启动扫描
   │
   ├──▶ 检测原神进程
   │    └──▶ 如果未找到，提示用户启动游戏
   │
   ├──▶ 初始化 OCR 引擎
   │    └──▶ 加载训练数据
   │
   ├──▶ 加载数据库
   │    └──▶ 读取 weapons.json, artifacts.json 等
   │
   ├──▶ 根据用户配置，依次执行扫描：
   │    │
   │    ├──▶ 扫描角色
   │    │    ├── 打开角色界面
   │    │    ├── 遍历所有角色
   │    │    ├── 截图并 OCR
   │    │    └── 提取角色信息
   │    │
   │    ├──▶ 扫描武器
   │    │    ├── 打开背包
   │    │    ├── 切换到武器标签
   │    │    ├── 滚动列表
   │    │    ├── 截图并 OCR
   │    │    └── 提取武器信息
   │    │
   │    ├──▶ 扫描圣遗物
   │    │    ├── 切换到圣遗物标签
   │    │    ├── 滚动列表
   │    │    ├── 逐个点击圣遗物
   │    │    ├── 截图详细信息
   │    │    ├── OCR 识别属性
   │    │    └── 提取圣遗物数据
   │    │
   │    └──▶ 扫描材料
   │         ├── 切换到材料标签
   │         ├── 滚动列表
   │         ├── 截图并 OCR
   │         └── 提取材料和数量
   │
   └──▶ 导出数据
        ├── 生成 GOOD 格式 JSON
        ├── 保存到用户指定目录
        └── 显示完成提示
```

#### OCR 识别流程

```
1. 截取游戏窗口特定区域
   │
   ├──▶ 图像预处理
   │    ├── 转换为灰度图
   │    ├── 调整对比度
   │    ├── 二值化处理
   │    └── 去噪
   │
   ├──▶ Tesseract OCR 识别
   │    ├── 使用自定义训练数据
   │    ├── 提取文本
   │    └── 返回识别结果
   │
   ├──▶ 文本后处理
   │    ├── 清理特殊字符
   │    ├── 标准化格式
   │    └── 修正常见错误
   │
   └──▶ 数据匹配
        ├── 与数据库进行模糊匹配
        ├── 选择最佳匹配项
        └── 返回标准化名称
```

### 开发建议

#### 代码规范

- **命名约定：** 遵循 C# 标准命名规范（PascalCase 用于类型和方法，camelCase 用于局部变量）
- **注释：** 为复杂逻辑添加注释，特别是 OCR 相关的图像处理代码
- **异常处理：** 使用 try-catch 捕获并记录异常到日志

#### 测试建议

- **单元测试：** 可以考虑为核心逻辑（如数据解析、匹配算法）添加单元测试
- **集成测试：** 使用真实的游戏截图测试 OCR 准确性
- **UI 测试：** 手动测试各种屏幕分辨率和 DPI 设置

#### 性能优化

- **OCR 优化：** OCR 是性能瓶颈，考虑：
  - 减少不必要的截图
  - 优化图像预处理
  - 使用多线程处理
- **内存管理：** 及时释放图像资源
- **异步操作：** 使用 async/await 避免 UI 冻结

#### 扩展性考虑

如果要添加新功能，建议：

1. **新的扫描类型：** 在 `scraping/` 目录下创建新的 Scraper 类
2. **新的数据模型：** 在 `game/` 目录下定义新的实体类
3. **新的导出格式：** 在 `data/` 目录下实现新的导出类
4. **UI 改进：** 在 `ui/` 目录下修改或创建新的窗体

### 贡献指南

如果您想为项目贡献代码：

1. Fork 项目仓库
2. 创建功能分支：`git checkout -b feature/your-feature`
3. 进行更改并编写测试
4. 提交更改：`git commit -m "Add your feature"`
5. 推送到分支：`git push origin feature/your-feature`
6. 创建 Pull Request

### 版本发布流程

1. 更新版本号：
   - 修改 `Properties/AssemblyInfo.cs` 中的 `AssemblyVersion`
   - 更新 `InventoryKamera.csproj` 中的 `ApplicationVersion`
2. 编译 Release 版本
3. 进行完整测试
4. 创建 Git 标签：`git tag v1.x.x`
5. 打包并创建 GitHub Release
6. 上传编译产物和更新日志

### 相关资源

- **GitHub 仓库：** https://github.com/KokomiSensei/Inventory_Kamera
- **原始仓库：** https://github.com/Andrewthe13th/Inventory_Kamera
- **Discord 社区：** https://discord.gg/zh56aVWe3U
- **GOOD 格式规范：** https://frzyc.github.io/genshin-optimizer/#/doc
- **Tesseract OCR 文档：** https://github.com/tesseract-ocr/tesseract

---

## 总结

本文档提供了 Inventory Kamera 项目的完整开发指南，包括：

✅ **详细的编译指南** - 从安装 Visual Studio 到成功编译发布  
✅ **故障排除** - 常见问题及解决方案  
✅ **项目结构说明** - 清晰的目录和组件说明  
✅ **架构设计** - 理解项目的整体架构和工作流程  
✅ **开发建议** - 代码规范、测试和性能优化建议  

如果您在开发过程中遇到任何问题，欢迎通过 GitHub Issues 或 Discord 社区寻求帮助。

祝您开发顺利！🎮
