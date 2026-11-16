# Clonar Repositorio en Máquina Local
## TrackFlow Tech — AI Engineering Project

---

## 📋 Requisitos Previos

Verifica que tengas instalado en tu máquina local:

```bash
# 1. Git
git --version
# Esperado: git version 2.x.x o superior

# 2. GitHub CLI (recomendado)
gh --version
# Esperado: gh version 2.x.x o superior

# 3. Python (opcional pero recomendado)
python --version
# Esperado: Python 3.10+
```

Si no tienes estas herramientas:
- **Git:** https://git-scm.com/download
- **GitHub CLI:** https://cli.github.com
- **Python:** https://www.python.org

---

## 🚀 Paso 1: Abre Terminal en Tu Máquina Local

**En Windows (PowerShell o Git Bash):**
```powershell
# Presiona: Win + R
# Escribe: powershell
# Presiona: Enter
```

**En Mac/Linux:**
```bash
# Presiona: Ctrl + Alt + T (Linux)
# O abre Terminal desde Applications (Mac)
```

---

## 📥 Paso 2: Navega a la Carpeta Donde Quieres Clonar

```bash
# Ejemplo: Carpeta de Proyectos
cd ~/projects

# O si prefieres otra ubicación:
cd ~/Documents/GitHub
cd /Users/tuusuario/projects

# Verifica que estés en la carpeta correcta:
pwd
```

---

## 🔗 Paso 3: Clona el Repositorio

### Opción A: Con HTTPS (Más Fácil)
```bash
git clone https://github.com/4GeeksAcademy/alexvargas1312-ctrl-repoprincipal.git
```

### Opción B: Con SSH (Si tienes SSH configurado en GitHub)
```bash
git clone git@github.com:4GeeksAcademy/alexvargas1312-ctrl-repoprincipal.git
```

### Opción C: Usando GitHub CLI (RECOMENDADO)
```bash
gh repo clone 4GeeksAcademy/alexvargas1312-ctrl-repoprincipal
```

---

## ⏱️ Tiempo de Clonación

**Esperado:** 10-30 segundos (depende de tu velocidad de internet)

```
Cloning into 'alexvargas1312-ctrl-repoprincipal'...
remote: Enumerating objects: 50, done.
remote: Counting objects: 100% (50/50), done.
remote: Compressing objects: 100% (40/40), done.
remote: Total 50 (delta 0), reused 35 (delta 0), pack-reused 0
Receiving objects: 100% (50/50), 250 KiB | 500 KiB/s, done.
```

---

## 📂 Paso 4: Entra en la Carpeta del Proyecto

```bash
cd alexvargas1312-ctrl-repoprincipal

# Verifica que la carpeta se clonó correctamente:
ls -la
```

**Esperado:** Verás archivos como:
```
BRIEFING.md
Project_Plan.md
Stock_Alerts_System.md
README.md
.devcontainer/
agents/
data/
...
```

---

## ✅ Paso 5: Verifica la Clonación

```bash
# Ver rama actual
git branch

# Ver remoto (debe estar origin)
git remote -v

# Ver últimos commits
git log --oneline | head -5

# Ver estado
git status
```

**Esperado:**
```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

---

## 🔐 Paso 6: Configura Git (Si es la primera vez)

```bash
# Configura tu nombre (reemplaza)
git config --global user.name "Tu Nombre"

# Configura tu email (reemplaza)
git config --global user.email "tu.email@example.com"

# Verifica la configuración
git config --global user.name
git config --global user.email
```

---

## 💻 Paso 7: Abre en Tu Editor Favorito

### Opción A: VS Code
```bash
code .
```

### Opción B: Abrir manualmente
- Abre VS Code
- Click en "File" → "Open Folder"
- Selecciona la carpeta `alexvargas1312-ctrl-repoprincipal`

### Opción C: Otro editor
```bash
# Sublime Text
subl .

# JetBrains (PyCharm, etc.)
pycharm .

# Atom
atom .
```

---

## 📊 Estructura Local Esperada

Después de clonar, tu carpeta local debería verse así:

```
alexvargas1312-ctrl-repoprincipal/
├── .git/                          ← Control de versión (Git)
├── .gitignore
├── .devcontainer/                 ← Configuración Codespaces
│   └── devcontainer.json
│
├── 📄 BRIEFING.md                 ← Briefing de TrackFlow
├── 📄 Project_Plan.md             ← Plan del proyecto
├── 📄 Stock_Alerts_System.md      ← Sistema de alertas
├── 📄 SETUP_VERIFICATION.md       ← Verificación
├── 📄 4GEEKS_CONNECTION.md        ← Conexión 4Geeks
├── 📄 README.md
├── 📄 README.es.md
│
├── agents/
├── data/
├── infra/
├── services/
├── skills/
└── ... (más carpetas)
```

---

## 🔄 Paso 8: Configura Git para Commits

```bash
# Verifica la remota
git remote -v
# Debe mostrar:
# origin  https://github.com/4GeeksAcademy/alexvargas1312-ctrl-repoprincipal.git (fetch)
# origin  https://github.com/4GeeksAcademy/alexvargas1312-ctrl-repoprincipal.git (push)
```

---

## 📝 Workflow Git Típico

Una vez clonado, para trabajar localmente:

```bash
# 1. Ver cambios
git status

# 2. Crear rama nueva (opcional)
git checkout -b feature/nombre-de-rama

# 3. Hacer cambios en archivos
# (edita BRIEFING.md, crea nuevos archivos, etc.)

# 4. Ver cambios
git diff

# 5. Agregar cambios
git add .

# 6. Hacer commit
git commit -m "Descripción de cambios"

# 7. Subir a GitHub
git push origin main
# (o git push origin nombre-de-rama si es una rama nueva)
```

---

## ⚠️ Troubleshooting

### Error: "Permission denied (publickey)"
```bash
# Significa que no tienes SSH configurado
# Solución: Usa HTTPS en lugar de SSH
git clone https://github.com/4GeeksAcademy/alexvargas1312-ctrl-repoprincipal.git
```

### Error: "fatal: could not read Username"
```bash
# Git pidiendo credenciales
# En Windows: Usa GitHub CLI (gh repo clone)
# En Mac/Linux: Configura credentials helper:
git config --global credential.helper store
```

### Carpeta no se crea
```bash
# Verifica que tienes permisos de escritura en la carpeta
chmod 755 ~/projects
```

### Quieres clonar en carpeta existente
```bash
# En lugar de:
git clone https://github.com/... alexvargas1312-ctrl-repoprincipal

# Primero crea carpeta vacía:
mkdir mi-proyecto
cd mi-proyecto
git clone https://github.com/... .  # Note el punto al final
```

---

## ✅ Verificación Post-Clonación

Ejecuta estos comandos para verificar que todo está bien:

```bash
# 1. Ver archivos principales
ls -lh BRIEFING.md Project_Plan.md Stock_Alerts_System.md

# 2. Contar líneas
wc -l *.md

# 3. Ver remota
git remote -v

# 4. Ver rama
git branch -a

# 5. Ver logs
git log --oneline | head -5
```

**Esperado:**
```
On branch main
3 archivos .md con contenido
Remota: origin (fetch/push)
Logs mostrados correctamente
```

---

## 🎯 Próximos Pasos

1. ✅ **Clona el repositorio** (usando los pasos arriba)
2. ✅ **Abre en tu editor** (VS Code, PyCharm, etc.)
3. ✅ **Lee BRIEFING.md** (entiende TrackFlow)
4. ✅ **Lee Project_Plan.md** (entiende el proyecto)
5. ✅ **Lee Stock_Alerts_System.md** (entiende el componente)
6. ✅ **Comienza a codear** (implementa Phase 1)

---

## 📞 Comando Rápido (Copia y Pega)

Si simplemente quieres clonar sin leer todo:

```bash
# En tu terminal, navega a donde quieras el proyecto
cd ~/projects

# Clona el repositorio
git clone https://github.com/4GeeksAcademy/alexvargas1312-ctrl-repoprincipal.git

# Entra en la carpeta
cd alexvargas1312-ctrl-repoprincipal

# Verifica
git status

# Abre en VS Code (si tienes instalado)
code .
```

**Eso es todo. En menos de 1 minuto tendrás el proyecto local.**

---

*Instrucciones de Clonación — TrackFlow Tech · 4Geeks Academy*
*Generada: 13 Nov 2026*
