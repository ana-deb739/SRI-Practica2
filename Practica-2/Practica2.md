# Práctica 2: Instalación y configuración de un servidor DHCP en Debian

## Índice

- [Práctica 2: Instalación y configuración de un servidor DHCP en Debian](#práctica-2-instalación-y-configuración-de-un-servidor-dhcp-en-debian)
  - [Índice](#índice)
- [Introduccion](#introduccion)
  - [*Recursos*](#recursos)
  - [*Escenario*](#escenario)
  - [Preparacion Del Entorno](#preparacion-del-entorno)
- [Enunciado](#enunciado)
  
# Introduccion

Vamos a realizar un ejercicio de instalación de conficuración de un servidor DHCP en Debian.
Para ello necesitaremos los siguientes recursos ytendremos el siguiente escenario.

## *Recursos*

- PC con acceso a Internet y paquete ofimático instalado.
- VirtualBox .
- Debian .
- Windows cliente.
- Ubuntu Desktop .
- PfSense .
- Wireshark.

## *Escenario*

- En esta práctica vamos a instalar y configurar un servidor DHCP en Debian con dos clientes, uno Windows y otro Linux.
- Las tres máquinas, el servidor Debian, el cliente con Windows y el cliente Ubuntu estarán en la misma subred privada interna SRI2XX, con dirección de red 10.0.XX.0/24,donde XX son los dos últimos dígitos de tu nombre de usuario del dominio. Las máquinas estarán conectadas a Internet a través de un router pfSense.

    ![ReferenciaEscenario](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/imagenreferencia.PNG)

## Preparacion Del Entorno

Para poder instalar y configurar el servidor de DHCP debemos preparar nuestro entorno para cuando tengamos que empezar a trabajar sea lo más comodo posible. Por eso debemos instalar las máquinas indicadas en el apartado de recursos y configurarlas tal y como indica el apartado de escanario.

Configuracon del Debian

![ConfiguracionDebian](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/conf-debian.PNG)

Configuración del PfSense

![ConfiguracionPfSense](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/ipPfSense.PNG)

Las configuraciones de Windows y Ubuntu son estaticas, ya que son clientes DHCP.

Debemos acordarnos de desactivar el DHCP de router PfSense para poder introducir a mano la configuración de red.

Por último debemos comprobar que la máquina de Debian navega por internet, ya sea con un ping a la ip 8.8.8.8 o un ping a <www.google.es>.

PingDebian-PfSense

![PingDebian-PfSense](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/ping10.0.206.2-PfSense.PNG)

PingPfSense-Debian

![PingPfSense-Debian](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/ping10.0.206.0-debian.PNG)

PingDebian-Google

![PingDebian-Google](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/pingDebian-8.8.8.8.PNG)

También debemos cambiar el nombre del promt para poder identificar más facilmente nuestro trabajo.

Cambio Nombre Windows

![CambioWindows](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/hostnamewindows.PNG)

Cambio Nombre Debian

![CambioDebian](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/hostnamedebian.PNG)

Cambio Nombre Ubuntu

![CambioUbuntu](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/hostnameubuntu.PNG)

# Enunciado

Para comenzar, debemos empezar por la configuración del servidor DHCP.

Para ello debemos instalar el servidor en la máquina correspondiente, en este caso lo instalaremos en la máquina de Debian.

Instalacion del DHCP en Debian

![InstalacionDHCP](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/Instalaciondhcpdebian.PNG)

Después de instalarlo debemos modificar el fichero dhcpf.conf, aunque antes debemos hacer una copia de seguridad por si acaso.

Copia fichero

![Copiafichero](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/copia-seguridad-dhcpd.conf.PNG)

Mofificación fichero

- El servidor repartirá direcciones IP en el rango 10.0.XX.1 – 10.0.XX.100.
- Utilizará la máscara por defecto correspondiente a esa subred.
- Deberá utilizar como puerta de enlace la que corresponda según el  diagrama de red.
- Como servidor DNS preferido se utilizará el del instituto (deberás averiguarlo) y como alternativo el de google.
- Además, se enviará a los clientes el sufijo DNS sriXX.local.
- Para el cliente Ubuntu se le reservará la dirección 10.0.XX.60.
- Respecto a los tiempos de alquiler:
  - El tiempo de alquiler por defecto será de 15 días para todos los equipos.
  - Nunca será superior a 30 días.
  - Nunca será inferior a 1 semana.

![Modificacion](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/dhcpd.conf.PNG)

También podemos reservar una IP, en este caso reservamos la Ip 10.0.206.60 para el cliente de Ubuntu.

Reserva de la Ip 10.0.206.60

![Reserva](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/reserva.60ubuntu.PNG)

Para finalizar debemos reiniciar el servicio.

![ReinicioServicio](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/restart-isc.PNG)

Una vez terminado la instalación y la configuración del servidor DHCP en Debian, debemos dirigir nuestra atención a los clientes.

Primero miramos que los clientes tienen la configuración de red en modo DHCP.

Ubuntu

![ClienteDHCPUbuntu](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/clientedhcpubuntu.PNG)

Windows

![ClienteDHCPWindows](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/clientedhcpwindows.PNG)

Una vez configurado el servidor DHCP deberian haberse administrado ip 

Ubuntu

![ClienteDHCPUbuntu](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/ip-dhcp-ubuntu.PNG)

Como indicamos en la reserva del servidor DHCP el cliente de Ubuntu tiene la IP 10.0.206.60.

Windows

![ClienteDHCPUbuntu](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/ip-dhcp-windows.PNG)

Para comprobar que están bien configurados debemos hacer ping entre todos ellos.

Ubuntu.

Ping Ubuntu-PfSense

![PingUbuntu-PfSense](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/pingubuntu-PfSense.PNG)

Ping Ubuntu-Debian

![PingUbuntu-Debian](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/pingubuntu-Debian.PNG)

Ping Ubuntu-Google

![PingUbuntu-Google](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/pingubuntu-google.PNG)

Windows

Ping Windows-PfSense

![PingWindows-PfSense](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/pingwindows-PfSense.PNG)

Ping Windows-Debian

![PingWindows-Debian](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/pingwindows-Debian.PNG)

Ping Windows-Google

![PingWindows-Google](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/pingwindows-google.PNG)

Para terminar hay que comprobar la funcionalidad del servicio con el comando "journalctl".

![Comprobacion](https://github.com/ana-deb739/SRI-Practica2/blob/master/Practica-2/img/journalctl.PNG)
