# Cómo Abrir la Terminal
## Guía por Sistema Operativo

---

## 💻 ¿Qué es una Terminal?

La **terminal** es una aplicación donde puedes escribir comandos de texto para:
- Clonar repositorios de GitHub
- Instalar programas
- Ejecutar código
- Navegar carpetas

---

## 🪟 EN WINDOWS

### Opción 1: PowerShell (RECOMENDADO)

**Paso 1:** Presiona estas teclas juntas
```
Win + R
```
Se abre una ventana pequeña llamada "Ejecutar"

**Paso 2:** Escribe esto:
```
powershell
```

**Paso 3:** Presiona "Enter"

✅ **¡La terminal PowerShell se abrirá!**

---

### Opción 2: Símbolo de Sistema (Cmd)

**Paso 1:** Presiona
```
Win + R
```

**Paso 2:** Escribe:
```
cmd
```

**Paso 3:** Presiona "Enter"

✅ **¡Se abrirá la terminal (más antigua pero funciona)**

---

### Opción 3: Terminal de Windows (Windows 11)

Si tienes Windows 11:

**Paso 1:** Presiona
```
Win + T
```

✅ **¡Se abre directamente!**

---

### Opción 4: Git Bash (Si tienes Git instalado)

**Paso 1:** Abre el Explorador de Archivos (File Explorer)

**Paso 2:** Navega a donde quieras el proyecto

**Paso 3:** Click derecho en la carpeta

**Paso 4:** Selecciona "Git Bash Here"

✅ **¡Git Bash se abre en esa carpeta!**

---

## 🍎 EN MAC

### Opción 1: Usando Spotlight (MÁS RÁPIDO)

**Paso 1:** Presiona estas teclas juntas
```
Cmd + Espacio
```
Se abre Spotlight Search

**Paso 2:** Escribe:
```
terminal
```

**Paso 3:** Presiona "Enter"

✅ **¡Terminal se abre!**

---

### Opción 2: Usando Applications

**Paso 1:** Abre Finder (icono cara azul en dock)

**Paso 2:** Click en "Applications" (lado izquierdo)

**Paso 3:** Abre la carpeta "Utilities"

**Paso 4:** Busca "Terminal" y haz doble click

✅ **¡Terminal se abre!**

---

### Opción 3: Desde VS Code

Si tienes VS Code instalado:

**Paso 1:** Abre VS Code

**Paso 2:** Presiona
```
Ctrl + `
```
(la tecla backtick, arriba de Tab)

✅ **¡Terminal integrada se abre abajo!**

---

## 🐧 EN LINUX

### Opción 1: Atajo de Teclado (MÁS RÁPIDO)

Presiona estas teclas juntas:
```
Ctrl + Alt + T
```

✅ **¡Terminal se abre inmediatamente!**

---

### Opción 2: Desde el Menú

**Paso 1:** Click en el botón "Actividades" (arriba a la izquierda)

**Paso 2:** Escribe "terminal"

**Paso 3:** Click en Terminal

✅ **¡Terminal se abre!**

---

### Opción 3: Click derecho en carpeta

**Paso 1:** Abre el Gestor de Archivos

**Paso 2:** Navega a donde quieras

**Paso 3:** Click derecho

**Paso 4:** Selecciona "Open in Terminal" (o similar)

✅ **¡Terminal se abre en esa carpeta!**

---

## 📸 Cómo se ve una Terminal Abierta

### Windows PowerShell:
```
Windows PowerShell
Copyright (C) Microsoft Corporation.

PS C:\Users\tuusuario>
```

Aquí escribes comandos.

### Mac Terminal:
```
Last login: Wed Nov 13 02:35:00 on ttys000
tuusuario@MacBook-Air ~ %
```

Aquí escribes comandos.

### Linux Terminal:
```
user@computer:~$
```

Aquí escribes comandos.

---

## ✅ Una vez abierta la Terminal

Verás algo como esto:
```
$
```
o
```
C:\Users\tuusuario>
```

Eso significa que la terminal **está lista** para que escribas comandos.

---

## 📝 AHORA YA PUEDES CLONAR

Una vez que tengas la terminal abierta:

**Paso 1:** Navega a donde quieras el proyecto

```bash
# En Windows:
cd Documents

# En Mac/Linux:
cd ~/Documents
```

**Paso 2:** Clona el repositorio

```bash
git clone https://github.com/4GeeksAcademy/alexvargas1312-ctrl-repoprincipal.git
```

**Paso 3:** Entra en la carpeta

```bash
cd alexvargas1312-ctrl-repoprincipal
```

**Paso 4:** Abre en VS Code

```bash
code .
```

✅ **¡Listo!**

---

## 🆘 Troubleshooting

### "git: command not found"
Significa que Git no está instalado.
**Solución:** Descarga Git desde https://git-scm.com

### "code: command not found"
Significa que VS Code no está en el PATH.
**Solución:** 
- Abre VS Code
- Presiona Ctrl+Shift+P (Cmd+Shift+P en Mac)
- Escribe: "install code command in PATH"
- Presiona Enter

### No sé dónde estoy en la terminal
Escribe:
```
pwd
```
(print working directory - muestra dónde estás)

### Quiero ir a una carpeta específica
```bash
cd /ruta/de/la/carpeta
```

### Quiero ver qué hay en la carpeta actual
```bash
ls
```
(en Windows: `dir`)

---

## 🎯 RESUMEN VISUAL

### Windows:
```
1. Presiona: Win + R
2. Escribe: powershell
3. Presiona: Enter
✅ ¡Terminal abierta!
```

### Mac:
```
1. Presiona: Cmd + Espacio
2. Escribe: terminal
3. Presiona: Enter
✅ ¡Terminal abierta!
```

### Linux:
```
1. Presiona: Ctrl + Alt + T
✅ ¡Terminal abierta!
```

---

## 💡 Pro Tips

### Copiar y pegar en terminal:

**Windows PowerShell:**
- Copiar: Ctrl + C
- Pegar: Click derecho (o Ctrl + V)

**Mac Terminal:**
- Copiar: Cmd + C
- Pegar: Cmd + V

**Linux Terminal:**
- Copiar: Ctrl + Shift + C
- Pegar: Ctrl + Shift + V

---

## ✅ ¿Terminal Abierta?

Si ves el cursor (`$`, `%`, o `>`) esperando que escribas, ¡**terminal está lista!**

Ahora puedes ejecutar:
```bash
git clone https://github.com/4GeeksAcademy/alexvargas1312-ctrl-repoprincipal.git
```

---

*Guía de Terminal — TrackFlow Tech · 4Geeks Academy*
*Generada: 13 Nov 2026*
