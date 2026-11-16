# Abrir Terminal y Clonar Repositorio en WINDOWS
## Guía Paso a Paso para Principiantes

---

## 📋 Requisitos Previos

Antes de empezar, verifica que tengas instalado:

### 1. Git
**¿Cómo sé si lo tengo?**
- Abre PowerShell (ver abajo)
- Escribe: `git --version`
- Si ves algo como `git version 2.40.0`, ✅ tienes Git

**¿Si no lo tengo?**
- Descarga desde: https://git-scm.com/download/win
- Instala con configuración por defecto

### 2. GitHub CLI (Opcional pero recomendado)
- Descarga desde: https://cli.github.com
- Instala

---

## 🪟 PASO 1: Abre PowerShell en Windows

### Método 1: Atajo de Teclado (MÁS FÁCIL)

**Paso A:** Presiona estas dos teclas juntas:
```
Win + R
```

Se abre una pequeña ventana negra que dice "Ejecutar"

**Paso B:** En la ventana, escribe exactamente esto:
```
powershell
```

**Paso C:** Presiona la tecla **Enter**

✅ **¡Se abre PowerShell (terminal azul)!**

---

### Método 2: Desde el Menú Inicio

**Paso A:** Click en el icono **Windows** (abajo a la izquierda)

**Paso B:** Escribe en la barra de búsqueda:
```
powershell
```

**Paso C:** Click en "Windows PowerShell"

✅ **¡Se abre PowerShell!**

---

## ✅ Cómo se ve PowerShell abierto

Verás una ventana azul oscura con algo como:

```
Windows PowerShell
Copyright (C) Microsoft Corporation.

PS C:\Users\tuusuario>
```

**¡Eso significa que está lista!**

---

## 📂 PASO 2: Navega a una Carpeta para el Proyecto

En PowerShell, escribe uno de estos comandos:

### Opción A: Escritorio
```powershell
cd Desktop
```

### Opción B: Documentos
```powershell
cd Documents
```

### Opción C: Mi PC (cualquier carpeta)
```powershell
cd C:\Users\tuusuario\projects
```
(reemplaza `tuusuario` con tu nombre de usuario)

Luego presiona **Enter**

---

## 🔗 PASO 3: Clona el Repositorio

En PowerShell, escribe exactamente esto:

```powershell
git clone https://github.com/4GeeksAcademy/alexvargas1312-ctrl-repoprincipal.git
```

Presiona **Enter**

**Verás algo como:**
```
Cloning into 'alexvargas1312-ctrl-repoprincipal'...
remote: Enumerating objects: 50, done.
remote: Counting objects: 100% (50/50), done.
Receiving objects: 100% (50/50), done.
```

✅ **¡Se está descargando!**

Espera a que termine (20-30 segundos)

---

## 📁 PASO 4: Entra en la Carpeta del Proyecto

Una vez que termine la descarga, escribe:

```powershell
cd alexvargas1312-ctrl-repoprincipal
```

Presiona **Enter**

Ahora estás dentro de la carpeta del proyecto.

---

## ✅ PASO 5: Verifica que Todo Está Correcto

Escribe:
```powershell
git status
```

Presiona **Enter**

**Debes ver algo como:**
```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

✅ **¡El repositorio está clonado correctamente!**

---

## 💻 PASO 6: Abre en VS Code (Recomendado)

### Si tienes VS Code instalado:

En PowerShell, escribe:
```powershell
code .
```

Presiona **Enter**

✅ **¡VS Code se abrirá con tu proyecto!**

---

## 🎯 RESUMEN: Los Comandos en Orden

Copia y pega esto en PowerShell, línea por línea:

```powershell
# 1. Ir a Documentos
cd Documents

# 2. Clonar el repositorio
git clone https://github.com/4GeeksAcademy/alexvargas1312-ctrl-repoprincipal.git

# 3. Entrar en la carpeta
cd alexvargas1312-ctrl-repoprincipal

# 4. Verificar que está correcto
git status

# 5. Abrir en VS Code
code .
```

**Cada comando presiona Enter después de escribirlo.**

---

## 📊 Vista Completa del Proceso

```
┌─────────────────────────────────────────┐
│ Tu computadora Windows                  │
├─────────────────────────────────────────┤
│ Win + R                                 │
│ ↓                                       │
│ Escribes: powershell                    │
│ ↓                                       │
│ Presionas: Enter                        │
│ ↓                                       │
│ ✅ PowerShell abierto (ventana azul)    │
├─────────────────────────────────────────┤
│ En PowerShell escribes:                 │
│ cd Documents                            │
│ ↓                                       │
│ git clone https://github.com/...       │
│ ↓                                       │
│ cd alexvargas1312-ctrl-repoprincipal    │
│ ↓                                       │
│ code .                                  │
│ ↓                                       │
│ ✅ VS Code abierto con tu proyecto      │
└─────────────────────────────────────────┘
```

---

## 📸 Screenshots de lo que Verás

### Screenshot 1: Presionar Win + R
```
┌──────────────────────┐
│   Ejecutar           │
├──────────────────────┤
│ _______________      │
│                      │
│  [Aceptar] [Cancelar]│
└──────────────────────┘
```

Escribes: `powershell`

---

### Screenshot 2: PowerShell Abierto
```
┌─────────────────────────────────────────┐
│ Windows PowerShell                      │
├─────────────────────────────────────────┤
│                                         │
│ PS C:\Users\tuusuario>                  │
│ _                                       │
│                                         │
└─────────────────────────────────────────┘
```

Aquí escribes los comandos.

---

### Screenshot 3: Clonando
```
PS C:\Users\tuusuario\Documents> git clone https://github.com/...

Cloning into 'alexvargas1312-ctrl-repoprincipal'...
remote: Enumerating objects: 50, done.
remote: Counting objects: 100% (50/50), done.
Receiving objects: 100% (50/50), 250 KiB | 500 KiB/s, done.
```

---

### Screenshot 4: VS Code Abierto
```
┌──────────────────────────────────────────┐
│ VS Code con el proyecto                  │
├──────────────────────────────────────────┤
│ 📁 alexvargas1312-ctrl-repoprincipal    │
│   📄 BRIEFING.md                         │
│   📄 Project_Plan.md                     │
│   📄 Stock_Alerts_System.md              │
│   📁 agents                              │
│   📁 data                                │
│   ... (más archivos)                     │
└──────────────────────────────────────────┘
```

✅ **¡Éxito! Ya tienes el proyecto localmente.**

---

## 🆘 Si Algo Sale Mal

### Error: "git: The term 'git' is not recognized"
**Significa:** Git no está instalado
**Solución:** Descarga desde https://git-scm.com/download/win

### Error: "fatal: destination path already exists"
**Significa:** La carpeta ya existe
**Solución:** Elimina la carpeta o clona en otro lado

### Error: "code: The term 'code' is not recognized"
**Significa:** VS Code no está en el PATH
**Solución:** 
- Abre VS Code
- Presiona: Ctrl + Shift + P
- Escribe: install code command in PATH
- Presiona Enter

### La terminal se cierra sin mostrar nada
**Significa:** Hay un error
**Solución:** Abre PowerShell nuevamente y escribe lentamente cada comando

---

## 💡 Atajos Útiles en PowerShell

```powershell
# Ver carpeta actual
pwd

# Listar archivos
ls

# Ver contenido de archivo
cat BRIEFING.md

# Limpiar pantalla
clear

# Ir a carpeta anterior
cd ..

# Ir a Home
cd ~
```

---

## ✅ Checklist Final

- [ ] Abrí PowerShell (Win + R → powershell → Enter)
- [ ] Navegué a una carpeta (cd Documents)
- [ ] Cloné el repo (git clone https://...)
- [ ] Entré en la carpeta (cd alexvargas1312-ctrl-repoprincipal)
- [ ] Verifiqué (git status)
- [ ] Abrí en VS Code (code .)
- [ ] Veo los archivos en VS Code (BRIEFING.md, Project_Plan.md, etc.)

✅ **Si marcaste todas, ¡estás listo para empezar!**

---

## 🎯 Próximo Paso

Una vez que tengas VS Code abierto con el proyecto:

1. ✅ Lee **BRIEFING.md** (entiende TrackFlow)
2. ✅ Lee **Project_Plan.md** (entiende el proyecto)
3. ✅ Lee **Stock_Alerts_System.md** (entiende el componente)
4. ✅ Abre la terminal integrada en VS Code (Ctrl + `)
5. ✅ Comienza a codear

---

*Guía Windows — TrackFlow Tech · 4Geeks Academy*
*Generada: 13 Nov 2026*
