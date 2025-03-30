# Freqtrade在Windows系统上的安装教程

本教程将指导您在Windows系统上安装Freqtrade加密货币交易机器人。有两种安装方式：自动安装和手动安装。建议新手用户选择自动安装，而希望对安装过程有更多控制的高级用户可以选择手动安装。

## 系统要求

- Windows 10或更高版本
- Python 3.10或更高版本 (Python 3.12也兼容)
- 最低4GB内存（推荐8GB）
- 至少2GB可用磁盘空间

## 方法一：自动安装（推荐）

Freqtrade提供了自动安装脚本，可以简化安装过程。以下是使用自动安装脚本的步骤：

### 1. 下载Freqtrade源码

有两种方法可以获取源码：

#### 方法A：使用Git克隆（推荐）

```powershell
# 安装Git（如果尚未安装）
# 访问 https://git-scm.com/download/win 下载安装程序

# 创建并进入目标目录
mkdir E:\Freqtrade_Binance01
cd E:\Freqtrade_Binance01

# 克隆Freqtrade代码仓库
git clone https://github.com/freqtrade/freqtrade.git .
```

#### 方法B：直接下载ZIP文件

1. 访问 https://github.com/freqtrade/freqtrade/archive/refs/heads/stable.zip 下载最新的稳定版
2. 解压文件到 `E:\Freqtrade_Binance01` 目录

### 2. 运行自动安装脚本

打开PowerShell（建议使用管理员权限），然后运行：

```powershell
cd E:\Freqtrade_Binance01
.\setup.ps1
```

安装脚本会：
1. 检查Python版本，确保兼容
2. 创建Python虚拟环境
3. 安装所有必要的依赖
4. 配置Freqtrade

安装过程中根据提示选择需要的组件。通常，选择默认选项即可满足大多数用户的需求。

### 3. 验证安装

安装完成后，通过运行以下命令验证安装是否成功：

```powershell
# 激活虚拟环境
.\.venv\Scripts\Activate.ps1

# 验证freqtrade版本
freqtrade --version
```

如果显示版本号，说明安装成功。

## 方法二：手动安装

对于希望完全控制安装过程的用户，以下是手动安装Freqtrade的步骤：

### 1. 安装Python

如果尚未安装Python 3.10或更高版本，请从官方网站下载并安装：
https://www.python.org/downloads/windows/

安装时请确保选择"添加Python到PATH"选项。

### 2. 下载Freqtrade源码

使用上述"方法一"的步骤1获取源码。

### 3. 创建并激活虚拟环境

```powershell
cd E:\Freqtrade_Binance01

# 创建虚拟环境
python -m venv .venv

# 激活虚拟环境
.\.venv\Scripts\Activate.ps1
```

### 4. 安装依赖

```powershell
# 升级pip
python -m pip install --upgrade pip

# 安装基本依赖
pip install -r requirements.txt

# 根据需要安装额外的依赖
pip install -r requirements-hyperopt.txt  # 用于优化策略的依赖
pip install -r requirements-plot.txt      # 用于绘图功能的依赖
pip install -r requirements-freqai.txt    # 用于FreqAI功能的依赖
```

### 5. 安装TA-Lib（技术分析库）

TA-Lib是一个用于技术分析的库，安装它可能比较复杂：

1. 从 https://www.lfd.uci.edu/~gohlke/pythonlibs/#ta-lib 下载对应Python版本的wheel文件
   
   例如，对于Python 3.10 (64位)，下载 `TA_Lib‑0.4.24‑cp310‑cp310‑win_amd64.whl`

2. 安装下载的wheel文件：

```powershell
pip install C:\path\to\TA_Lib‑0.4.24‑cp310‑cp310‑win_amd64.whl
```

### 6. 安装Freqtrade

```powershell
# 在开发模式下安装Freqtrade
pip install -e .
```

### 7. 验证安装

```powershell
freqtrade --version
```

## 初始化和配置

安装完成后，您需要初始化配置文件和用户数据目录：

```powershell
# 创建用户配置目录
freqtrade create-userdir --userdir user_data

# 创建配置文件
freqtrade new-config --config config.json
```

## 使用已修改的扩展版本Freqtrade

如果您正在使用包含币安K线数据扩展的"Freqtrade_Binance01"版本，已经修改了以下源文件：

1. `freqtrade/constants.py` - 扩展了默认数据框列定义
2. `freqtrade/optimize/backtesting.py` - 修改了回测索引和表头
3. `freqtrade/data/converter/converter.py` - 修改了数据转换函数
4. `freqtrade/exchange/binance_public_data.py` - 修改了币安公共数据下载
5. `freqtrade/exchange/exchange.py` - 修改了K线获取函数
6. `freqtrade/data/dataprovider.py` - 添加了扩展数据访问方法

这些修改使Freqtrade能够支持完整的币安K线数据字段，包括收盘时间、成交额、成交笔数、主动买入成交量和主动买入成交额。

详细的修改信息，请参阅项目中的`项目数据源码修改说明.md`文件。

## 常见问题解决

### 1. 虚拟环境激活失败

如果遇到"无法加载文件，因为在此系统上禁止运行脚本"错误，请以管理员身份运行PowerShell并执行：

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### 2. TA-Lib安装问题

如果无法安装TA-Lib，可以：

1. 确保下载了正确的wheel文件（对应您的Python版本和系统架构）
2. 尝试使用预编译的二进制版本：`pip install ta-lib-bin`

### 3. 依赖冲突

如果遇到依赖冲突，可以尝试创建一个新的虚拟环境并重新安装：

```powershell
deactivate  # 退出当前虚拟环境
rm -r .venv  # 删除当前虚拟环境
python -m venv .venv  # 创建新的虚拟环境
.\.venv\Scripts\Activate.ps1  # 激活新虚拟环境
# 重新安装依赖
```

## 下一步

安装完成后，您可以：

1. **下载历史数据**：
   ```powershell
   freqtrade download-data --exchange binance --pairs BTC/USDT ETH/USDT --timeframes 1h 4h 1d
   ```

2. **创建和测试策略**：
   ```powershell
   freqtrade new-strategy --strategy MyAwesomeStrategy
   freqtrade backtesting --strategy MyAwesomeStrategy --timeframe 1h
   ```

3. **运行模拟交易**：
   ```powershell
   freqtrade trade --strategy MyAwesomeStrategy --config config.json --dry-run
   ```

4. **启动实盘交易**：
   ```powershell
   freqtrade trade --strategy MyAwesomeStrategy --config config.json
   ```

## 参考资源

- [Freqtrade官方文档](https://www.freqtrade.io/en/stable/)
- [Freqtrade GitHub仓库](https://github.com/freqtrade/freqtrade)
- [Freqtrade社区Discord](https://discord.gg/p7nuUNVfP7)

希望本教程对您有所帮助！如有疑问，请参考官方文档或在社区中寻求帮助。 