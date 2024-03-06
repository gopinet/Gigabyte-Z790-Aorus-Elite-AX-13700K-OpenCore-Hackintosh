### Mis componentes de hardware y software

- **Placa base:** Gigabyte Z790 Aorus Elite ax DDR5 Ver1.0
- **CPU:** I7 13700K (No hay requisitos especiales para la CPU, cualquier otra CPU solo necesita cambiar el nombre de visualización en config.plist)
- **GPU:** Gigabyte RX6600 EAGLE 8G (Cualquier tarjeta gráfica compatible debería funcionar sin necesidad de controladores)
- **macOS:** Sonoma 14.3.1 (Teóricamente, todas las versiones de Sonoma y Ventura son compatibles. No se ha probado con Monterey ni Big Sur)
- **OpenCore:** 0.9.8
- **BIOS:** F9 [Enlace oficial](https://www.aorus.com/motherboards/Z790-AORUS-ELITE-AX-rev-10/Support)



# Advertencia！！！

**Después de actualizar la BIOS, es posible que se restablezca la configuración de la BIOS y necesites volver a configurarla.**

**Después de actualizar la BIOS, a veces puede ser necesario reiniciar una vez e ingresar al sistema para ejecutar ResetNram. De lo contrario, a veces no se puede acceder al sistema. La razón es desconocida y no se sabe si esta operación es efectiva.**



# Acerca de las actualizaciones incrementales

Desactiva BlueToolFixup.kext y luego actualiza para obtener actualizaciones incrementales

He desactivado con éxito los tres kext relacionados con Bluetooth (BlueToolFixup.kext, IntelBluetoothFirmware.kext, IntelBTPatcher.kext) para realizar actualizaciones incrementales.



# Por favor, genera tu propio SMBIOS

Utiliza [OCAT](https://github.com/ic005k/OCAuxiliaryTools/releases) para generar aleatoriamente tu UUID, etc., y luego úsalo.

Puedes verificar si el SystemSerialNumber generado se puede encontrar en el sitio web oficial. Si se muestra algún mensaje de error, significa que es utilizable. Si se encuentra información sobre garantía u otros detalles, significa que es un ID utilizado por el hardware vendido por Apple oficialmente. No lo uses, ya que podría resultar en una prohibición.

Es muy probable que no puedas encontrar información sobre el número generado aleatoriamente, lo cual es una buena señal.



# Acerca de la creación de una unidad de arranque EFI

#### 1. [Creación de ACPI](https://dortania.github.io/Getting-Started-With-ACPI/ssdt-methods/ssdt-easy.html#running-ssdttime)

Esta operación no es necesaria, puedes usar directamente el ACPI que he generado.

Usé la herramienta [SSDTTime](https://github.com/corpnewt/SSDTTime) en Windows para generarlos. Es muy simple, inicialmente pensé que era difícil porque primero leí la documentación en inglés sobre cómo hacerlo manualmente, pero resulta que la herramienta en Windows lo hace automáticamente seleccionando algunas opciones.



#### 2. Personaliza los controladores USB de tu propia caja

Esta operación es muy necesaria.

Usa [USBToolBox](https://github.com/USBToolBox/tool/releases) en Windows para hacerlo y luego reemplaza UTBMap.kext en la EFI.

Si no lo personalizas, puedes usar los puertos USB de la placa base, pero los de la caja podrían no ser reconocidos.

Además, los controladores USB que he personalizado pueden no ser perfectos y la velocidad del puerto puede ser baja.



#### 3. Unidad de arranque USB

~~Anteriormente, descargaba una imagen completa de MacOS y luego la grababa en un USB con [etcher](https://github.com/balena-io/etcher). Luego colocaba la EFI en la partición EFI del USB, pero siempre tenía problemas para encontrar la opción de MacOS al arrancar. Supongo que el problema es que etcher no marca la partición EFI como de arranque. Tal vez cambiar la partición EFI a una de arranque con una herramienta de particionamiento funcione, pero no lo he probado.~~

Anteriormente, usaba el método recomendado por el oficial: [método rufus](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/windows-install.html#rufus-method)

El proceso actual de creación de la unidad de arranque USB (sistema macOS, consulté: https://post.smzdm.com/p/a785e48l/ )

1. Usa [macadmin-scripts](https://github.com/munki/macadmin-scripts) para descargar la imagen de instalación fuera de línea correspondiente. Se descargará automáticamente como un archivo DMG.

   El branch [scheblein/macadmin-scripts](https://github.com/scheblein/macadmin-scripts) soluciona problemas de uso en macOS 13.

2. Monta el archivo DMG en el sistema.

3. Ejecuta sudo /Volumes/nombredeladescargadelaimagen/Contents/Resources/createinstallmedia --volume /Volumes/nombredeunidaddearrancovacía/ 

4. Luego monta el archivo de imagen recién creado y monta su carpeta EFI. Copia la EFI en ella.

5. Usa [etcher](https://github.com/balena-io/etcher) para escribir en el USB.



O bien, usa [gibMacOS](https://github.com/corpnewt/gibMacOS) para descargar el archivo PKG, instálalo en el sistema y luego usa [el método oficial](https://support.apple.com/en-us/HT201372) para crear la unidad USB, lo cual también es muy conveniente.



# Acerca de la instalación

Durante la instalación, **después de que aparezca el logotipo de Apple, la pantalla se apagará durante un período de tiempo considerable**. La primera vez que instalé, la pantalla estuvo en negro durante varios minutos. Actualmente, el tiempo oscuro es de unos segundos, después de lo cual aparecerá la pantalla de configuración o de inicio de sesión.



# Acerca de la EFI

La EFI se actualizará periódicamente.

~~Cambiando el modelo a iMac pro1,1 podría mejorar el rendimiento de un solo núcleo.~~

Ya he agregado CPUFriend y CPUFriendDataProvider, por lo que cambiar el modelo debería tener poco o ningún impacto.

Mi modelo utiliza MacPro y tiene los controladores de CPU habilitados (CPUFriend y CPUFriendDataProvider).



# Acerca de la BIOS

No es necesario seguir mi configuración exacta. Si puedes arrancar y usar el sistema con éxito, no es necesario modificarla.

- **Secure Boot : Disabled**

- **Internal Graphics : Disabled**

- Above 4G Decoding : Enabled

- Above 4GB MMIO BIOS assignment : Enabled

- Re-Size BAR Support : Disabled

- **Intel Platform Trust Technology(PTT): Disabled**

- **Hyper-Threading : Enabled**

- **CFG Lock : Disabled**

![image info](./img/IMG_0390.avif)

![image info](./img/IMG_0391.avif)

![image info](./img/IMG_0392.avif)

![image info](./img/IMG_0393.avif)

![image info](./img/IMG_0394.avif)

![image info](./img/IMG_0395.avif)

![image info](./img/IMG_0396.avif)

![image info](./img/IMG_0397.avif)

# Acerca de las Funcionalidades

- Arranque normal del sistema
- Capacidad para iniciar sesión en la AppStore y descargar software
- Problemas con el modo de suspensión
- No se han probado funciones como iCloud, Mensajes, etc., pero podrían funcionar correctamente. No las he utilizado.
- Después de instalar el sistema, se recomienda realizar un ResetNram.



# Acerca de las Puntuaciones de Rendimiento

- Las puntuaciones de R23 son casi idénticas a las de Windows.
- Si utilizas solo un módulo de memoria RAM, es posible que las puntuaciones de Geekbench sean más bajas de lo esperado.
- Las puntuaciones de rendimiento no son indicativas de todo. Lo importante es que el software pueda utilizar la CPU y la GPU al máximo.



# Software Recomendado

- [MonitorControl](https://github.com/MonitorControl/MonitorControl): Para ajustar el brillo de la pantalla.
- [OCAT](https://github.com/ic005k/OCAuxiliaryTools/releases): Para modificar archivos EFI.
- [Stats](https://github.com/exelban/stats): Herramienta de monitoreo del sistema en la barra de estado.



# Compartiendo Archivos EFI

[Gigabyte-Z790-Aorus-Elite-AX-13700K-OpenCore-Hackintosh](https://github.com/xtvj/Gigabyte-Z790-Aorus-Elite-AX-13700K-OpenCore-Hackintosh)



# Registro de Actualizaciones
