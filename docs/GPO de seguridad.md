# Ficha técnica: GPO de seguridad

## 1. Objetivo

Configurar y comprobar directivas de seguridad mediante las directivas de grupo (GPO) en el dominio `empresa.test`.

En esta práctica se ha configurado el bloqueo de cuentas del dominio tras varios intentos de inicio de sesión fallidos y una restricción de acceso al Panel de control y a Configuración de Windows para los usuarios de la unidad organizativa `Empresa/Usuarios`.

## 2. Entorno

| Elemento | Configuración |
|---|---|
| Servidor de dominio | `SRV-DC01` |
| Dirección IP del servidor | `192.168.56.10` |
| Equipo cliente | `PC-CLI01` |
| Dominio | `empresa.test` |
| Unidad organizativa | `Empresa/Usuarios` |
| GPO de seguridad de usuario | `GPO_Seguridad_Cuentas` |
| Directiva de dominio | `Default Domain Policy` |
| Usuario de prueba | `ana.garcia` |

## 3. Bloqueo de cuentas del dominio

Se configuró la directiva de bloqueo de cuenta en `Default Domain Policy`, siguiendo esta ruta:

**Configuración del equipo → Directivas → Configuración de Windows → Configuración de seguridad → Directivas de cuenta → Directiva de bloqueo de cuenta.**

Los parámetros establecidos fueron los siguientes:

| Parámetro | Valor |
|---|---:|
| Umbral de bloqueo | 5 intentos fallidos |
| Duración del bloqueo | 15 minutos |
| Restablecimiento del contador | 15 minutos |

Esta configuración permite bloquear temporalmente una cuenta cuando se alcanza el número establecido de intentos de inicio de sesión no válidos.

## 4. Restricción del Panel de control

Se creó la GPO `GPO_Seguridad_Cuentas` y se vinculó a la unidad organizativa `Empresa/Usuarios`.

Dentro de la directiva se habilitó la siguiente opción:

**Configuración de usuario → Directivas → Plantillas administrativas → Panel de control → Prohibir el acceso al Panel de control y a Configuración de PC.**

La finalidad es restringir el acceso al Panel de control y a la aplicación Configuración de Windows para los usuarios a los que se aplique esta GPO.

## 5. Pruebas de funcionamiento

Las comprobaciones se realizaron desde el equipo cliente `PC-CLI01`, con la cuenta de dominio `ana.garcia`.

### 5.1. Actualización de directivas

Se ejecutó el siguiente comando:

```cmd
gpupdate /force
```

El comando terminó correctamente, sin informar de errores durante la actualización.

### 5.2. Comprobación de la restricción

Se intentó abrir el Panel de control mediante el comando `control`.

Windows mostró el mensaje:

> Esta operación ha sido cancelada debido a las restricciones especificadas para este equipo.

El resultado confirmó que la restricción estaba funcionando.

### 5.3. Verificación de las GPO aplicadas

Se ejecutó el comando:

```cmd
gpresult /r /scope:user
```

En la lista de objetos de directiva de grupo aplicados aparecieron `GPO_Seguridad_Cuentas` y la otra GPO creada para el laboratorio.

Esto permitió comprobar que ambas directivas figuraban entre las aplicadas al usuario.

## 6. Evidencias

Se añadirán capturas de pantalla de las siguientes comprobaciones:

- Configuración de la directiva de bloqueo de cuenta.
- Configuración de `GPO_Seguridad_Cuentas`.
- Mensaje mostrado al intentar abrir el Panel de control.
- Resultado de `gpresult /r /scope:user`.

## 7. Conocimientos adquiridos

- Creación y vinculación de directivas de grupo.
- Configuración de políticas de bloqueo de cuentas.
- Restricción del acceso al Panel de control mediante una GPO.
- Actualización de directivas con `gpupdate`.
- Verificación de directivas aplicadas mediante `gpresult`.
- Comprobación del funcionamiento de una política desde un equipo cliente.
