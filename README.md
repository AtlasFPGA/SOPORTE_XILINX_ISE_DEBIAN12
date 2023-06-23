# SOPORTE_XILINX_ISE_14.7_EN_DEBIAN12

# Estoy trabajando para dar soporte xilinx a ATLAS y estos son los primeros pasos.

Me puse a ver como poder instalar el ISE 14.7 en el nuevo DEBIAN 12, y funciona perfectamente.
Aquí os dejo un código con el script a generar:

```
unset QT_PLUGIN_PATH
XILINX_HOME=/home/jaime/Xilinx
export XILINXD_LICENSE_FILE=/opt/Xilinx/xilinx_ise.lic
export XIL_CSE_PLUGIN_DIR=/home/jaime/.cse
export LD_PRELOAD=/opt/Xilinx/14.7/usb-driver/libusb-driver.so
sh /opt/Xilinx/14.7/ISE_DS/settings64.sh
/opt/Xilinx/14.7/ISE_DS/ISE/bin/lin64/ise
```

La librería libusb-driver la forma más facil de instalarla es:

Instrucciones seguidas del siguiente manual/video paso a paso :
https://www.youtube.com/watch?v=meO-b6Ib17Y

Instalación del cable compatible iMPACT:

1. Ejecuta la siguiente secuencia de comandos en la linea de comandos de Linux:

```
sudo /opt/Xilinx/14.7/ISE_DS/ISE/bin/lin64/install_script/install_drivers/./install_drivers
```

(Esta linea muestra un error code 1, es normal a mi me ha salido y luego me instaló el driver en Debian 12.)

```
cd /opt/Xilinx/14.7/ISE_DS/ISE/bin/lin64/digilent/
sudo ./install_digilent.sh
sudo apt-get install gitk git-gui libusb-dev build-essential libc6-dev-i386 fxload libftdi-dev
cd /opt/Xilinx/14.7
sudo git clone git://git.zerfleddert.de/usb-driver
cd usb-driver
sudo make 
./setup_pcusb /opt/Xilinx/14.7/ISE_DS/ISE/
```

(Usar en la línea de comandos make para crear binarios de 32bit y 64bit.)

Para evitar la advertencia "iMPACT - Module windrvr6 is not loaded".
agregar al fichero ~/.bashrc la siguiente linea:


```
export LD_PRELOAD=/opt/Xilinx/14.7/usb-driver/libusb-driver.so
```

Es mejor añadirla a un script que no sea .bashrc dado que se pueden producir problemas con el Xilinx SDK.
Por ejemplo el guión .sh que mencióne en una anterior entrada, para arrancar ise 14.7.

viewtopic.php?f=25&t=457

Fuente y traducción al Español y limpieza de esta entrada del foro de AMD:

https://support.xilinx.com/s/question/0D52E00006iHlPGSA0/unable-to-program-my-fpga-spartan-6-on-ise-1407-ubuntu-1804?language=en_US

Usar una mejor librería con un amplio soporte de cables Xilinx compatibles o no con su uso en iMPACT:

Modelos de cables soportados:

XILINX Platform Cable USB II
XILINX Platform Cable USB DLC9, DLC9LP and DLC9G
Integrated Platform Cable USB on Spartan 3E starter kit
Integrated Platform Cable USB on Spartan 3A starter kit
Integrated Platform Cable USB on XUP-V2Pro
XILINX Parallel Cable IV (in Parallel Cable III compatibility mode)
FCPU-X platform cable (XILINX Platform Cable USB clone)
Enterpoint Prog2 Parallel Cable III clone
Trenz TE0149-01 Parallel Cable III clone
Digilent JTAG3 Parallel Cable III clone
Amontec JTAGkey-Tiny (experimental)

Para poder compilarla hay que tener las siguientes dependencias de paquetes instaladas

```
apt-get install libusb-dev
```

Descargar el fichero:
usb-driver-HEAD.tar.gz
http://git.zerfleddert.de/cgi-bin/gitwe ... EAD;sf=tgz
Repositorio GIT:
http://git.zerfleddert.de/cgi-bin/gitwe ... ver?a=tree

Clonar el repositorio:
```
git clone git://git.zerfleddert.de/usb-driver
```
Ejecuto el script que llevan los fuentes.

```
sh /home/jaime/fuentes/usb-driver/setup_pcusb
```

Y luego cambio la linea del arrancador de ise por la nueva librería más moderna:

```
#export LD_PRELOAD=/opt/Xilinx/14.7/usb-driver/libusb-driver.so
export LD_PRELOAD=/fuentes/usb-driver/libusb-driver.so
```
