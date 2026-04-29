# Crear-EXE-jecutable-de-archivo-Python

1. ¿Qué es PyInstaller?

PyInstaller empaqueta tu script Python y todas sus dependencias en un solo ejecutable. El usuario final no necesita tener Python instalado.
2. Instalación
bash

# Instalación básica
pip install pyinstaller

# Con soporte de compresión (opcional, mejora el tamaño)
pip install pyinstaller[encryption]

3. Comando Base Explicado
bash

python -m PyInstaller [OPCIONES] script.py

4. Opciones fundamentales
Opción	Qué hace	Ejemplo
--onefile	Un solo archivo .exe	--onefile
--onedir	Carpeta con múltiples archivos	--onedir
--console	Muestra consola (programas CLI)	--console
--noconsole	Sin consola (aplicaciones GUI)	--noconsole
--name	Nombre del ejecutable	--name="MiApp"
--icon	Ícono del ejecutable	--icon="logo.ico"
--add-data	Incluir archivos adicionales	--add-data "archivo.txt;."
--hidden-import	Incluir módulos no detectados	--hidden-import modulo
5. Ejemplos según tipo de aplicación
Aplicación de Consola (CLI)
bash

python -m PyInstaller --onefile --console --name="MiCLI" mi_script.py

Aplicación con Interfaz Gráfica (GUI)
bash

python -m PyInstaller --onefile --noconsole --name="MiAppGUI" app.py

Script con recursos (imágenes, configuraciones)
bash

python -m PyInstaller --onefile --noconsole --name="MiApp" --add-data "imagenes;imagenes" --add-data "config.ini;." app.py

Script complejo con dependencias
bash

python -m PyInstaller --onefile --console --name="Complejo" --hidden-import pandas --hidden-import numpy script.py

6. Trabajar con archivo .spec (recomendado para proyectos grandes)
Paso 1: Generar archivo .spec
bash

pyi-makespec --onefile --console --name="MiApp" mi_app.py

Paso 2: Editar el archivo .spec
python

# mi_app.spec
a = Analysis(
    ['mi_app.py'],
    pathex=[],
    binaries=[],
    datas=[('config.ini', '.'), ('imagenes/*.png', 'imagenes')],  # Agregar recursos
    hiddenimports=['requests', 'json'],  # Dependencias ocultas
    hookspath=[],
    runtime_hooks=[],
    excludes=[],
    noarchive=False
)
pyz = PYZ(a.pure)
exe = EXE(
    pyz,
    a.scripts,
    a.binaries,
    a.datas,
    name='MiApp',
    debug=False,
    bootloader_ignore_signals=False,
    strip=False,
    upx=True,  # Compresión
    console=True  # True = consola, False = sin consola
)

Paso 3: Construir desde .spec
bash

pyinstaller mi_app.spec

7. Comandos prácticos para diferentes situaciones
Si tienes errores de importación
bash

# Incluir módulos específicos
python -m PyInstaller --onefile --hidden-import=modulo_faltante script.py

# Usar --debug para ver problemas
python -m PyInstaller --onefile --debug script.py

Si el antivirus lo detecta como falso positivo
bash

# Usar --upx-dir o deshabilitar compresión
python -m PyInstaller --onefile --noupx --name="MiApp" script.py

Si necesitas permisos de administrador
bash

python -m PyInstaller --onefile --uac-admin --name="MiApp" script.py

Para reducir el tamaño del ejecutable
bash

python -m PyInstaller --onefile --strip --exclude-module tkinter --exclude-module matplotlib script.py

8. Script de ejemplo (app.py)
python

#!/usr/bin/env python
# app.py - Ejemplo simple

import sys
import os

def main():
    # Detectar si es ejecutable o script
    if getattr(sys, 'frozen', False):
        application_path = os.path.dirname(sys.executable)
    else:
        application_path = os.path.dirname(os.path.abspath(__file__))
    
    print(f"Ejecutando desde: {application_path}")
    print("¡Hola desde el ejecutable!")
    
    input("Presiona Enter para salir...")

if __name__ == "__main__":
    main()

9. Flujo de trabajo completo
bash

# 1. Instalar PyInstaller
pip install pyinstaller

# 2. Probar tu script normalmente
python mi_script.py

# 3. Crear ejecutable
python -m PyInstaller --onefile --console --name="MiExecutable" mi_script.py

# 4. Ver resultado
cd dist
MiExecutable.exe

# 5. Limpiar archivos temporales (opcional)
cd ..
rmdir /s /q build *.spec  # Windows
# rm -rf build *.spec      # Linux/Mac

10. Dónde encontrar el ejecutable
text

tu_proyecto/
├── mi_script.py
├── build/           # Archivos temporales (puedes borrar)
├── dist/            # ✅ AQUÍ ESTÁ TU EXE
│   └── MiExecutable.exe
└── mi_script.spec   # Archivo de configuración

11. Solución de problemas comunes
Problema	Solución rápida
PyInstaller not found	pip install pyinstaller --upgrade
Archivo muy grande	Usar --exclude-module para excluir librerías no usadas
Error "No module named X"	Agregar con --hidden-import X
La consola no aparece	Cambiar --noconsole a --console
El exe se cierra rápido	Agregar input() al final del script
DLL missing en Windows	Instalar Visual C++ Redistributable
12. Crear un batch para automatizar (Windows)

Crear build_exe.bat:
batch

@echo off
echo ========================================
echo Creando ejecutable de Python
echo ========================================

echo Instalando PyInstaller...
pip install pyinstaller

echo Eliminando builds anteriores...
rmdir /s /q build dist 2>nul
del *.spec 2>nul

echo Creando ejecutable...
python -m PyInstaller --onefile --console --name="MiApp" app.py

echo.
echo ========================================
echo ¡Ejecutable creado en dist/MiApp.exe!
echo ========================================
pause

13. Comparativa de herramientas
Herramienta	Ventajas	Desventajas
PyInstaller	Más popular, fácil, multiplataforma	Ejecutables grandes
auto-py-to-exe	Interfaz gráfica, basado en PyInstaller	Menos control
cx_Freeze	Más ligero	Configuración más compleja
Nuitka	Más rápido, mejor rendimiento	Compilación más lenta
Py2exe	Solo Windows	Antiguo, menos soporte
14. Comandos rápidos para copiar y pegar
bash

# MÍNIMO (consola, un archivo)
python -m PyInstaller --onefile --console script.py

# MÍNIMO (GUI, un archivo)
python -m PyInstaller --onefile --noconsole script.py

# COMPLETO (consola, con nombre e ícono)
python -m PyInstaller --onefile --console --name="MiApp" --icon="icono.ico" script.py

# PRODUCCIÓN (sin consola, con recursos)
python -m PyInstaller --onefile --noconsole --name="MiApp" --add-data "recursos;recursos" script.py
