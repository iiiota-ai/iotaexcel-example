# iotaexcel-example

这是一个 `iotaexcel` 示例工程，用于演示如何把 Excel 配置表导出为运行时使用的
`.bytes` 数据文件，并生成对应的 C# 配置读取代码。

当前示例以 Windows 批处理脚本为主要入口，内置了 `all`、`client`、`server`
三种目标导出流程，方便分别生成全量、客户端和服务端配置资料。

## 目录说明

- `bin/`：`iotaexcel` 命令行程序。
- `excels/`：输入的 Excel 配置表，目前示例文件为 `TestConfig.xlsx`。
- `confs/`：`convert` 和 `decode` 命令使用的配置文件。
- `generated/bytes/`：由 Excel 导出的 `.bytes` 数据文件。
- `generated/code/`：生成的 C# 配置读取代码。
- `decoded/`：由 `.bytes` 反解出的 JSON 文件，便于人工检查和调试。
- `log/`：脚本执行日志。

生成目录会按目标拆分：

- `all/`：导出全部配置。
- `client/`：只导出客户端目标配置。
- `server/`：只导出服务端目标配置。

## 快速开始

在 Windows 环境下，直接运行仓库根目录的 `.bat` 脚本即可。

### 导出 bytes

```powershell
.\convert-all-config.bat
.\convert-client-config.bat
.\convert-server-config.bat
```

输出目录：

- `generated/bytes/all/`
- `generated/bytes/client/`
- `generated/bytes/server/`

### 生成 C# 读取代码

```powershell
.\codegen-all.bat
.\codegen-client.bat
.\codegen-server.bat
```

输出目录：

- `generated/code/all/`
- `generated/code/client/`
- `generated/code/server/`

### 反解 bytes 为 JSON

```powershell
.\decode-all-config.bat
.\decode-client-config.bat
.\decode-server-config.bat
```

输出目录：

- `decoded/all/`
- `decoded/client/`
- `decoded/server/`

## 常用流程

修改 `excels/TestConfig.xlsx` 后，推荐按下面顺序重新生成资料：

```powershell
.\convert-all-config.bat
.\convert-client-config.bat
.\convert-server-config.bat
.\codegen-all.bat
.\codegen-client.bat
.\codegen-server.bat
.\decode-all-config.bat
.\decode-client-config.bat
.\decode-server-config.bat
```

如果只关注某一端，也可以只运行对应的 `client` 或 `server` 脚本。

## 配置说明

`confs/` 目录中的配置文件用于控制导出和反解参数，例如：

- `input`：输入目录。
- `output`：输出目录。
- `target`：目标类型，可为 `all`、`client`、`server`。
- `convertFormat`：导出格式，当前为 `bin`。
- `decodeFormat`：反解格式，当前为 `json`。
- `checkRef`：是否检查引用关系。
- `overwrite`：是否覆盖已有文件。
- `logFile`：日志输出文件。

`codegen` 脚本直接在命令行中指定参数，当前生成语言为 C#，并开启引用检查和覆盖输出。

## Excel 表格格式

每个 Excel 工作表都应遵循 iotaexcel 表格格式：

1. 字段名
2. 字段类型
3. 字段目标
4. 字段注释
5. 数据行

字段目标用于区分配置导出到 `client`、`server` 或 `all`。

## 可执行文件说明

仓库使用 `bin/iotaexcel-windows-amd64.exe` 执行 Windows 脚本。如果杀毒软件提示风险，
请先确认文件来源可信，并对比官方发布的 SHA256 校验值。确认误报后，再考虑为当前
目录或该可执行文件添加排除项，不建议关闭整个杀毒软件。

### SHA256 校验

Windows PowerShell:

```powershell
Get-FileHash .\bin\iotaexcel-windows-amd64.exe -Algorithm SHA256
```

将输出中的 `Hash` 值与 `sha256sums.txt` 中对应文件名的校验值进行对比。如果两者
一致，说明文件内容与记录的校验值匹配；如果不一致，请不要继续运行该可执行文件，
应重新获取可信来源的文件。

Linux:

```bash
sha256sum ./bin/iotaexcel-linux-amd64
```

macOS:

```bash
shasum -a 256 ./bin/iotaexcel-darwin-arm64
```

如果使用 Intel 芯片的 macOS 设备，请将文件名改为 `iotaexcel-darwin-amd64`。
