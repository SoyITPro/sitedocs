---
date: 2026-06-03T16:33:18-05:00
draft: false
title: Coreutils de Linux llegan a Windows 11
description: Comandos como ls, cp, rm y más de Linux ahora son nativos en Windows.
categories:
  - Windows
tags:
  - linux
  - comandos
---

Durante décadas, las **GNU Coreutils** han sido el conjunto de herramientas esenciales en sistemas **Linux y macOS**. Comandos como `ls`, `cat`, `rm`, `mv`, `cp`, `echo` o `grep` forman la base de la administración de archivos y procesos en entornos UNIX.

Estos comandos son esenciales para todos los que administran Linux y es lo primero que se aprende.

Ahora, gracias a Microsoft, *Si, ya sé me van a decir, Windows copiando a Linux como siempre* 😄 estas utilidades están disponibles **de forma nativa en Windows**, sin necesidad de WSL.

---

## 🔎 ¿Qué son las Coreutils?

Las **Coreutils** son un conjunto de utilidades básicas que permiten:

- **Listar archivos** (`ls`)
- **Copiar y mover** (`cp`, `mv`)
- **Eliminar** (`rm`)
- **Concatenar y mostrar** (`cat`)
- **Buscar y filtrar** (`grep`, `find`)

Son la columna vertebral de cualquier script o flujo de automatización en Linux.

Con esto Microsoft quiere eliminar las barreras que se puedan tener a la hora de usar diferentes Sistemas Operativos y que los usuarios, sobre todo los desarrolladores en esta nueva era de IA se sientan cómodos al usar Windows como su centro de IA con las diferentes herramientas que ya usan en otros entornos.

---

## ⚙️ Instalación en Windows

Existen dos formas principales de instalar las Coreutils en Windows:

1. **Con WinGet (recomendado)**  

``` powershell
   winget install Microsoft.Coreutils
```

*Requiere PowerShell 7.4 o superior.*

El repositorio puedes encontrarlo en https://github.com/microsoft/coreutils
## 🖥️ Uso en Windows

Una vez instaladas, cada utilidad se expone como un ejecutable (ls.exe, cat.exe, grep.exe).

Puedes usarlas directamente en CMD o PowerShell:

``` powershell
ls -la
cat archivo.txt | grep error
find . -name "*.log"
mkdir prueba
touch prueba.txt
rm -rf prueba
```

## ⚠️ Consideraciones

Conflictos de nombres: algunos comandos (find, sort) ya existen en CMD/PowerShell.

La versión que se ejecuta dependerá del PATH y de los alias configurados en PowerShell.

✅ Conclusión

Con la llegada de las coreutils a Windows 11 los desarrolladores y administradores de sistemas podrán usar los mismos comandos en los diferentes Sistemas Operativos y evitar la fricción que esto puede generar.

## 📽️ Video completo

{{< youtube Mle3Jm0pwRQ >}} 
