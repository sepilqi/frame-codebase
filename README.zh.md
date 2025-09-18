# Frame 固件与 RTL 代码库

欢迎来到 Frame 硬件的完整代码库。日常使用请查看文档：[这里](https://docs.brilliant.xyz)。

## 系统架构

该代码库分为三个部分：**nRF52 应用程序**、**nRF52 引导程序（Bootloader）** 与 **FPGA RTL**。

nRF52 负责整个系统的总体运行：运行 Lua、处理蓝牙网络、AI 任务以及电源管理。FPGA 则专注于图形与相机的加速。

![Frame 系统架构图](docs/diagrams/frame-system-architecture.drawio.png)

## nRF52 固件开发入门

1. 请确保已安装 [ARM GCC 工具链](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads)。

1. 请确保已安装 [nRF 命令行工具](https://www.nordicsemi.com/Products/Development-tools/nRF-Command-Line-Tools)。

1. 请确保已安装 [nRF Util](https://www.nordicsemi.com/Products/Development-tools/nRF-Util)，并安装 `device` 和 `nrf5sdk-tools` 子命令。

    ```sh
    ./nrfutil install device
    ./nrfutil install nrf5sdk-tools
    ```

1. 克隆本仓库并初始化子模块：

    ```sh
    git clone https://github.com/brilliantlabsAR/frame-codebase.git
    
    cd frame-codebase
    
    git submodule update --init
    ```

1. 在 `frame-codebase` 目录中运行以下命令，可为 [nRF52840 DK](https://www.nordicsemi.com/Products/Development-hardware/nRF52840-DK) 构建并烧录项目：

    ```sh
    make release
    make erase-jlink # 如有需要，解除闪存保护
    make flash-jlink
    ```

### 调试

1. 在 [VSCode](https://code.visualstudio.com) 中打开本项目。

    `.vscode/tasks.json` 已预配置了一些构建任务。通过 `Ctrl-Shift-P`（在 macOS 上为 `Cmd-Shift-P`）→ `Tasks: Run Task` 访问。

    尝试运行 `Build` 任务，项目应可以正常构建。

    在编程或调试前，可能需要先运行 `Erase` 任务以解锁设备。

1. 要启用 IntelliSense，请在 VSCode 中选择正确的编译器：`Ctrl-Shift-P`（macOS 为 `Cmd-Shift-P`）→ `C/C++: Select IntelliSense Configuration` → `Use arm-none-eabi-gcc`。

1. 为启用调试，请安装 VSCode 扩展 [Cortex-Debug](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug)。

1. `.vscode/launch.json` 已配置好调试启动项。在 “运行和调试” 面板中运行 `Application (J-Link)`，或按 `F5`。项目会在启动前自动构建并烧录。

1. 若需查看日志，运行任务 `RTT Console (J-Link)`，并确保 `Application (J-Link)` 调试配置正在运行。

1. 若需使用 [Black Magic Probe](https://black-magic.org/index.html) 进行调试，请参见[此处](/production/blackmagic/README.md)的说明。

## FPGA 开发入门

完整的 FPGA 架构说明见文档：[这里](docs/fpga-architecture.md)。

为方便起见，预编译的 FPGA RTL 已包含在 `fpga_application.h` 中。若希望修改 FPGA RTL，请遵循[这里](docs/fpga-toolchain-setup.md)的说明。
