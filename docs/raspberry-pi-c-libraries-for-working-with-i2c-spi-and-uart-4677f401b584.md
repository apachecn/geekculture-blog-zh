# Raspberry Pi:用于 I2C、SPI 和 UART 的 C++库

> 原文：<https://medium.com/geekculture/raspberry-pi-c-libraries-for-working-with-i2c-spi-and-uart-4677f401b584?source=collection_archive---------4----------------------->

![](img/5ef172d1d856be79f00b530bcfeb63f3.png)

Raspberry Pi 是一个单板计算机，现在有 4 个版本和一个极简的零版本。它是不同项目的流行选择，因为它的体积小，功耗低，处理速度快，并且是一台完全基于 Linux 的计算机。

连接多个单板计算机和/或微控制器的一种方式是直接布线。为此，最常用的协议是 I2C、SPI 和 UART。博客系列的前几篇文章解释了这些协议的原理，并展示了 Arduino 的特定 C 库。在本文中，我将解释能够在 Raspberry Pi 上使用这些协议的 C++库。对于每个协议，我都研究了可用的库，并给出了简短的解释和代码示例。请注意，这些示例不是我开发的，而是来自于库文档，应该作为具体工作示例的基础。

*本文原载于* [*我的博客 admantium.com*](https://admantium.com/blog/micro07_raspberry_pi_i2c_spi_uart_cpp/)。

# I2C

I2C 可以在 SMBus 协议的帮助下得到支持，SMBus 协议被描述为 I2C 总线的一个特殊变体。该协议可作为 [Linux 内核模块](https://www.kernel.org/doc/Documentation/i2c/smbus-protocol)获得。要使用它，您需要配置您的 Raspberry Pi。在终端中，运行`raspi-config`，选择`3 Interfacing Options`和`P5 I2C`。

按照 kernel.org 的[代码示例，您需要打开代表连接的 I2C 设备的设备文件，然后通过写入设备寄存器来发送 SMBus 命令。](https://www.kernel.org/doc/Documentation/i2c/dev-interface)

```
// SOURCE: https://www.kernel.org/doc/Documentation/i2c/dev-interface */
#include <linux/i2c-dev.h>
#include <i2c/smbus.h>int file;
int adapter_nr = 2;
char filename[20];snprintf(filename, 19, "/dev/i2c-%d", adapter_nr);
file = open(filename, O_RDWR);
if (file < 0) {
  exit(1);
} int addr = 0x40;if (ioctl(file, I2C_SLAVE, addr) < 0) {
  exit(1);
}__u8 reg = 0x10;
__s32 res;
char buf[10];res = i2c_smbus_read_word_data(file, reg);
if (res < 0) {
  /* ERROR HANDLING: i2c transaction failed */
} else {
  /* res contains the read word */
}buf[0] = reg;
buf[1] = 0x43;
buf[2] = 0x65;
if (write(file, buf, 3) != 3) {
  /* ERROR HANDLING: i2c transaction failed */
}
```

# 精力

为了使用 SPI，您还需要添加一个特定的内核模块: [Spidev](https://elixir.bootlin.com/linux/latest/source/drivers/spi/spidev.c) 。树莓 Pi 支持这个模块，你需要通过调用`raspi-config`进行配置，然后选择`3 Interfacing Options`和`P4 SPI`。

要用 C/C++访问 SPI 函数，可以使用 [spidev 包装库](https://github.com/milekium/spidev-lib)。按照[示例代码](https://raw.githubusercontent.com/milekium/spidev-lib/master/sample/spidev-testcpp.cc)，需要配置 SPI 连接，然后打开想要连接的设备，然后使用库方法读写数据。

```
//SOURCE: https://raw.githubusercontent.com/milekium/spidev-lib/master/sample/spidev-testcpp.cc
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <spidev_lib++.h>spi_config_t spi_config;
uint8_t tx_buffer[32];
uint8_t rx_buffer[32];int  main( void)
{ SPI *mySPI = NULL; spi_config.mode=0;
  spi_config.speed=1000000;
  spi_config.delay=0;
  spi_config.bits_per_word=8; mySPI=new SPI("/dev/spidev1.0",&spi_config); if (mySPI->begin())
  {
    memset(tx_buffer,0,32);
    memset(rx_buffer,0,32);
    sprintf((char*)tx_buffer,"hello world");
    printf("sending %s, to spidev2.0 in full duplex \n ",(char*)tx_buffer);
    mySPI->xfer(tx_buffer,strlen((char*)tx_buffer),rx_buffer,strlen((char*)tx_buffer));
    printf("rx_buffer=%s\n",(char *)rx_buffer);
    //mySPI->end();
    delete mySPI;
  }
 return 1;
}
```

# 通用非同步收发传输器(Universal Asynchronous Receiver/Transmitter)

UART 连接可以用普通的 C 库来建立。按照本文中[非常详细的解释，您需要打开设备，然后使用`termios struct`定义 TTY 设备的各种属性，然后写入和读取串口。](https://blog.mbedded.ninja/programming/operating-systems/linux/linux-serial-ports-using-c-cpp/)

```
// SOURCE: https://blog.mbedded.ninja/programming/operating-systems/linux/linux-serial-ports-using-c-cpp/
#include <stdio.h>
#include <string.h>
#include <fcntl.h>
#include <errno.h>
#include <termios.h>
#include <unistd.h>int main() {
  int serial_port = open("/dev/ttyUSB0", O_RDWR); struct termios tty; if(tcgetattr(serial_port, &tty) != 0) {
      printf("Error %i from tcgetattr: %s\n", errno, strerror(errno));
      return 1;
  } tty.c_cflag &= ~PARENB;
  tty.c_cflag &= ~CSTOPB;
  tty.c_cflag &= ~CSIZE;
  tty.c_cflag |= CS8;
  tty.c_cflag &= ~CRTSCTS;
  tty.c_cflag |= CREAD | CLOCAL; tty.c_lflag &= ~ICANON;
  tty.c_lflag &= ~ECHO;
  tty.c_lflag &= ~ECHOE;
  tty.c_lflag &= ~ECHONL;
  tty.c_lflag &= ~ISIG;
  tty.c_iflag &= ~(IXON | IXOFF | IXANY);
  tty.c_iflag &= ~(IGNBRK|BRKINT|PARMRK|ISTRIP|INLCR|IGNCR|ICRNL); tty.c_oflag &= ~OPOST;
  tty.c_oflag &= ~ONLCR; tty.c_cc[VTIME] = 10;
  tty.c_cc[VMIN] = 0; cfsetispeed(&tty, B9600);
  cfsetospeed(&tty, B9600); if (tcsetattr(serial_port, TCSANOW, &tty) != 0) {
      printf("Error %i from tcsetattr: %s\n", errno, strerror(errno));
      return 1;
  } unsigned char msg[] = { 'H', 'e', 'l', 'l', 'o', '\r' };
  write(serial_port, "Hello, world!", sizeof(msg)); char read_buf [256]; memset(&read_buf, '\0', sizeof(read_buf); int num_bytes = read(serial_port, &read_buf, sizeof(read_buf)); if (num_bytes < 0) {
      printf("Error reading: %s", strerror(errno));
      return 1;
  } printf("Read %i bytes. Received message: %s", num_bytes, read_buf); close(serial_port)
  return 0;
```

# 通用 GPIO 访问

库 [libGPIOsd](https://git.kernel.org/pub/scm/libs/libgpiod/libgpiod.git/about/?h=v1.6.x) 提供了对任何运行 Linux 的设备的 gpio 的通用访问。它检测可用的 GPIO，可以向它们读写数据，并等待事件被触发。有了这个，你就可以写 ow 代码来和任何连接的设备进行 UART 对话。

要安装它，请运行以下命令:

```
apt-get install autoconf autoconf-archive libtool libkmod-dev pkg-config
git clone https://github.com/brgl/libgpiod.gitcd libgpiod
./autogen.sh --enable-tools=yes --prefix=/usr/local/bin
make
sudo make install
```

如果编译和安装成功，在子文件夹`./tools`中，你会发现类似于`gpiodetect`和`gpioinfo`的二进制文件，你可以用它们来研究 GPIOs。请参见以下示例。

```
$> ./tools/gpiodetect
gpiochip0 [pinctrl-bcm2711] (58 lines)
gpiochip1 [raspberrypi-exp-gpio] (8 lines)./tools/gpioinfo
gpiochip0 - 58 lines:
 line   0:     "ID_SDA"       unused   input  active-high
 line   1:     "ID_SCL"       unused   input  active-high
 line   2:       "SDA1"       unused   input  active-high
 line   3:       "SCL1"       unused   input  active-high
 line   4:  "GPIO_GCLK"       unused   input  active-high
 line   5:      "GPIO5"       unused   input  active-high
...
```

如果你想使用这个库，阅读[这篇文章](https://www.beyondlogic.org/an-introduction-to-chardev-gpio-and-libgpiod-on-the-raspberry-pi/)获得详细的介绍。

# 结论

对于在 Raspberry Pi 上使用 I2C、SPI 和 UART，不仅可以使用 Python，还可以使用 C++库。具体来说，您需要通过`raspi-config`激活 I2C 和 SPI 函数，它会加载适当的内核模块。然后选择一个客户端库和其他必要的 C++头文件。使用库遵循相同的原则:确定连接的设备文件，配置连接对象，打开设备文件，然后读取/写入设备文件。最后，方便的 libgpiod 库可以帮助您直接访问所有 GPIO 引脚，这对调试很有帮助。