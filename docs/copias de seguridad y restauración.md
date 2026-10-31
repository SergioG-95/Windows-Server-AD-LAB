# Ficha técnica: Copias de seguridad y restauración

## 1. Objetivo

Configurar un sistema de copias de seguridad en Windows Server mediante la herramienta **Windows Server Backup**, utilizando un segundo disco virtual como destino de almacenamiento.

En esta práctica se realizó una copia de seguridad de una carpeta compartida del servidor y posteriormente se comprobó la recuperación de un archivo eliminado o perdido mediante el asistente de restauración.

## 2. Entorno

| Elemento | Configuración |
|---|---|
| Servidor | `SRV-DC01` |
| Sistema operativo | Windows Server |
| Dirección IP | `192.168.56.10` |
| Disco del sistema | `C:` |
| Disco destinado a copias | `E:` |
| Carpeta de destino | `E:\CopiasSeguridad` |
| Carpeta protegida | `C:\EmpresaDatos\Administracion` |
| Herramienta | Windows Server Backup |

Se añadió un segundo disco virtual a la máquina `SRV-DC01`, que posteriormente se inicializó utilizando el estilo de partición GPT, se formateó con NTFS y se asignó la letra `E:`.

En este disco se creó la carpeta `CopiasSeguridad`, destinada al almacenamiento de las copias.

## 3. Instalación de Windows Server Backup

Se instaló la característica **Copia de seguridad de Windows Server** desde el Administrador del servidor, siguiendo esta ruta:

**Administrar → Agregar roles y características → Características → Copia de seguridad de Windows Server.**

Una vez completada la instalación, se abrió la herramienta desde:

**Administrador del servidor → Herramientas → Copia de seguridad de Windows Server.**

## 4. Configuración de la copia de seguridad

Se utilizó el asistente **Copia de seguridad una vez**, seleccionando las siguientes opciones:

- Tipo de copia: Personalizado.
- Elemento seleccionado: `C:\EmpresaDatos\Administracion`.
- Destino: Unidades locales.
- Unidad de almacenamiento: `E:`.

Se dejó la configuración avanzada con sus valores predeterminados.

Se realizaron dos copias de seguridad:

| Hora | Contenido | Resultado |
|---|---|---|
| 15:55 | Carpeta Administración vacía | Correcta |
| 16:00 | Carpeta Administración con el documento de prueba | Correcta |

La segunda copia incluyó el archivo `Documento_prueba.txt`, creado para comprobar posteriormente el proceso de restauración.

## 5. Prueba de restauración

Para comprobar que la copia de seguridad permitía recuperar información, se utilizó el asistente **Recuperar** de Windows Server Backup.

Se seleccionó la copia correspondiente a las 16:00, ya que contenía el documento de prueba.

En el asistente se eligieron las siguientes opciones:

| Parámetro | Configuración |
|---|---|
| Elemento a recuperar | `Documento_prueba.txt` |
| Destino de recuperación | Ubicación original |
| Si el archivo ya existe | Crear copias para tener ambas versiones |
| Permisos de seguridad | Restaurar los permisos originales |

La recuperación finalizó correctamente a las 16:09.

## 6. Comprobaciones realizadas

Después de completar la restauración, se accedió a la carpeta:

`C:\EmpresaDatos\Administracion`

Se comprobó que el archivo `Documento_prueba.txt` estaba disponible y que conservaba exactamente el contenido original.

También se verificó desde la consola de Windows Server Backup que las dos copias de seguridad y la recuperación de archivos aparecían con el resultado **Correcta**.

## 7. Evidencias

### 7.1. Copias de seguridad y restauración

Esta captura muestra el resultado de las dos copias de seguridad realizadas y la recuperación del archivo, todas ellas completadas correctamente.

![Copias de seguridad y restauración completadas](../screenshots/W%20Server%20-%20Copia%20de%20seguridad%20y%20restauraci%C3%B3n%20completada.png)

### 7.2. Documento restaurado

Esta captura muestra el archivo `Documento_prueba.txt` recuperado en la carpeta Administración del servidor.

![Documento restaurado](../screenshots/W%20Server%20-%20Documento%20restaurado.png)

## 8. Conocimientos adquiridos

- Añadir y configurar discos virtuales en VirtualBox.
- Inicializar discos mediante GPT.
- Crear y formatear volúmenes NTFS.
- Instalar y utilizar Windows Server Backup.
- Realizar copias de seguridad personalizadas.
- Seleccionar unidades locales como destino de las copias.
- Consultar versiones de copias de seguridad disponibles.
- Restaurar archivos desde una copia anterior.
- Comprobar la integridad de los archivos recuperados.
