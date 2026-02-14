# masscan.exe 使用手册（内含沙箱运行结果，确保无毒，B神编译B神辛苦）🛠️

## 一、工具简介

masscan 是一款高速的异步端口扫描工具，采用独特的传输方式，可在短时间内完成大规模网络扫描。它专为扫描整个互联网或大型网段设计，扫描速度远超传统工具如 nmap，常被用于网络安全评估与渗透测试。

---

## 二、安装与依赖

### 1. 系统要求

- **Windows**：Windows 7/8/10/11 或 Windows Server 2008 R2 及以上版本。

- **依赖组件**：(**建议先运行试一下大部分电脑有相关的npcap和dll**，附带npcap安装包，dll修复工具)
- **Npcap**：网络抓包驱动，必须安装。下载地址：[Npcap 官网](https://nmap.org/npcap/)，安装时勾选“WinPcap API-compatible Mode”。

- **Visual C++ Redistributable**：运行库，确保安装 Visual C++ 2015-2022 Redistributable（x86 + x64）。

### 2. 安装步骤

1. 下载 masscan 可执行文件或通过源码编译。

2. 安装 Npcap 和 VC++ 运行库。

3. 将 masscan.exe 所在目录添加到系统环境变量，或直接在命令行中切换到该目录运行。

---

## 三、基础命令与参数

### 1. 核心语法

```Bash
masscan.exe <目标IP/网段> <端口范围> [选项]
```

### 2. 常用目标与端口参数

| 参数            | 说明                     | 示例                                                        |
| --------------- | ------------------------ | ----------------------------------------------------------- |
| `<目标IP/网段>` | 单个IP、IP范围或CIDR网段 | `192.168.1.1`、`10.0.0.1-10.0.0.255`、`192.168.1.0/24`      |
| `-p <端口>`     | 指定扫描的端口           | `-p 80`、`-p 22,80,443`、`-p 1-1000`、`-p-`（扫描所有端口） |
| `--rate <速率>` | 设置每秒发送的数据包数量 | `--rate 1000`（默认100 pps，内网可适当提高）                |

### 3. 网络与接口参数

| 参数                   | 说明                         | 示例                                                    |
| ---------------------- | ---------------------------- | ------------------------------------------------------- |
| `--router-ip <IP>`     | 手动指定网关IP，跳过ARP探测  | `--router-ip 192.168.1.1`                               |
| `--router-mac <MAC>`   | 手动指定网关MAC地址，绕过ARP | `--router-mac 00-11-22-33-44-55`                        |
| `--source-ip <IP>`     | 指定本机源IP地址             | `--source-ip 192.168.1.100`                             |
| `--interface <网卡名>` | 指定使用的网络接口           | `--interface "以太网"`、`--interface \Device\NPF_{...}` |
| `--list-interfaces`    | 列出所有可用网络接口         | `masscan.exe --list-interfaces`                         |

### 4. 输出与结果参数

| 参数           | 说明               | 示例                    |
| -------------- | ------------------ | ----------------------- |
| `--open`       | 仅输出开放的端口   | `--open`                |
| `-oX <文件名>` | 输出为XML格式      | `-oX scan_results.xml`  |
| `-oG <文件名>` | 输出为grepable格式 | `-oG scan_results.txt`  |
| `-oJ <文件名>` | 输出为JSON格式     | `-oJ scan_results.json` |
| `-v` / `-vv`   | 增加输出详细程度   | `-vv`                   |

---

## 四、典型使用场景

### 1. 快速扫描单个主机

```Bash
masscan.exe 192.168.1.100 -p 80,443,22 --rate 500 --open
```

扫描目标主机的 80、443、22 端口，仅输出开放端口。

### 2. 扫描整个网段

```Bash
masscan.exe 192.168.1.0/24 -p 1-1000 --rate 1000 -oG subnet_scan.txt
```

扫描 [192.168.1.0/24](192.168.1.0/24) 网段的 1-1000 端口，并将结果保存到文件。

### 3. 云主机/复杂网络环境扫描

```Bash
masscan.exe 43.139.58.44 -p- --rate 500 --router-mac 3c-c7-86-3b-4c-1d --source-ip 192.168.27.209 --open
```

在云主机上，手动指定网关MAC和源IP，避免ARP探测失败。

---

## 五、高级技巧

### 1. 排除特定IP

```Bash
masscan.exe 192.168.1.0/24 --exclude 192.168.1.1,192.168.1.2 -p 80
```

排除网段中的特定IP，避免扫描关键设备。

### 2. 从文件读取目标

```Bash
masscan.exe -iL targets.txt -p 80 --rate 1000
```

从 `targets.txt` 文件中读取IP列表进行扫描。

### 3. 调整扫描超时

```Bash
masscan.exe 192.168.1.100 -p 1-1000 --wait 10
```

设置扫描结束后等待10秒，确保所有响应都被接收。

---

## 六、沙箱动态检测报告

### 1. 基本信息

- **分析环境**：Win10(1903 64bit, Office2016)

- **文件路径**：`C:\Users\Administrator\Desktop\masscan.exe`

- **文件大小**：344 KB

- **文件类型**：PE32 executable (console) Intel 80386, for MS Windows, 5 sections

- **引擎检出**：3 / 28

- **威胁分类**：潜在有害应用

- **木马家族**：Masscan

- **HASH值**：

  - SHA256: `2abcb4c0dd7573e81bcca0cfc4970ca8f819187366a44c8eec1da448c3b1419c`

  - MD5: `8f9020b167d5ef57580f37d03a62510a`

  - SHA1: `5d01728c0cfd5bdd1ed1c8a54b01acde1fa5513e`

### 2. 执行流程

- 启动 `masscan.exe` (PID: 6468)

- 未发现主动外连行为

### 3. 行为检测

#### 可疑行为（1条）

- **信息搜集**：获取系统网络适配器信息

#### 通用行为（2条）

- **一般行为**：该可执行文件存在调试数据库文件（PDB）路径

- **静态文件特征**：在文件内存中发现IP地址或URL

### 4. 处置建议

- 该分析环境样本分析结果无处置建议。

---

## 七、常见问题与排查

### 1. 启动类错误

- **`0xc000007b`** ** / ** **`VCRUNTIME140.dll`** ** 丢失**：安装 Visual C++ 2015-2022 Redistributable。

- **`could not find a suitable network interface`**：以管理员身份运行，或使用 `--interface` 指定网卡。

### 2. 网络类错误

- **`ARP timed-out resolving MAC address`**：使用 `--router-ip` 或 `--router-mac` 手动指定网关信息。

- **`found=0`** ** 无扫描结果**：降低 `--rate`，检查目标防火墙/安全组，或使用 nmap 验证连通性。

### 3. 性能与安全建议

- **扫描速率**：内网环境建议 `--rate 100-1000`，互联网扫描建议不超过 `10000`，避免触发安全告警。

- **权限要求**：必须以管理员身份运行，否则无法发送原始数据包。

- **合法性**：仅在获得授权的网络中使用，未经许可的扫描可能违反法律法规。

---

## 八、参考资源

- 官方文档：[masscan GitHub](https://github.com/robertdavidgraham/masscan)

- Npcap 下载：[https://nmap.org/npcap/](https://nmap.org/npcap/)

- masscan源码来源：https://github.com/robertdavidgraham/masscan

### 总结

1. masscan核心依赖Npcap抓包驱动和VC++运行库，Windows Server需手动安装这两个组件才能正常启动。

2. 复杂网络环境（如云主机）需手动指定网关IP/MAC、源IP，避免ARP探测超时等问题。

3. 沙箱检测显示masscan无主动外连行为，仅会搜集网络适配器信息，属于工具正常行为。

