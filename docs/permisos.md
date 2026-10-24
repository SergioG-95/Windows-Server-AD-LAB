# Ficha técnica: Permisos NTFS y SMB

## 1. Objetivo

Configurar los permisos de acceso a las carpetas compartidas del dominio `empresa.test` para que cada usuario pueda acceder únicamente a los archivos de su departamento.

## 2. Entorno

| Elemento | Configuración |
|---|---|
| Servidor | `SRV-DC01` |
| Dirección IP del servidor | `192.168.56.10` |
| Equipo cliente | `PC-CLI01` |
| Dominio | `empresa.test` |
| Grupos de seguridad | `GG_Administracion` y `GG_Ventas` |

## 3. Diferencia entre permisos NTFS y SMB

- **Permisos NTFS:** controlan qué usuarios y grupos pueden acceder a los archivos y carpetas almacenados en el sistema de archivos del servidor.
- **Permisos SMB:** controlan el acceso a esos archivos cuando se utilizan a través de un recurso compartido de red.

Cuando se accede a una carpeta por red, los permisos efectivos dependen de ambas configuraciones.

## 4. Carpetas y grupos autorizados

| Departamento | Ruta local | Grupo autorizado |
|---|---|---|
| Administración | `C:\EmpresaDatos\Administracion` | `GG_Administracion` |
| Ventas | `C:\EmpresaDatos\Ventas` | `GG_Ventas` |

## 5. Configuración de permisos

Se asignaron permisos de modificación al grupo correspondiente en cada carpeta mediante NTFS.

En los permisos del recurso compartido SMB, se concedió al grupo correspondiente acceso de cambio y lectura. Se conservaron los permisos necesarios para la administración del servidor.

La finalidad es que cada grupo pueda trabajar con los archivos de su departamento sin acceder a los del otro.

## 6. Pruebas de funcionamiento

Las pruebas se realizaron desde el equipo cliente `PC-CLI01`, utilizando las cuentas del dominio:

| Usuario | Departamento | Administración | Ventas |
|---|---|---|---|
| Ana García (`ana.garcia`) | Administración | Permitido | Denegado |
| Carlos López (`carlos.lopez`) | Ventas | Denegado | Permitido |

Los resultados confirmaron que la separación de acceso entre departamentos funcionaba correctamente.

## 7. Evidencias

### 7.1. Configuración de la carpeta compartida

La siguiente captura muestra la configuración de la carpeta del departamento de Administración en el servidor `SRV-DC01`.

![Configuración de la carpeta Administración](../screenshots/W%20Server%20-%20Configuraci%C3%B3n%20carpeta%20Administraci%C3%B3n.png)

### 7.2. Permisos de acceso

En esta captura se muestra la configuración de los permisos de la carpeta de Administración, utilizada para controlar el acceso de los usuarios del dominio.

![Permisos de la carpeta Administración](../screenshots/W%20Server%20-%20Permisos%20de%20Administraci%C3%B3n.png)

### 7.3. Pruebas desde el equipo cliente

Las siguientes capturas muestran el acceso a las carpetas compartidas y las pruebas realizadas desde el cliente Windows 11.

![Carpeta compartida en el cliente](../screenshots/W%2011%20-%20carpeta%20compartida%20en%20cliente.png)

![Prueba de permisos en el cliente](../screenshots/W%2011%20-%20prueba%20de%20permisos%20en%20cliente.png)

