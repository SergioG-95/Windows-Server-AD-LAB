# Ficha técnica: Carpetas compartidas mediante SMB

## 1. Objetivo

Crear recursos compartidos en Windows Server para que los usuarios del dominio `empresa.test` puedan acceder a los archivos de su departamento desde el equipo cliente Windows 11.

## 2. Entorno

| Elemento | Configuración |
|---|---|
| Servidor de archivos | `SRV-DC01` |
| Dirección IP del servidor | `192.168.56.10` |
| Equipo cliente | `PC-CLI01` |
| Dirección IP del cliente | `192.168.56.20` |
| Dominio | `empresa.test` |
| Protocolo de compartición | SMB |

## 3. Estructura de carpetas

Las carpetas se encuentran en el servidor, dentro de `C:\EmpresaDatos`.

```text
C:\EmpresaDatos
├── Administracion
└── Ventas
```

## 4. Recursos compartidos

| Departamento | Ruta local | Nombre del recurso | Ruta de red |
|---|---|---|---|
| Administración | `C:\EmpresaDatos\Administracion` | `Administracion` | `\\SRV-DC01\Administracion` |
| Ventas | `C:\EmpresaDatos\Ventas` | `Ventas` | `\\SRV-DC01\Ventas` |

## 5. Acceso por departamento

Cada recurso se destina al grupo de seguridad correspondiente del dominio:

- **Administración:** `GG_Administracion`.
- **Ventas:** `GG_Ventas`.

La configuración de los permisos NTFS y SMB se documentará en la ficha técnica de permisos.

## 6. Pruebas de funcionamiento

Las pruebas se realizaron desde el equipo `PC-CLI01`, utilizando cuentas del dominio:

- Ana García (`ana.garcia`) puede acceder al recurso de Administración, pero no al de Ventas.
- Carlos López (`carlos.lopez`) puede acceder al recurso de Ventas, pero no al de Administración.

Estas comprobaciones permiten verificar que los recursos están disponibles por red y que el acceso está separado por departamento.

## 7. Evidencias

### 7.1. Configuración de la carpeta compartida en Windows Server

Captura de la configuración del recurso compartido de Administración en el servidor.

![Configuración de la carpeta Administración en Windows Server](../screenshots/W%20Server%20-%20Configuraci%C3%B3n%20carpeta%20Administraci%C3%B3n.png)

### 7.2. Acceso a la carpeta compartida desde Windows 11

Comprobación del acceso al recurso compartido desde el equipo cliente `PC-CLI01`.

![Carpeta compartida en Windows 11](../screenshots/W%2011%20-%20carpeta%20compartida%20en%20cliente.png)

### 7.3. Prueba de permisos desde Windows 11

Verificación de que los usuarios pueden acceder a las carpetas autorizadas y no pueden acceder a las del otro departamento.

![Prueba de permisos en Windows 11](../screenshots/W%2011%20-%20prueba%20de%20permisos%20en%20cliente.png)


