# Crear-EXE-jecutable-de-archivo-Python

Manual de PyInstaller para ClientRDP
1. Sintaxis del Comando
bash

python -m PyInstaller --onefile --console --name="ClientRDP" client_rdp.py

2. Explicación de cada parámetro
Parámetro	Explicación
python -m PyInstaller	Ejecuta PyInstaller como módulo de Python
--onefile	Genera un único archivo .exe en lugar de una carpeta con múltiples archivos
--console	Muestra una ventana de consola/terminal al ejecutar el programa
--name="ClientRDP"	Especifica el nombre del ejecutable resultante
client_rdp.py	Script Python que quieres convertir a ejecutable
3. Instalación previa
bash

# Instalar PyInstaller si no lo tienes
pip install pyinstaller

# Verificar instalación
pyinstaller --version

4. Variantes del comando según necesidades
Sin consola (aplicación GUI)
bash

python -m PyInstaller --onefile --noconsole --name="ClientRDP" client_rdp.py

Con icono personalizado
bash

python -m PyInstaller --onefile --console --icon="icono.ico" --name="ClientRDP" client_rdp.py

Agregando metadatos
bash

python -m PyInstaller --onefile --console --name="ClientRDP" --version-file="version.txt" client_rdp.py

5. Crear archivo de especificaciones (más control)
bash

# Generar archivo .spec
pyi-makespec --onefile --console --name="ClientRDP" client_rdp.py

# Editar el archivo ClientRDP.spec según necesidades

# Construir desde el .spec
pyinstaller ClientRDP.spec

6. Estructura de ejemplo de client_rdp.py
python

#!/usr/bin/env python
# client_rdp.py - Cliente RDP básico

import subprocess
import sys

def main():
    print("=== ClientRDP ===")
    print("Iniciando conexión RDP...")
    
    # Ejemplo de conexión RDP (modificar según necesidades)
    servidor = input("Servidor RDP: ")
    usuario = input("Usuario: ")
    
    # Comando mstsc para Windows
    cmd = f"mstsc /v:{servidor}"
    
    try:
        subprocess.run(cmd, shell=True)
        print(f"Conectando a {servidor} con usuario {usuario}")
    except Exception as e:
        print(f"Error: {e}")
        input("Presione Enter para salir...")

if __name__ == "__main__":
    main()

7. Comandos útiles adicionales
bash

# Limpiar builds anteriores
rmdir /s /q build dist *.spec  # Windows
rm -rf build dist *.spec       # Linux/Mac

# Build con más control
pyinstaller --onefile --console \
    --name="ClientRDP" \
    --clean \
    --noconfirm \
    client_rdp.py

# Incluir recursos adicionales
pyinstaller --onefile --console \
    --add-data "config.ini;." \
    --add-data "cert.pem;." \
    --name="ClientRDP" client_rdp.py

8. Solución de problemas comunes
Problema	Solución
Archivo muy grande	Usar --upx-dir con UPX compresor
Falso positivo antivirus	Firmar el ejecutable o usar --uac-admin
DLL faltantes	Especificar rutas con --paths
Import errors	Usar --hidden-import modulo_faltante
9. Optimización y buenas prácticas
bash

# Build optimizado
pyinstaller --onefile --console \
    --name="ClientRDP" \
    --strip \
    --noupx \
    --exclude-module tkinter \
    --exclude-module matplotlib \
    client_rdp.py

# Para producción
pyinstaller --onefile --noconsole \
    --name="ClientRDP" \
    --uac-admin \
    --version-file="version_info.txt" \
    --manifest="manifest.xml" \
    client_rdp.py

10. Ejemplo de archivo version_info.txt
ini

VSVersionInfo(
  ffi=FixedFileInfo(
    filevers=(1, 0, 0, 0),
    prodvers=(1, 0, 0, 0),
    mask=0x3f,
    flags=0x0,
    OS=0x40004,
    fileType=0x1,
    subtype=0x0,
    date=(0, 0)
  ),
  kids=[
    StringFileInfo(
      [
        StringTable(
          u'040904B0',
          [StringStruct(u'CompanyName', u'Tu Empresa'),
          StringStruct(u'FileDescription', u'Cliente RDP'),
          StringStruct(u'FileVersion', u'1.0.0.0'),
          StringStruct(u'InternalName', u'ClientRDP'),
          StringStruct(u'LegalCopyright', u'Copyright 2024'),
          StringStruct(u'OriginalFilename', u'ClientRDP.exe'),
          StringStruct(u'ProductName', u'ClientRDP'),
          StringStruct(u'ProductVersion', u'1.0.0.0')])
      ]),
    VarFileInfo([VarStruct(u'Translation', [1033, 1200])])
  ]
)

11. Resultado esperado

Después de ejecutar el comando:

    build/ - Archivos temporales (puedes borrarlos)

    dist/ClientRDP.exe - ¡Tu ejecutable final!

    client_rdp.spec - Archivo de especificaciones

12. Ejecutar el comando en diferentes sistemas

Windows:
cmd

python -m PyInstaller --onefile --console --name="ClientRDP" client_rdp.py

Linux/Mac:
bash

python3 -m PyInstaller --onefile --console --name="ClientRDP" client_rdp.py


