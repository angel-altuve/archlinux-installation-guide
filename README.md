# archlinux installation guide 

# # first steps 

Internet connection:

```bash
ip link
```

Executing this command will show you various interfaces, i.e. devices, to connect to the Internet. Among them there should be one called “wlan0”.

Next we will have to activate this device to be able to use it, to activate it we execute the following command:

***root@archiso ~ #***

```bash
ip link set wlan0 up
```

If the device has been successfully activated then we can proceed. If it is not activated, it means that there was an error trying to activate the device.

To check if the device has been turned on, we execute the previous command again:

***root@archiso ~ #***

```bash
ip link
```

Remember that in my case it is “**wlan0**“, but it doesn't always have to be like that, that's why we have run **ip link** to check it, and you may have to adapt the commands according to the name of your device.

When doing so, it should appear next to the interface, that is, together with the device **“wlan0”**, the word **“UP”**.

Once we have the device turned on we will have to scan the Wi-Fi networks that we have nearby and connect to one of them.

To perform a scan of Wi-Fi networks we will have to execute the following command:

***root@archiso ~ #***
```bash
iwlist wlan0 scan
```

Este comando mostrará una lista completa con todas las redes Wi-Fi que tenemos cercanas, así como el nombre de cada una de ellas.

El nombre de la red Wi-Fi juntamente con la contraseña es lo que tendremos que utilizar a continuación para conectarnos.

[https://lh5.googleusercontent.com/d9Uf0OuYypeVEgjfFlxQ4Q6ZXgO9YmETnpIPqLc2n1neS5c07uydLz-Us746rwmvboFsI0ZMeDw5lD4DbFERaB31Yu8xmhptVrVGLaJj4xuuASiWtLnOwkSkjCoyFBnu58m3K68JwQziCxpVyA](https://lh5.googleusercontent.com/d9Uf0OuYypeVEgjfFlxQ4Q6ZXgO9YmETnpIPqLc2n1neS5c07uydLz-Us746rwmvboFsI0ZMeDw5lD4DbFERaB31Yu8xmhptVrVGLaJj4xuuASiWtLnOwkSkjCoyFBnu58m3K68JwQziCxpVyA)

## Conectarse a una red Wi-Fi con contraseña WPA

Si la red Wi-Fi a la cual nos queremos conectar utiliza protocolo de encriptación WPA o WPA2, entonces tendremos que ejecutar dos comandos.

En mi caso, como habéis podido observar en la imagen anterior, mi red Wifi (PerezWifi) con encriptación WPA, y por ende, es el método que tendré que utilizar.

[https://lh4.googleusercontent.com/OHFyyMPjpoRUFwr8Y6Huhby-vyHbCN-2GaH0LTdWMh7zC99Wdh8tyhQq_Bw7d0NAwgOdu2Xy-yBOjIQt6S8IORqlQxioh4V6277sQg4puQ93ZtT6-e4GpEB1ps_sfT03meT13hh7nLppz65rpw](https://lh4.googleusercontent.com/OHFyyMPjpoRUFwr8Y6Huhby-vyHbCN-2GaH0LTdWMh7zC99Wdh8tyhQq_Bw7d0NAwgOdu2Xy-yBOjIQt6S8IORqlQxioh4V6277sQg4puQ93ZtT6-e4GpEB1ps_sfT03meT13hh7nLppz65rpw)

El primer comando es para configurar la contraseña que vamos a utilizar y el ESSID, y la guardamos en un archivo de configuración, por ejemplo **“/etc/wifi”**:

```bash
wpa_passphrase PerezWifi Contraseña > /etc/wifi
```

El fichero **«/etc/wifi»** que se ha generado podéis visualizarlo con el comando:

```bash
cat /etc/wifi
```

A continuación tendremos que conectarnos a esta red Wi-Fi utilizando el fichero de configuración que hemos generado, para ello tendremos que ejecutar el siguiente comando:

```bash
wpa_supplicant -B -i wlan0 -D wext -c /etc/wifi
```

El comando «**wpa_supplicant**» sirve para conectarnos a una red Wifi que utilice **WPA/WPA2** como método de encriptación.

[https://lh5.googleusercontent.com/0Gc-H8QitBQK_oRnUTQtnu46G26ABUO5pQ9CPlivrInW6uXWKrLvXbZzI-8UWaa0lwVQ1BM15IhNgbQExCzz0IuR23uoiNJ6qGrQ5f_74lgc89HoB5Gz0TEXJ758tk6P-Mj9qd8OW50Rmlc70A](https://lh5.googleusercontent.com/0Gc-H8QitBQK_oRnUTQtnu46G26ABUO5pQ9CPlivrInW6uXWKrLvXbZzI-8UWaa0lwVQ1BM15IhNgbQExCzz0IuR23uoiNJ6qGrQ5f_74lgc89HoB5Gz0TEXJ758tk6P-Mj9qd8OW50Rmlc70A)

El argumento **-B** indica que el comando será ejecutado en segundo plano. Esto evita que aparezca el «log» en la línea de comandos.

El argumento **-i** sirve para especificar el nombre de la interfaz wifi, en este caso, **«wlan0»**.

El argumento **-D** indica el «driver» o controlador a utilizar, en este caso, utilizo **«wext»**.

Finalmente, el argumento **-c** sirve para indicar el archivo de configuración a utilizar, en este caso, **«/etc/wifi»**, el cual hemos generado con el comando anterior: **wpa_passphrase**.

## **Finalizar conexión Wi-Fi**

Una vez hayamos realizado la conexión, según el tipo de contraseña de la red Wi-Fi, tendremos que finalizar la conexión y establecer una negociación IP. Esto puede parecer muy complicado, pero realmente no lo es, pues sólo tenemos que ejecutar un único comando:

```bash
dhclient
```

El comando **dhclient** sirve para realizar la negociación **IP**, es decir, para obtener una dirección **IP** local y una dirección **DNS** automáticamente. Sin este comando, aun estando conectados a la red wifi, no tendríamos conexión a Internet.

[https://lh6.googleusercontent.com/vwhsc4XwQydsc6sXqcNwRtqr3UFGBiu9frU0tAMjJArTlkWDsZKVkda8okyVxhAd85E8K9-bWVlYlXNu3Prz5xWZnMWLXZxFsWM6ofh04IB08vvmwGqibEGrF4bhfcoL8pl2gAHW0_OtcduBqg](https://lh6.googleusercontent.com/vwhsc4XwQydsc6sXqcNwRtqr3UFGBiu9frU0tAMjJArTlkWDsZKVkda8okyVxhAd85E8K9-bWVlYlXNu3Prz5xWZnMWLXZxFsWM6ofh04IB08vvmwGqibEGrF4bhfcoL8pl2gAHW0_OtcduBqg)

Hecho esto, ya tendríamos conexión vía Wi-Fi,  lo cuál podemos comprobar de igual manera que si utilizáramos cable con el comando “**ping**”:

root@archiso ~ # ping google.com

## **Particionar disco:**

****Para consultar la tabla de particiones que tiene su disco duro donde va a instalar el Sistema Operativo use el siguiente comando.

```bash
fdisk -l
```

[https://lh3.googleusercontent.com/RS-KZuWN8-wormEHv1-UKqLVeZJsGHzgDKDHUfXkH9e8y11e-CAKj517GAYdhHZ7faetCfxcJHD3iljTUqU5qoLedTb9bF8xy60l_XK-rPZhjrKHub2dglV5zEPK8IGzNFzvN4iCQHvQtDgDZI0](https://lh3.googleusercontent.com/RS-KZuWN8-wormEHv1-UKqLVeZJsGHzgDKDHUfXkH9e8y11e-CAKj517GAYdhHZ7faetCfxcJHD3iljTUqU5qoLedTb9bF8xy60l_XK-rPZhjrKHub2dglV5zEPK8IGzNFzvN4iCQHvQtDgDZI0)

## **Disco Duro**

Primero debemos identificar como es la ruta de nuestros discos luego saber cuales son particiones.

Ruta de disco pueden ser:

/dev/sda (sdd o hdd)

/dev/sdb (sdd o hdd)

/dev/sdc (Así cambia de letra...)

/dev/nvme0n1 (veMMC o SD Card)

/dev/mmcblk0 (veMMC o SD Card)

## lsblk

### **Crear las particiones**

**>> Arch Linux incluye las siguientes herramientas de particionado:**

## 

[https://lh3.googleusercontent.com/Ijrd4_eCuhLrcA0btmeSj5_06ZkN6XH1vKc_4xmS6EfBirJ4lRsBhI2Z3PGljpZqfEwtzmSx5_2Tx341Ixp_4YCet7Z5E7oD0neJEazV17iraemrJ6Ibo6lmJRExuDl4D-35lITfLA_EODhf_NM](https://lh3.googleusercontent.com/Ijrd4_eCuhLrcA0btmeSj5_06ZkN6XH1vKc_4xmS6EfBirJ4lRsBhI2Z3PGljpZqfEwtzmSx5_2Tx341Ixp_4YCet7Z5E7oD0neJEazV17iraemrJ6Ibo6lmJRExuDl4D-35lITfLA_EODhf_NM)

**>> Estructura de ficheros en Linux**

[https://lh5.googleusercontent.com/oZH_qnVuMST5wpGpqnNChYASeuRQ-6zviiNyYhzAduzk3KDt-zK-XFxqTdRoKkA9crN-_MCsPvMXNVNuTsrkT3KgpxNwporMaO8sEn05sDh_j81UayCjpf91SKysz7ZaKH6tW3ZhEbmZpIrgbp0](https://lh5.googleusercontent.com/oZH_qnVuMST5wpGpqnNChYASeuRQ-6zviiNyYhzAduzk3KDt-zK-XFxqTdRoKkA9crN-_MCsPvMXNVNuTsrkT3KgpxNwporMaO8sEn05sDh_j81UayCjpf91SKysz7ZaKH6tW3ZhEbmZpIrgbp0)

Debemos saber que de **root** nacen las demás carpetas

La ventaja que **Linux permite asignar una partición para cada carpeta**

Podemos tener **/boot/** en una partición

Podemos tener **/home/** en otra partición

***/home/** la carpeta HOME guarda los archivos de todos los usuarios algo parecido a un DISCO D: de windows*

*Todo depende si queremos tenerlo separado en otra partición*

Lo cual puede ser bueno o malo, depende de las necesidades del usuario

**Ejemplos de particiones:**

[https://lh3.googleusercontent.com/ZxOR0wz0-ch7AF828zYaLhYyIHnr6J1QdNOFCC13VZbbBsKqsiviPikqtzaw6URccyUpN4BgIiki_wKzEr59rUqGsEOkK6YV_nWIfV0Q3LfHplB03uBykzatqB_3zeI9uQuZ5RKK2UqJZfoL-tg](https://lh3.googleusercontent.com/ZxOR0wz0-ch7AF828zYaLhYyIHnr6J1QdNOFCC13VZbbBsKqsiviPikqtzaw6URccyUpN4BgIiki_wKzEr59rUqGsEOkK6YV_nWIfV0Q3LfHplB03uBykzatqB_3zeI9uQuZ5RKK2UqJZfoL-tg)

[https://lh6.googleusercontent.com/8mWLWWSASUlNuUI84xdQ-ZtoXdlZEzpTHvoHZYVmSlwfbKA9n6C5LXsTQyYLNeESIbLGN9mBXlVxw9TyYE9WpXYJdzgJcLfhb9CD1EjzAFKbTheI9wsgEQma4i04ekQ-HWrUd2uSnDxEUJ6st_4](https://lh6.googleusercontent.com/8mWLWWSASUlNuUI84xdQ-ZtoXdlZEzpTHvoHZYVmSlwfbKA9n6C5LXsTQyYLNeESIbLGN9mBXlVxw9TyYE9WpXYJdzgJcLfhb9CD1EjzAFKbTheI9wsgEQma4i04ekQ-HWrUd2uSnDxEUJ6st_4)

[https://lh6.googleusercontent.com/znHoqgKrF6nSReDVB3WjJbKXF9sX0Y-qixe8URIzo5KlxuuTi0uZE4e5rf3ZXICF3H6enx9Tmhn9C7nMnnGo0uO-IIc4oRLlCwnGAartQ_fmWsvLiesb3T9-57xhPRqdeXN3jJj_igKVNZDqc2Y](https://lh6.googleusercontent.com/znHoqgKrF6nSReDVB3WjJbKXF9sX0Y-qixe8URIzo5KlxuuTi0uZE4e5rf3ZXICF3H6enx9Tmhn9C7nMnnGo0uO-IIc4oRLlCwnGAartQ_fmWsvLiesb3T9-57xhPRqdeXN3jJj_igKVNZDqc2Y)

[https://lh6.googleusercontent.com/ti-1vERW0La1aXp1zsU52Dbph9lHhj9gN85b5A-6JDUbinLvhlMml65MVStAhQaUZ_nCxE-xhKhOiEh0BjB0TtvTozvw2g1-DjRrMoH53ddFk0ANO1Z3WFfHlEr_MW-yydNdFL41OBq7Scut_qw](https://lh6.googleusercontent.com/ti-1vERW0La1aXp1zsU52Dbph9lHhj9gN85b5A-6JDUbinLvhlMml65MVStAhQaUZ_nCxE-xhKhOiEh0BjB0TtvTozvw2g1-DjRrMoH53ddFk0ANO1Z3WFfHlEr_MW-yydNdFL41OBq7Scut_qw)

## **>> Memoria SWAP**

Menos de 1GB RAM física >> 2GB de SWAP

Entre 2GB a 4GB RAM física >> 2GB a 4GB de SWAP

8GB de RAM física >> 4GB de SWAP

Más de 8GB de RAM física >> 2GB a 4GB de SWAP

[https://lh4.googleusercontent.com/0oHz1IaBZ4Viun6CLfEQ1cDC9ovgxwS_smchg9eo1Vu_PH1SdJeXrPY8FfqvAXF6mIhm8hIDAdRlJzyBmMlSQ0NQGjH0nsfpLXBr0iRGioZnkAH8_veDTEnMI8NKQUKZSSh40dQADHuf3jbgHUQ](https://lh4.googleusercontent.com/0oHz1IaBZ4Viun6CLfEQ1cDC9ovgxwS_smchg9eo1Vu_PH1SdJeXrPY8FfqvAXF6mIhm8hIDAdRlJzyBmMlSQ0NQGjH0nsfpLXBr0iRGioZnkAH8_veDTEnMI8NKQUKZSSh40dQADHuf3jbgHUQ)

Entendido estos conceptos ejecutamos **cfdisk**

[https://lh4.googleusercontent.com/ADdEn9fjcrW1Yd88n0qvsZDTItWdodrZoZlAealyqa8Nzxv6UAME4-gHO486Heo2mTAEV0bC5ccvmPeqob2g38Sk7kl27CDxAgJM7scQ-3MIJY9EwMmjV0jqIw80umMkvb-L6uN8q2KELYU_cCk](https://lh4.googleusercontent.com/ADdEn9fjcrW1Yd88n0qvsZDTItWdodrZoZlAealyqa8Nzxv6UAME4-gHO486Heo2mTAEV0bC5ccvmPeqob2g38Sk7kl27CDxAgJM7scQ-3MIJY9EwMmjV0jqIw80umMkvb-L6uN8q2KELYU_cCk)

```bash
 cfdisk /dev/sda
```

/*Vamos empezar eliminando las particiones en **[ Delete ]** y creando nuevas en **[ New ]**

/*Seleccionamos **[ Bootable ]** donde esta la partición de arranque

/*Podemos crear varias particiones y solo se generarán: **sda1, sda2, sda3...**

/*Podemos crear las particiones y cambiar el tipo de partición en **[ Type ]**

/***Flecha Arriba / Abajo - Flechas Derecha / Izquierda** - para movernos en cfdisk

**En este ejemplo usaremos:**

Sda1 > boot

Sda2 > root - home

Sda3 > Swap

/*Al finalizar el particionado le damos en **[ Write ]** para escribir los cambios

[https://lh5.googleusercontent.com/gvc_7LNDe9-GZwsQlzQGlwTqXFp33L-HQHm9EHrWIjVznpyIIRRnb3hP-FbJGuYx0tLTZ5gfx3WC4FNiF8KgZgog-e0vylh62BRCWEsuiCPKfJQr7ems9B0rzJH4VDc602kzaCCOjRq0xKHe5ts](https://lh5.googleusercontent.com/gvc_7LNDe9-GZwsQlzQGlwTqXFp33L-HQHm9EHrWIjVznpyIIRRnb3hP-FbJGuYx0tLTZ5gfx3WC4FNiF8KgZgog-e0vylh62BRCWEsuiCPKfJQr7ems9B0rzJH4VDc602kzaCCOjRq0xKHe5ts)

## **Formatear las particiones**

Existen múltiples formatos de particiones disponibles para usar, sin embargo en esta guía usaremos ext4

Ext4 es el más usado y recomendado.

Ext4 es un sistema de archivos transaccional (en inglés journaling) de Linux

Formateo de la Partición de arranque:

```bash
mkfs.ext4 /dev/sda1
```

Formateo de la Partición de Root y Home en una sola partición:

```bash
mkfs.ext4 /dev/sda2
```

Partición de memoria virtual o memoria de intercambio SWAP:

```bash
mkswap /dev/sda3
```

Activar partición SWAP:

```bash
swapon /dev/sda3
```

### **Montar las particiones**

Ahora para root en este ejemplo es ***/dev/sda2*** lo cual debe ser montado primero ya que todo inicia con ***ROOT /***

```bash
mount /dev/sda2 /mnt/
```

Para montar las particiones necesitamos crear antes las rutas lógicas de montaje.

Si va a instalar Archlinux como único sistema en modo BIOS/Legacy, entonces necesitamos crear ***/mnt*** y dentro de este directorio incluir ***/boot*** o ***/home*** o ***/tmp*** o etc...

En este caso solo tenemos ***/boot*** en /dev/sda1

```bash
mkdir -p /mnt/boot/
```

Montando la partición de Arranque ***/boot***

```bash
mount /dev/sda1 /mnt/boot
```

Verificamos que se hayan creado correctamente los directorios

```bash
ls /mnt/
```

> /mnt - sistemas de archivos montados manualmente en el disco duro.

> / - diagonal invertida significa root

### **Generador de Mirrorlist dentro LiveCD**

Ahora montadas las particiones instalamos los programas base y lo más esencial posible.

Pero hay muchos casos que a los pacstrap les resulta una descarga muy lenta y eso se debe al no tener los mirrors más rápidos de descarga.

Para tener los Mirrors más rápidos para tener mejores descargas usaremos ***reflector***

```bash
pacman -Sy reflector python --noconfirm
```

Para ejecutar reflector y tener los mejores Mirrors Servers es:

```bash
reflector --verbose --latest 15 --sort rate --save /etc/pacman.d/mirrorlist
```

Para revisar la lista de Mirrors Servers y confirmar que lo hizo reflector el comando es:

Confirmamos comparando donde dice ***by Reflector***

```bash
cat /etc/pacman.d/mirrorlist
```

[https://lh4.googleusercontent.com/NcDrgfFuzjZS-U53mLdKXr2nm77EsXMsoyVsqVzdpDvz988AQqjpY551s490IXSk8ETMmUHlVGYf3_ma2lUYcV-JgiWL3dt5cpj3Pq7KCfd09wPVxYb71hoCcpqNqKGC1LKpB9YpdS_HobwPF6A](https://lh4.googleusercontent.com/NcDrgfFuzjZS-U53mLdKXr2nm77EsXMsoyVsqVzdpDvz988AQqjpY551s490IXSk8ETMmUHlVGYf3_ma2lUYcV-JgiWL3dt5cpj3Pq7KCfd09wPVxYb71hoCcpqNqKGC1LKpB9YpdS_HobwPF6A)

## **Instalación del sistema**

**Especificación de los paquetes necesarios.**

```bash
root@archiso ~ # pacstrap /mnt base base-devel nano
```

/* **Instalación base - base-devel**: programas, configuraciones, directorios, etc...

/* **Editor de texto en terminal**: Nano

```bash
pacstrap /mnt linux-firmware linux linux-headers mkinitcpio
```

/* **linux-firmware**: Binarios de controladores generalmente propietarios, Las tarjetas gráficas modernas de AMD, NVIDIA y Intel Wi-Fi requieren la carga de blobs para que el hardware funcione correctamente.

/* **linux**: kernel en su versión estable

/* **linux-headers**: cabeceras del kernel en su versión estable

/* **mkinitcpio**: Utilidad de creación de imágenes initramfs para el kernel

Después de la instalación del Kernel, Automático se crearán las IMG de Linux en la carpeta de ***/boot/*** gracias a ***mkinitcpio***

**Especificación de los paquetes necesarios.**

Los siguientes paquetes nos permiten gestionar las conexiones a Internet

```bash
pacstrap /mnt dhcpcd networkmanager net-tools wpa_supplicant dialog netctl
```
En el caso que uses una laptop, para el Bluetooth: (opcional)

```bash
pacstrap /mnt bluez bluez-utils pulseaudio-bluetooth
```

Si sale un error al usar pacstrap del tipo "error: could not open file /mnt/var/lib/pacman/sync/core.db: Unrecognized archive format" tenemos que eliminar el directorio "...sync/" de forma recursiva para solucionarlo.

```bash
rm -R /mnt/var/lib/pacman/sync/
```

### **El archivo fstab**

Es usado para definir cómo las particiones,

Estas definiciones se montaron de forma dinámica en el arranque

[https://lh6.googleusercontent.com/fxdMgFWWurNbZeRE1JrAWDQ_Qyd1QwDs9I8EQOOjAGZryHNvmnqyg06kILg3_JOta97jmaeOzqWVfCE-pYzvS0fOf8eUhgQkmNvoDSkGPeSDnLERA0sPeBZWVwnd2DQhLzIMGZ40vlTN-oimqeM](https://lh6.googleusercontent.com/fxdMgFWWurNbZeRE1JrAWDQ_Qyd1QwDs9I8EQOOjAGZryHNvmnqyg06kILg3_JOta97jmaeOzqWVfCE-pYzvS0fOf8eUhgQkmNvoDSkGPeSDnLERA0sPeBZWVwnd2DQhLzIMGZ40vlTN-oimqeM)

```bash
genfstab -p /mnt >> /mnt/etc/fstab
```

**/*Luego de generar el archivo fstab para las etiquetas de nuestras particiones**

**/*Revisamos con:**

```bash
cat /mnt/etc/fstab
```

[https://lh3.googleusercontent.com/29y8nSdYfJjhy8hP-_0b6brwLKsaXKCD-0sdM7BX3lN47BgKRHFYlnJ_9qaoHRcoEwE08ybKgWbkDmK_Q5bGZtgnVH3DtNZpOtIyPozf8Ulb4s5qzBAYRDj9tQ-MovwEC8SoDQxShZL97dqi3vA](https://lh3.googleusercontent.com/29y8nSdYfJjhy8hP-_0b6brwLKsaXKCD-0sdM7BX3lN47BgKRHFYlnJ_9qaoHRcoEwE08ybKgWbkDmK_Q5bGZtgnVH3DtNZpOtIyPozf8Ulb4s5qzBAYRDj9tQ-MovwEC8SoDQxShZL97dqi3vA)

## **Chroot**

Entramos a la raíz del nuevo sistema como usuario root.

```bash
arch-chroot /mnt
```

/* Entramos a raíz como root

/* Vemos cambio el color y cambio el **~** del livecd por **/** que significa root.

/* Dentro de nuestro sistema vamos a configurar idioma, teclado, hora y usuarios.

```bash
nano /etc/locale.gen
```

/*Quitamos el ***#*** que es comentario en nuestro idioma >> ***es_*** y nuestro país

/*En mi caso es Venezuela pero siempre coloco ingles en > US

/*Debe terminar en ***es_[país].UTF-8 UTF-8***

[https://lh3.googleusercontent.com/heMu03yoZTIU4LUUaqh9Gj8FLLqlMEzNGZMbxU2UgnA8phEPEzvZVYnw_JFJm3Qzg5XRL4ql0IkU1wajMakENxfFEtUy6ds_JFR6vld0Ig3rVnfu8cXY_H3McI80Wsb4TEgOb8Eyk_-Lb4bs6nY](https://lh3.googleusercontent.com/heMu03yoZTIU4LUUaqh9Gj8FLLqlMEzNGZMbxU2UgnA8phEPEzvZVYnw_JFJm3Qzg5XRL4ql0IkU1wajMakENxfFEtUy6ds_JFR6vld0Ig3rVnfu8cXY_H3McI80Wsb4TEgOb8Eyk_-Lb4bs6nY)

Ctrl + W para buscar palabras en nano

Ctrl + O para guardar en nano

Ctrl + X para cerrar en nano

Generamos el idioma seleccionado

```bash
locale-gen
```

establezca la variable LANG en locale.conf

Mi caso es **es_PE.UTF-8**

```bash
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

Exporte la variable LANG con el local especificado

```bash
export LANG=es_PE.UTF-8
```

[https://lh6.googleusercontent.com/7B7MFnqVHZL2pwWxfQKg2he5t0oDM6VE7Qga9bipKExT-QDNMD3ZQq101FxLdp-Dm_U-kJG4gwmfLWFtCVg58_5Z4I-7VSAaxKGX7SkVwr-Fc4Jgc4fBfEiXCz3ZRHZjBr02NAHIZiZSEgk2GVE](https://lh6.googleusercontent.com/7B7MFnqVHZL2pwWxfQKg2he5t0oDM6VE7Qga9bipKExT-QDNMD3ZQq101FxLdp-Dm_U-kJG4gwmfLWFtCVg58_5Z4I-7VSAaxKGX7SkVwr-Fc4Jgc4fBfEiXCz3ZRHZjBr02NAHIZiZSEgk2GVE)

```bash
ls /usr/share/zoneinfo/America/
```

```bash
ls /usr/share/zoneinfo/Europe/
```

/* **ln -sf** - Genera un enlace simbólico es un acceso al fichero

/* Mi caso es de Venezuela y la ciudad es Maracaibo (America/Caracas).

```bash
ln -sf /usr/share/zoneinfo/America/Caracas /etc/localtime
```

[https://lh4.googleusercontent.com/GuCqyNB03epL7jVgcgEdScwdpIWmylkbFdFVfN6tJBDX6aStf4zh9MUAufrWM8JvFfG0hW5phqnD5_Ymjl98y5_XlrwZ-YwJfkv-awulehrCl1T6L8hdarbAKeRRewxsZFew8LqBZkHsUatgQKw](https://lh4.googleusercontent.com/GuCqyNB03epL7jVgcgEdScwdpIWmylkbFdFVfN6tJBDX6aStf4zh9MUAufrWM8JvFfG0hW5phqnD5_Ymjl98y5_XlrwZ-YwJfkv-awulehrCl1T6L8hdarbAKeRRewxsZFew8LqBZkHsUatgQKw)

## **Configuración del Usuario**

El sistema está configurado para leer el reloj interno del equipo, después el reloj del sistema

Estable el RTC a partir de la hora del sistema.

```bash
hwclock -w
```

Defina la distribución de teclado en vconsole.conf

para que permanezca en cada reinicio solo aplica para la terminal virtual.

para que permanezca activo en tu Escritorio (DE) es otro comando.

```bash
echo KEYMAP=es > /etc/vconsole.conf
```

Nombre del equipo, esto no es **USUARIO!**

En **nombre_de_pc** le cambian por uno a su gusto, **más adelante crearán un usuario**

```bash
echo nombre_de_pc > /etc/hostname
```

[https://lh5.googleusercontent.com/IOPCiIxwzUeEfMukqmooLEWeyoErOOTQwEJJcB0GlzW9qWox0s9tDN2HaqBgDLWX2lV6GCFabJRDS5k4qOgvteAK1nJ8AHO7j3W6Z7VU2KRC5zxh6Rf_U_3YRZN-gRiBr1r0ZDWVPosSYwWh7n8](https://lh5.googleusercontent.com/IOPCiIxwzUeEfMukqmooLEWeyoErOOTQwEJJcB0GlzW9qWox0s9tDN2HaqBgDLWX2lV6GCFabJRDS5k4qOgvteAK1nJ8AHO7j3W6Z7VU2KRC5zxh6Rf_U_3YRZN-gRiBr1r0ZDWVPosSYwWh7n8)

Modificamos el archivo **Hosts**

Es importante saber que nombre pusieron en **Hostname** porque aquí sera usado

```bash
nano /etc/hosts
127.0.0.1 [tab] localhost
::1 [tab] localhost
127.0.1.1 [tab] nombre_de_pc.localdomain(Espacio)nombre_de_pc
```

[https://lh3.googleusercontent.com/f2mmftUqEjKKQqTpcyN16QU90771s1_K9a85fcQ7gui98qQAOW3baVsv_UquD3zk3kXAunjBQZPBPFkNddTshMVkGxnT4_lAnR38uWltHMdAD7ZoVqSLFXXWCPRPgvBtYeoTvCEHy-mzKbmT-Go](https://lh3.googleusercontent.com/f2mmftUqEjKKQqTpcyN16QU90771s1_K9a85fcQ7gui98qQAOW3baVsv_UquD3zk3kXAunjBQZPBPFkNddTshMVkGxnT4_lAnR38uWltHMdAD7ZoVqSLFXXWCPRPgvBtYeoTvCEHy-mzKbmT-Go)

Ctrl + O para guardar en nano

Ctrl + X para cerrar en nano

### **USUARIOS**

Contraseña para root:

```bash
passwd root
```

Creamos nuestro usuario, para entrar a nuestro sistema.

```bash
useradd -m -g users -G wheel -s /bin/bash nombre_de_usuario
```

```bash
passwd nombre_de_usuario
```

En nombre_de_usuario, lo cambiamos a nuestro gusto.

[https://lh6.googleusercontent.com/opadNTnhv5xZTkvzztxTjhwD0qlMAZIWbHWRzoR27U2KkN3pobvAEZxxJETOEPjAtsirfP23MlPjY5woD0K0_aSvSNjudoyiUR7I8B_TpoOK2B-OFv7L78sQ04dZQqbAqvpEeAGCk7xwQAtT9hc](https://lh6.googleusercontent.com/opadNTnhv5xZTkvzztxTjhwD0qlMAZIWbHWRzoR27U2KkN3pobvAEZxxJETOEPjAtsirfP23MlPjY5woD0K0_aSvSNjudoyiUR7I8B_TpoOK2B-OFv7L78sQ04dZQqbAqvpEeAGCk7xwQAtT9hc)

Ahora con nuestro usuario si queremos hacer algo como root por lo general usamos SUDO

Sudo tendrá efecto si nuestro usuario está en la lista de **Sudoers**.

```bash
nano /etc/sudoers
```

Buscamos **root ALL=(ALL) ALL** y abajo ponemos nuestro usuario

Para que tenga permisos y mismos privilegios al ejecutar sudo.

```bash
nombre_de_usuario ALL=(ALL) ALL
```

[https://lh3.googleusercontent.com/tJgznZ1kUZ_qzlKQiTZqV6VgrqCkdFv_2GXz-7AzLx8rZSDwxca46GSaY3tkGTZTtNgE4ozeNfE75Qjt_9LsJ5X6AOxL2ZRDARLjy-MdDiEFzwl2rFko9PtMDs9P2QCFZ6gtIUrN9hNTTxiCcIQ](https://lh3.googleusercontent.com/tJgznZ1kUZ_qzlKQiTZqV6VgrqCkdFv_2GXz-7AzLx8rZSDwxca46GSaY3tkGTZTtNgE4ozeNfE75Qjt_9LsJ5X6AOxL2ZRDARLjy-MdDiEFzwl2rFko9PtMDs9P2QCFZ6gtIUrN9hNTTxiCcIQ)

Ctrl + W para buscar palabras en nano

Ctrl + O para guardar en nano

Ctrl + X para cerrar en nano

### **Activando Servicios**

Dynamic Host Configuration Protocol (DHCP)

El servidor proporciona a los clientes una dirección IP dinámica,

La máscara de subred, Gracias a "systemd" podemos activar ese servicio

Detección y configuración automática para conectarse a la red, respetar la mayúscula

```bash
systemctl enable dhcpcd NetworkManager
```

Si instalaron paquetes de Bluetooth activen el servicio:

```bash
systemctl enable bluetooth
```

### **Mirrorlist en root**

**Reflector** es un script que es capaz de generar una lista y usa los repositorios más rápidos

Ordenarlos en base a su velocidad, y sobrescribir el archivo /etc/pacman.d/mirrorlist

[root@archiso /]# pacman -Sy reflector

[root@archiso /]# reflector --verbose --latest 15 --sort rate --save /etc/pacman.d/mirrorlist

### **GRUB - Gestor de Arranque**

Instalamos GRUB y os prober

**Os-prober** detecta más sistemas operativos si los tuviéramos, que usará grub para su menú (opcional)

```bash
pacman -S grub os-prober
```

Aquí solo importa instalar grub en el disco donde esta ArchLinux instalado en mi caso es **/dev/sda**

```bash
grub-install /dev/sda
```

[https://lh3.googleusercontent.com/q1CRgYVDY7Cs_u4AxqyhgHFGWfDhmQdbMLHLpCJQyjlxKXxQ_2GjY62QNsyY8VLImAQEagOj0POFVGRrfuxY3p6Hpb8o5rKrLALtD1mTf20NcwK-PorbY0RdZfyRgIcVO746cS976sOKrYOX1zk](https://lh3.googleusercontent.com/q1CRgYVDY7Cs_u4AxqyhgHFGWfDhmQdbMLHLpCJQyjlxKXxQ_2GjY62QNsyY8VLImAQEagOj0POFVGRrfuxY3p6Hpb8o5rKrLALtD1mTf20NcwK-PorbY0RdZfyRgIcVO746cS976sOKrYOX1zk)

```bash
[root@archiso /]# grub-mkconfig -o /boot/grub/grub.cfg
```

```
mkinitcpio -p
```

### **Saliendo de LiveCD**

```bash
exit
```

```bash
exit
```

[https://lh4.googleusercontent.com/6z88kImm2Xu13wiAZ2eDZBgZ4lOXqG7q3iiczzrPFfps11-Qwl2HgCxNM42lhRIeZpUB5qX6kSPIjByl_ZknA9F3modQuDHt2q20fDPunnMGoFzgMloCG0Lj8r7jYiYppp3pg3hcH9l14m1KIYU](https://lh4.googleusercontent.com/6z88kImm2Xu13wiAZ2eDZBgZ4lOXqG7q3iiczzrPFfps11-Qwl2HgCxNM42lhRIeZpUB5qX6kSPIjByl_ZknA9F3modQuDHt2q20fDPunnMGoFzgMloCG0Lj8r7jYiYppp3pg3hcH9l14m1KIYU)

Salimos del sistema, desmontamos todas las particiones...

```bash
umount -R /mnt
```

Y finalmente reiniciamos, **retiramos la USB o el CD cuando este apagada la pc**

Entramos como **ROOT**

```bash
reboot
```

### **Después de reiniciar**

[https://lh6.googleusercontent.com/DKCMV60wuKMTDWVW7SPnnwbGBLFRKFhx_rj4nTcoG59aLeAds7AucBCsTqZILh_AXivnN4RrQWTmxFmtvSzsvoFDV_tVsVIlZPJUU_R6CUhW_ernPl-jQthwAfiSLlIP2KFTwJRaF2q72fFNgHw](https://lh6.googleusercontent.com/DKCMV60wuKMTDWVW7SPnnwbGBLFRKFhx_rj4nTcoG59aLeAds7AucBCsTqZILh_AXivnN4RrQWTmxFmtvSzsvoFDV_tVsVIlZPJUU_R6CUhW_ernPl-jQthwAfiSLlIP2KFTwJRaF2q72fFNgHw)

[https://lh5.googleusercontent.com/DBs6-A0tJJSxflZTUMI-Niy9sRT6dkoHnvLDTVT37zlyH-m2a9h_17qjVJmm0hLBUTU90vHlSnJbscvAflEYz1gM3IFSjVrlEivHfUGkdndDS-S4IhvymuOXyVKIf6BkaOrruuU2GsvOfs3-oVc](https://lh5.googleusercontent.com/DBs6-A0tJJSxflZTUMI-Niy9sRT6dkoHnvLDTVT37zlyH-m2a9h_17qjVJmm0hLBUTU90vHlSnJbscvAflEYz1gM3IFSjVrlEivHfUGkdndDS-S4IhvymuOXyVKIf6BkaOrruuU2GsvOfs3-oVc)

### Activamos nuestra tarjeta de red con el siguiente comando

```bash
ip link
```

[https://lh4.googleusercontent.com/bf-EGic7nONAM1TRwrZ0Vnar5mDzhEYSIDqSqM2VoOuuaCE4aq7EgeJ74309YWv6s0SJ1blEAIKvjtlkLEBLlHhjgGtr9CpKyCJnwTt1uUC57GbrXJNXBgfrLe3CbPvUH9QaVg4So4516G4XD8o](https://lh4.googleusercontent.com/bf-EGic7nONAM1TRwrZ0Vnar5mDzhEYSIDqSqM2VoOuuaCE4aq7EgeJ74309YWv6s0SJ1blEAIKvjtlkLEBLlHhjgGtr9CpKyCJnwTt1uUC57GbrXJNXBgfrLe3CbPvUH9QaVg4So4516G4XD8o)

**> Para conectarnos a Wifi:**

```bash
nmcli dev wifi list
```

Si la red tiene espacios solo ponemos comilla simple o doble comillas en el bssid:

[root@nombre_de_pc ~]# nmcli dev wifi connect 'NOMBRE DE RED' password CLAVE

Verificamos la conexión:

```bash 
nmcli dev status
```
Estamos listos para iniciar hacer descargas en nuestro sistema

**Pero siempre antes de hacer alguna instalación siempre actualizamos el sistema con:**

```bash
pacman -Syu
```

### **Personalizando PACMAN**

El archivo general de configuración de pacman se encuentra en:

```bash
nano /etc/pacman.conf
```

Nos ubicamos en la parte que dice: **# Misc options**

 ```bash
# Misc options
#UseSyslog
Color
#NoProgressBar
CheckSpace
VerbosePkgLists
ParallelDownloads = 5
ILoveCandy
```

Y agregamos **ILoveCandy** (es una i mayús y luego una L)

[https://lh6.googleusercontent.com/xH6nNlsoSeATA6P_ep4cXOFnAx2yoPniy6EJ7sFuomogONgWpYWN5uyD8mgi2s26Bgw0trCO0mZUWTCcKvSXvTr6oRRnQDftQU1jlr4XVwq7WEddVgo5o5PgEPLbbLkgEMklyLkeqVhsERG44iE](https://lh6.googleusercontent.com/xH6nNlsoSeATA6P_ep4cXOFnAx2yoPniy6EJ7sFuomogONgWpYWN5uyD8mgi2s26Bgw0trCO0mZUWTCcKvSXvTr6oRRnQDftQU1jlr4XVwq7WEddVgo5o5PgEPLbbLkgEMklyLkeqVhsERG44iE)

Luego quitamos el **#** comentario en el repositorio de **MultiLib**

Esto nos brinda varias librerías de 32bits, en juegos o programas viejos que necesitan esas librerias

```bash
[multilib]
Include = /etc/pacman.d/mirrorlist
```

[https://lh6.googleusercontent.com/nO5fnZEuH1s3ZqLiPG_2DqrpmFiRBAktmckheBfksJQzr5AMXciKAt0jN5NW_8-S9Kslri314qRkcgxpFN5HM-D5tqSvKcvJTqpPPK41VYDz3PItNFnOnBAQ0Bxxicj1BOR3bnPL5Yx-Ou7mWlk](https://lh6.googleusercontent.com/nO5fnZEuH1s3ZqLiPG_2DqrpmFiRBAktmckheBfksJQzr5AMXciKAt0jN5NW_8-S9Kslri314qRkcgxpFN5HM-D5tqSvKcvJTqpPPK41VYDz3PItNFnOnBAQ0Bxxicj1BOR3bnPL5Yx-Ou7mWlk)

Actualizamos el sistema para ver el color y animación de pacman:

```bash
pacman -Syu
```

[https://lh4.googleusercontent.com/_ChoEG5nDFQRr6bWPRvs9xiG6g0xjSZ0eIgxKPVHmQlw5YpmIapfx5fKfp-60tYVGfGtFJTUSXefKbd1JV-8JZfaCT158x3DTCdT1C6eNjccPyCXxD50jeVPtIP1xcPydXoSmEPpqrbjRl3dRn0](https://lh4.googleusercontent.com/_ChoEG5nDFQRr6bWPRvs9xiG6g0xjSZ0eIgxKPVHmQlw5YpmIapfx5fKfp-60tYVGfGtFJTUSXefKbd1JV-8JZfaCT158x3DTCdT1C6eNjccPyCXxD50jeVPtIP1xcPydXoSmEPpqrbjRl3dRn0)

### **Programas Extras**

```bash
pacman -Sy git wget
```

/* Descargas con git

/* Descargar con wget

```bash
pacman -Sy neofetch
```

```bash
pacman -Sy intel-ucode
```

Los fabricantes de procesadores lanzan actualizaciones de estabilidad y seguridad para el microcódigo de procesador **(código cerrado)**

No tenemos los directorios comunes:

Escritorio

Documentos

Descargas

Música

Imágenes

Public

Plantillas

Videos

```bash
pacman -S xdg-user-dirs
```

[https://lh6.googleusercontent.com/Pf0qB31MZZ-rYFU0rhWJwPbHyaN9qUmGFeBLPryjvVwHuydld3ThSz71SMk05WhjzpZDpzW1ghA44xLqw0OqEG8qtqhIk5sb_zGHilfNGwEaYx_01C8t-fdcf9y7pyLiyEGdgOKUl6ITHcUKXz0](https://lh6.googleusercontent.com/Pf0qB31MZZ-rYFU0rhWJwPbHyaN9qUmGFeBLPryjvVwHuydld3ThSz71SMk05WhjzpZDpzW1ghA44xLqw0OqEG8qtqhIk5sb_zGHilfNGwEaYx_01C8t-fdcf9y7pyLiyEGdgOKUl6ITHcUKXz0)

La forma en que funciona es que **xdg-user-dirs-update** se ejecuta

Luego crea versiones localizadas de estos directorios

En el directorio de inicio de los usuarios con iconos especiales

```bash
xdg-user-dirs-update
```

[https://lh3.googleusercontent.com/ceKkxsm2OedgYlJaTCRaBcGs7IbwURQ5RMKD237Hl976GPgqb06YQ5_pFDuTn_atFovu1IBXoTIH2CqgCO0vX4uZL42ptIvlBYhf7mM0xZgx07uo7shlb09h_5H0ftN32iPUHMIttIZWB6ASrvQ](https://lh3.googleusercontent.com/ceKkxsm2OedgYlJaTCRaBcGs7IbwURQ5RMKD237Hl976GPgqb06YQ5_pFDuTn_atFovu1IBXoTIH2CqgCO0vX4uZL42ptIvlBYhf7mM0xZgx07uo7shlb09h_5H0ftN32iPUHMIttIZWB6ASrvQ)

### **X.Org Server**

X.Org Server proporciona las herramientas estándar para proveer de interfaces gráficas

X.Org es necesario tanto para Desktop environment (DE) y Window manager (WM)

```bash
pacman -Sy xorg xorg-apps xorg-xinit xorg-twm xterm xorg-xclock
```

Para ejecutar X.org

```bash
startx
```

### **Driver de Vídeo**

Con el siguiente comando nos dará información de nuestra tarjeta gráfica

```bash
lspci | grep VGA
```

**Para tarjetas de Intel**

```bash
pacman -S xf86-video-intel
```

**Audio**

```bash
pacman -S pulseaudio pavucontrol
```

### **Fuentes de Letra**

```bash
[root@nombre_de_pc ~]# pacman -S gnu-free-fonts ttf-hack ttf-inconsolata
```

### **Instalacion y configuracion  de paru**

Instale primero git y base-devel Asociación de paquetes que contiene   herramientas para crear (clasificar y vincular) paquetes a partir del código fuente

```bash
sudo pacman -S --needed base-devel
```

Git Clone Paru Repository con el comando:

```bash
git clone [https://aur.archlinux.org/paru.git](https://aur.archlinux.org/paru.git)
```

Cambie al paru Directorio:

```bash
cd paru
```

Finalmente, cree e instale el asistente Paru AUR en Arch Linux usando el venidero comando:

```bash
makepkg -si
```

**Configuración de paru**

### Habilitar color en Paru

```bash
sudo nano /etc/pacman.conf
```

Una vez que abra el archivo de configuración de pacman, desconecte el "Color" para habilitar esta función.

[https://lh3.googleusercontent.com/ekwMkUDuHnEI2cGOhsvKTWxJjtgD_WADGN4TxSHUhqKC4AaDZs9e_zB1U0m8xN_uopb9UqKWKIQDd5lWCeUJTSqXxWG3Wxo570cDmHlvtSoEzwsqbJVXGPPezy6YXIVQPfaHG_qPkTFVNpm1Yg](https://lh3.googleusercontent.com/ekwMkUDuHnEI2cGOhsvKTWxJjtgD_WADGN4TxSHUhqKC4AaDZs9e_zB1U0m8xN_uopb9UqKWKIQDd5lWCeUJTSqXxWG3Wxo570cDmHlvtSoEzwsqbJVXGPPezy6YXIVQPfaHG_qPkTFVNpm1Yg)

### **Voltear orden de búsqueda**

```bash
sudo nano /etc/paru.conf
```

Descomente el término "BottomUp" y guarde el archivo.

[https://lh6.googleusercontent.com/llXMZwwzyVdKLbIV0iL4S64bCKAxLLuov6Qe71fd0kLErn_k9oDCzO57O3tHXoKyRrEpbZ5xM8e7ikbbIIQrYIxoizyGYJVY3fdesFNi-Rc4HRY5v9DjXbN_ngFwg5Rw9bPWxI5olIv2vRleGA](https://lh6.googleusercontent.com/llXMZwwzyVdKLbIV0iL4S64bCKAxLLuov6Qe71fd0kLErn_k9oDCzO57O3tHXoKyRrEpbZ5xM8e7ikbbIIQrYIxoizyGYJVY3fdesFNi-Rc4HRY5v9DjXbN_ngFwg5Rw9bPWxI5olIv2vRleGA)

```bash
sudo pacman -S vifm
```

```bash
sudo nano /etc/paru.conf
```

Abra el archivo de configuración y elimine los comentarios como se muestra a continuación.

[https://lh5.googleusercontent.com/0bHsX7vZN-6q6yd3dYn06rKND5JYhhdFJpO9ahCcf9KcaPjfLYsJ4rmGLwiu62J_hBXXGvSeg8DmldcCBKqTHssuHAWNQIGRP-THq4nVdtED-5vRZXw1t2NXnbvEhGIAswotV7ok0uf3GWBdsQ](https://lh5.googleusercontent.com/0bHsX7vZN-6q6yd3dYn06rKND5JYhhdFJpO9ahCcf9KcaPjfLYsJ4rmGLwiu62J_hBXXGvSeg8DmldcCBKqTHssuHAWNQIGRP-THq4nVdtED-5vRZXw1t2NXnbvEhGIAswotV7ok0uf3GWBdsQ)

**Firmware complementario**

```bash
paru -S mkinitcpio-firmware
```

**Paquete opcional para leer discos ntfs**

```bash
sudo pacman -S ntfs-3g
```

Utilidades

```bash
sudo pacman -S exa bat
```

```bash
paru -S shell-color-scripts
```
