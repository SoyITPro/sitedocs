---
title: Verificar Low Latency Profile en Windows 11
date: 2026-05-28
draft: false
categories:
  - Windows
tags:
  - WindowsK2
---
**Low Latency Profile** es la característica estrella de la iniciativa Windows K2 que mejorará el rendimiento en Windows 11 y que todos los usuarios están pidiendo a Microsoft.

Esta característica lo que hace es **aumentar la frecuencia de la CPU** para que apartados de la interfaz de Windows 11 como el menú inicio, las aplicaciones, el menú contextual o el explorador de archivos vaya mucho más rápido.

Esto se ha comenzado a desplegar con la actualización lanzada vía Windows Update el 27 de mayo de 2026 **KB5089573** pero aunque la hayas instalado no obtendrás de inmediato esta funcionalidad.

> [!CAUTION] 
>Mi recomendación es que no la habilitar de forma forzada porque puede ocasionar problemas en tu equipo.
>Lo mejor es esperar a que se habilite de forma autónoma cuando tu equipo esté preparado.

## ¿Cómo puedo validar si está habilidad?

Por medio de la herramienta [Vivetool](https://github.com/thebookisclosed/ViVe/releases/tag/v0.3.4) podemos saber si los IDs que hacen referencia a Low Latency Profile se encuentran habilitados en nuestro equipo.
Descarga la herramienta desde su repositorio en GitHub si aún no la tienes y ejecuta la siguiente Query.

``` powershell
.\ViVeTool.exe /query /id:58989092,60716524,48433719,61391826
```


Si recibes una respuesta como esta o parecida donde el estatus es *Disabled* entonces aún tu equipo no está preparado para Low Latency Profile.

``` text
[58989092]
Priority        : ImageDefault (0)
State           : Disabled (1)
Type            : Override (0)

No configuration for feature ID 60716524 was found in the Runtime store
[48433719] (UxAccOptimization)
Priority        : ImageDefault (0)
State           : Disabled (1)
Type            : Override (0)

No configuration for feature ID 61391826 was found in the Runtime store
```

Si se encuentra habilitada te aparecerá de la siguiente forma donde confirma que esta *Enabled* todos los IDs.

``` text
ViVeTool v0.3.4 - Windows feature configuration tool

[58989092]
Priority        : User (8)
State           : Enabled (2)
Type            : Override (0)

[60716524]
Priority        : User (8)
State           : Enabled (2)
Type            : Override (0)

[48433719] (UxAccOptimization)
Priority        : User (8)
State           : Enabled (2)
Type            : Override (0)

[61391826]
Priority        : User (8)
State           : Enabled (2)
Type            : Override (0)
```
## ¿Cómo puedo habilitarla?

Si deseas habilitarla, no le tienes miedo a nada 😁 y quieres probar esto para mejorar el rendimiento de Windows 11 puedes ejecutar con la misma herramienta Vivetool con los IDs pero con el parámetro */enable*.

``` powershell
.\ViVeTool.exe /enable /id:58989092,60716524,48433719,61391826
```

Recibirás una respuesta como esta:

``` text
ViVeTool v0.3.4 - Windows feature configuration tool

Successfully set feature configuration(s)
```

Nuevamente te lo comento, es mejor esperar que se habilite por si sola esta característica y que no vayamos a presentar inestabilidad o pantallazos azules en el equipo.

## 📽️ Video completo
{{< youtube aqjpNRdXIrw >}}
