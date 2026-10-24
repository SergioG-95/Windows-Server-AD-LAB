# Ficha técnica: Estructura de Active Directory

## 1. Objetivo

Documentar la estructura del dominio de Active Directory creado en el laboratorio, incluyendo las unidades organizativas (OU), los usuarios y los grupos de seguridad.

El objetivo es centralizar la administración de las cuentas de usuario y organizar los recursos de la empresa para facilitar la aplicación de permisos y directivas de grupo.

## 2. Entorno

| Elemento | Configuración |
|---|---|
| Servidor de dominio | `SRV-DC01` |
| Dirección IP | `192.168.56.10` |
| Dominio DNS | `empresa.test` |
| Nombre NetBIOS | `EMPRESA` |
| Servicios | Active Directory Domain Services (AD DS) y DNS |
| Equipo cliente | `PC-CLI01` |
| Sistema operativo cliente | Windows 11 Pro |

## 3. Estructura de unidades organizativas

Se creó una estructura de unidades organizativas para organizar los objetos del dominio y facilitar su administración.

```text
empresa.test
└── Empresa
    ├── Usuarios
    │   ├── ana.garcia
    │   └── carlos.lopez
    ├── Equipos
    └── Grupos
        ├── GG_Administracion
        └── GG_Ventas
```

La estructura separa los usuarios, los equipos y los grupos en diferentes unidades organizativas. Esto permite mantener una organización clara y facilita la vinculación de directivas de grupo a las ubicaciones correspondientes.

## 4. Usuarios del dominio

Se crearon dos cuentas de usuario para comprobar el funcionamiento de la autenticación y la asignación de permisos según el departamento.

| Nombre | Cuenta de inicio de sesión | Departamento |
|---|---|---|
| Ana García | `ana.garcia` | Administración |
| Carlos López | `carlos.lopez` | Ventas |

Ambas cuentas pertenecen al dominio `empresa.test` y se utilizan para realizar pruebas desde el equipo cliente `PC-CLI01`.

## 5. Grupos de seguridad

Se crearon dos grupos de seguridad globales para organizar los permisos por departamento.

| Grupo | Finalidad | Usuario miembro |
|---|---|---|
| `GG_Administracion` | Acceso a los recursos de Administración | `ana.garcia` |
| `GG_Ventas` | Acceso a los recursos de Ventas | `carlos.lopez` |

La utilización de grupos permite asignar permisos de forma centralizada, sin tener que configurar cada recurso individualmente para cada usuario.

## 6. Relación con los recursos compartidos

Los grupos de seguridad se utilizan para controlar el acceso a las carpetas compartidas del servidor `SRV-DC01`.

| Grupo | Recurso compartido | Ruta de red |
|---|---|---|
| `GG_Administracion` | Administración | `\\SRV-DC01\Administracion` |
| `GG_Ventas` | Ventas | `\\SRV-DC01\Ventas` |

Esta organización permite que cada usuario acceda a los recursos de su departamento según su pertenencia a los grupos de seguridad.

## 7. Relación con las directivas de grupo

La GPO `GPO_Mapeo_Unidades` está vinculada a la OU `Empresa/Usuarios`.

Mediante los destinatarios de nivel de elemento, las unidades de red se asignan según el grupo de seguridad al que pertenece cada usuario:

- `GG_Administracion`: asignación de la unidad `H:`.
- `GG_Ventas`: asignación de la unidad `V:`.

De esta forma, Active Directory permite centralizar tanto la gestión de usuarios y grupos como la aplicación de determinadas configuraciones a los usuarios del dominio.

## 8. Evidencias

### 8.1. Usuarios del dominio

![Usuarios de Active Directory](../screenshots/W%20Server%20-%20Usuarios.png)

### 8.2. Grupos de seguridad

![Grupos de seguridad de Active Directory](../screenshots/W%20Server%20-%20Departamentos.png)

### 8.3. Configuración de la GPO

![Configuración de la GPO de mapeo de unidades](../screenshots/W%20Server%20-%20Configuraci%C3%B3n%20de%20la%20GPO.png)

## 9. Conocimientos adquiridos

- Creación y administración de un dominio de Active Directory.
- Organización de objetos mediante unidades organizativas.
- Creación y administración de usuarios y grupos de seguridad.
- Asignación de permisos mediante grupos.
- Relación entre Active Directory, recursos compartidos SMB y directivas de grupo.
- Verificación del acceso a recursos desde un equipo cliente unido al dominio.
