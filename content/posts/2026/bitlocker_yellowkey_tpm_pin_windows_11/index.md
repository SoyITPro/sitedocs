---
title: Bitlocker roto en Windows por YellowKey Microsoft publica mitigación
date: 2026-05-21
draft: false
categories:
  - Windows
tags:
  - bitlocker
  - vulnerabilidad
---

**YellowKey** es el nombre de un exploit o vulnerabilidad publicada en mayo de 2026 que permite saltarse BitLocker en algunos equipos Windows 11 y Windows Server 2022/2025 aprovechando WinRE, el entorno de recuperación de Windows.

#### ¿Qué hace?

En términos simples, YellowKey no “rompe” el cifrado matemático de BitLocker, sino que abusa de una ruta de recuperación para obtener acceso a discos protegidos cuando hay acceso físico al equipo.

Microsoft ha publicado un Script para mitigar esta vulnerabilidad mientras se lanza una actualización que corrija este problema de raíz.

Pero antes debemos validar:

1. Si tenemos habilitado Bitlocker
2. Que tipo de protector tenemos TPM solo o PIN

**¿Por qué?**

Microsoft había recomendado que para mitigar rápidamente esta vulnerabilidad se habilitara el Protector con PIN y evitar que el atacante pueda acceder al entorno de recuperación de Windows **WinRE**
## Ver los protectores actuales de BitLocker

Abrir PowerShell como administrador y ejecutar:

```powershell
Get-BitLockerVolume
```

Esto mostrará los volúmenes cifrados y los tipos de protectores configurados.

Para ver únicamente los protectores de la unidad del sistema:

```powershell
(Get-BitLockerVolume -MountPoint "C:").KeyProtector
```

Ejemplo de salida:

```text
KeyProtectorId      : {GUID}
KeyProtectorType    : Tpm
```

O:

```text
KeyProtectorType    : TpmPin
```

---

# Verificar si el TPM está listo

```powershell
Get-Tpm
```

Validar especialmente:

```text
TpmPresent : True
TpmReady   : True
```

---
El Script publicado por Microsoft lo podemos encontrar en: https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-45585

El Script para que funcione debemos realizar unas modificaciones porque tiene dos errores que al parecer se le pasaron a Microsoft :D 

## 📽️ Video completo
{{< youtube tr7H0M35-5s >}}

> [!CAUTION] 
> ⚠️ Microsoft recomienda habilitar el protector PIN para Bitlocker para maximizar la seguridad.  
> Si ya ejecutaste el Script de mitigación o en el futuro ha salido una actualización para corregir la vulnerabilidad YellowKey no es necesario habilitar el Protector PIN.

---
# Configurar BitLocker con TPM + PIN

## 1. Habilitar la política de PIN al arranque

Ejecutar:

```powershell
gpedit.msc
```

Ir a:

```text
Configuración del equipo
└── Plantillas administrativas
    └── Componentes de Windows
        └── Cifrado de unidad BitLocker
            └── Unidades del sistema operativo
```

Abrir la política:

```text
Requerir autenticación adicional al inicio
```

Configurarla en:

- Habilitada
- Permitir TPM
- Permitir PIN de inicio con TPM

---

## 2. Agregar el protector TPM+PIN

En PowerShell como administrador:

```powershell
Add-BitLockerKeyProtector -MountPoint "C:" -TpmAndPinProtector
```

El sistema solicitará ingresar el PIN.

---

## 3. Verificar que quedó aplicado

```powershell
(Get-BitLockerVolume -MountPoint "C:").KeyProtector
```

Ahora debería aparecer:

```text
KeyProtectorType : TpmPin
```

---
# Comandos útiles

## Ver estado general de BitLocker

```powershell
manage-bde -status
```

## Ver protectores desde manage-bde

```powershell
manage-bde -protectors -get C:
```

## Agregar TPM+PIN desde manage-bde

```powershell
manage-bde -protectors -add C: -TPMAndPIN
```

---

# Recomendaciones

- Guardar la clave de recuperación de BitLocker en un lugar seguro.
- Verificar que el TPM esté habilitado en el BIOS/UEFI.
- Utilizar un PIN fácil de recordar pero difícil de adivinar.
- Probar el reinicio del equipo después de aplicar la configuración.
- En entornos empresariales se recomienda respaldar las claves en Active Directory o Microsoft Entra ID.

---

# Documentación oficial

- https://learn.microsoft.com/windows/security/operating-system-security/data-protection/bitlocker/
- https://learn.microsoft.com/windows-serve