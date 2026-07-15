# iotaexcel-example

这是一个 `iotaexcel` 示例工作区。`iotaexcel` 是一个跨平台命令行工具，
用于将结构化的 Excel 配置表转换为 `.bytes` 数据文件，并生成对应的 C#
读取代码。

## 目录说明

- `excels/`：输入的 Excel 配置文件。
- `bytes/`：生成的 `.bytes` 输出文件。
- `code/`：生成的 C# 读取代码。
- `log/`：可选的命令执行日志。
- `iotaexcel-windows-amd64.exe`：Windows 命令行程序。
- `iotaexcel-linux-amd64`：Linux 命令行程序。
- `iotaexcel-darwin-amd64`：macOS Intel 芯片命令行程序。
- `iotaexcel-darwin-arm64`：macOS Apple Silicon 芯片命令行程序。
- `sha256sums.txt`：可执行文件的校验和。

## 快速开始

Windows:

```powershell
.\iotaexcel-windows-amd64.exe convert --input .\excels --output .\bytes --format bin
.\iotaexcel-windows-amd64.exe codegen --input .\excels --output .\code --lang csharp
```

Linux:

```bash
chmod +x ./iotaexcel-linux-amd64
./iotaexcel-linux-amd64 convert --input ./excels --output ./bytes --format bin
./iotaexcel-linux-amd64 codegen --input ./excels --output ./code --lang csharp
```

macOS:

```bash
chmod +x ./iotaexcel-darwin-arm64
./iotaexcel-darwin-arm64 convert --input ./excels --output ./bytes --format bin
./iotaexcel-darwin-arm64 codegen --input ./excels --output ./code --lang csharp
```

如果使用 Intel 芯片的 macOS 设备，请改用 `iotaexcel-darwin-amd64`。

## 说明

- 每个 Excel 工作表都应遵循 iotaexcel 表格格式：字段名、字段类型、字段目标、
  字段注释，之后是数据行。
- `convert` 用于导出运行时使用的数据文件。
- `codegen` 用于生成 C# 加载代码，以读取导出的 `.bytes` 文件。
- `decode` 可用于查看 `.bytes` 文件，或将其转换回 JSON、CSV 以便调试。
