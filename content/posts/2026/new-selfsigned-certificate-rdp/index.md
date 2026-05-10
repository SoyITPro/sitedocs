---
title: "Auto-firmar archivos RDP en Windows 11"
date: 2026-04-30
draft: false
description: "Crea certificados autofirmados para advertencia de RDP en Windows 11"
categories:
  - Windows

tags:
 - RDP
---



Guía paso a paso para crear un certificado autofirmado y usarlo para firmar archivos `.rdp`, para las advertencias de seguridad al abrirlos en Windows 11 después de instalar las actualizaciones de abril de 2026 **KB5083769**.



## 📋 Requisitos

- Privilegios de administrador en Terminal o VScode


---

## 🛠️ Paso 1: Crear un certificado autofirmado

### Opción A: Usando PowerShell (Recomendada - Más Rápida)

Abre **PowerShell como Administrador** y ejecuta:

```powershell
New-SelfSignedCertificate -Type CodeSigningCert -Subject "CN=MyRDPCert" -CertStoreLocation "Cert:\LocalMachine\My"

```

### Opción B: Usando el complemento MMC

Presiona Win + R, escribe certlm.msc y presiona Enter

Expande Personal → haz clic derecho en Certificados → Todas las tareas → Solicitar nuevo certificado

Sigue el asistente y crea un certificado con propósito de Firma de código

Nómbralo como prefieras (ej: "Mi Firma RDP")

## 📄 Paso 2: Obtener la huella digital (Thumbprint)
Desde PowerShell (Más Rápido)
```powershell
Get-ChildItem -Path "Cert:\LocalMachine\My" | Where-Object { $_.Subject -like "*RDP-Firma-Local*" } | Format-List Subject, Thumbprint
```

Desde el complemento MMC
Abre certlm.msc → Personal → Certificados

- Doble clic en tu certificado

- Ve a la pestaña Detalles

- Busca el campo Huella digital

- Copia el valor y elimina todos los espacios (debe quedar una cadena continua)

> [!IMPORTANT] 
> ⚠️ La huella debe ser una cadena continua sin espacios y debe ser SHA256 (64 caracteres) para `rdpsign /sha256`.  
> Ejemplo válido SHA256: 3f2c1b0e4d5c6b7a94a1e8a27d8f257f8d7d5d9a3f2c1b0e4d5c6b7a94a1e8a27d8f257f8

## ✍️ Paso 3: Firmar el archivo RDP
Abre Símbolo del sistema (cmd.exe) como Administrador

Ejecuta el siguiente comando:

```cmd
rdpsign /sha256 "HUELLA_DIGITAL_SIN_ESPACIOS" "C:\ruta\completa\a\tu\archivo.rdp"
```
Ejemplo práctico:

```cmd
rdpsign /sha256 a94a1e8a27d8f257f8d7d5d9a3f2c1b0e4d5c6b7 "C:\Users\Usuario\Desktop\servidor.rdp"
```


## ✅ Paso 4: Verificar la firma (Opcional)
Para verificar que el archivo está firmado correctamente sin modificarlo:

```cmd
rdpsign /l "C:\ruta\archivo.rdp"
```
Si la firma es válida, verás un mensaje confirmándolo o puedes abrir el archivo RDP con notepad y verificar la firma.

## 🚀 Paso 5: Hacer que Windows confíe en tu firma

Para que Windows no muestre la advertencia de "Editor no confiable":

Instalar el certificado en el almacén raíz
powershell
- ### Exportar el certificado con PowerShell

Puedes utilizar la MMC y exportar el certificado manualmente utilizando x.509 codificado base 64 .CER o utilizar en PowerShell 

```powershell
$cert = Get-ChildItem -Path "Cert:\LocalMachine\My" | Where-Object { $_.Subject -like "*RDP-Firma-Local*" }
Export-Certificate -Cert $cert -FilePath "C:\Temp\mi-firma-rdp.cer"
```
La importanción la puedes realizar vía MMC en:
- Entidades de certificación de raiz de confianza
- Editores de confianza

Puedes importar los certificados también vía PowerShell:


```powershell 
Import-Certificate -FilePath "C:\Temp\mi-firma-rdp.cer" -CertStoreLocation "Cert:\LocalMachine\Root"
```


### Configurar Directiva de Grupo (GPO)
> [!NOTE] 
> Puedes distribuir el certificado de firma por GPO a los equipos del dominio para que confíen solo en esos .RDP con la firma establecida.

Presiona Win + R, escribe gpedit.msc y presiona Enter

Navega a:

```text
Configuración del Equipo → Plantillas Administrativas → Componentes de Windows → Servicios de Escritorio Remoto → Cliente de Conexión a Escritorio Remoto
Habilita la opción: Especificar las huellas digitales SHA1 de los certificados que representan editores .rdp de confianza
```
Habilita la directiva y agrega la Huella, puedes separar por , diferentes Thumbprint

Haz clic en Aceptar

---

## ✅ Verificación

Para confirmar que todo funciona correctamente:

- Abre el archivo `.rdp` firmado con editor de texto y verifica que contenga la firma digital
- La advertencia de "Editor no confiable" debería desaparecer al abrir el archivo
- Puedes verificar el estado de la firma con: `rdpsign /l "archivo.rdp"`

## ⚠️ Solución de Problemas

| Problema | Causa | Solución |
|----------|-------|----------|
| "El certificado no es válido" | El certificado no es para Firma de Código | Crear un nuevo certificado con `-Type CodeSigningCert` |
| Error al firmar | Certificado no encontrado | Verificar que la huella digital sea correcta (sin espacios) |
| Windows sigue mostrando advertencia | El certificado no está en Raíz de Confianza | Importar el certificado en `Cert:\LocalMachine\Root` |
| `rdpsign` no se encuentra | El comando no está en PATH | Usar la ruta completa: `C:\Windows\System32\rdpsign.exe` |

## 📝 Notas Adicionales

- **Certificados autofirmados**: Solo son válidos en tu equipo o en equipos donde los importes específicamente
- **Dominios**: Si usas Active Directory, distribuye el certificado por GPO para centralizar la confianza
- **Seguridad**: Los certificados autofirmados no son verificados por una autoridad externa, son solo para evitar advertencias locales
- **KB5083769**: Esta actualización aumentó los requisitos de seguridad para los archivos `.rdp`, siendo necesaria esta firma

## 📹 Video completo
{{< youtube ZLx9EZa_MnM >}}


## 🔗 Referencias

- [Microsoft - Remote Desktop Connection Manager](https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/rdp-files)
- [KB5083769 - RDP Security Updates](https://support.microsoft.com/en-us/kb/5083769)
- [New-SelfSignedCertificate - PowerShell](https://learn.microsoft.com/en-us/powershell/module/pki/new-selfsignedcertificate)
- [rdpsign - Firma de archivos RDP](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/rdpsign)
