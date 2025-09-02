# 📂 Guía Rápida de **todos los comandos y funciones de la librería `os`**  
Lista práctica con **ejemplos comentados línea a línea** para bots headless o cualquier script Python.

```python
import os
import platform
import subprocess

# ---------- 1. INFORMACIÓN DEL SISTEMA ----------
print(os.name)                    # 'posix' (Linux/macOS) o 'nt' (Windows)
print(platform.system())          # 'Linux', 'Windows', 'Darwin'
print(os.getcwd())                # Directorio de trabajo actual
print(os.listdir())               # Lista de archivos/carpetas en cwd

# ---------- 2. NAVEGACIÓN Y RUTAS ----------
os.chdir('/tmp')                  # Cambiar directorio
ruta_abs = os.path.abspath('archivo.txt')   # Ruta absoluta
ruta_join = os.path.join('carpeta', 'sub', 'file.log')  # Une sin importar SO
print(os.path.dirname(ruta_abs))  # Parte carpeta
print(os.path.basename(ruta_abs)) # Parte archivo
print(os.path.splitext('data.csv'))  # ('data', '.csv')

# ---------- 3. CREAR / ELIMINAR / MOVER ----------
os.mkdir('nueva_carpeta')         # Crear carpeta
os.makedirs('a/b/c', exist_ok=True)  # Crear árbol
os.rename('viejo.txt', 'nuevo.txt')  # Renombrar/mover
os.remove('temp.txt')             # Borrar archivo
os.rmdir('carpeta_vacia')         # Borrar carpeta vacía
import shutil
shutil.rmtree('carpeta_con_contenido')  # Borrar carpeta con todo dentro

# ---------- 4. VARIABLES DE ENTORNO ----------
os.environ['MI_VAR'] = 'valor'    # Crear/modificar
print(os.getenv('MI_VAR'))        # Leer
print(os.environ.get('PATH'))     # Leer PATH
os.environ.pop('MI_VAR', None)    # Eliminar

# ---------- 5. EJECUTAR COMANDOS DEL SISTEMA ----------
# Opción 1: os.system (simple, sin output)
os.system('echo Hola > salida.txt')

# Opción 2: subprocess (recomendado, captura output)
resultado = subprocess.run(['ls', '-la'], capture_output=True, text=True)
print(resultado.stdout)           # Texto de stdout
print(resultado.returncode)       # 0 = OK

# Ejecutar y obtener PID
pid = os.getpid()                 # PID del script actual
print(f"PID actual: {pid}")

# ---------- 6. GESTIÓN DE PERMISOS ----------
os.chmod('script.sh', 0o755)      # Cambiar permisos Unix

# ---------- 7. INFORMACIÓN DE ARCHIVOS ----------
stats = os.stat('archivo.txt')
print(stats.st_size)              # Tamaño en bytes
print(stats.st_mtime)             # Última modificación (timestamp)

# ---------- 8. TEMPORALES ----------
tmp = os.path.join(os.sep, 'tmp') if os.name == 'posix' else os.getenv('TEMP')
tmp_file = os.path.join(tmp, 'temp_bot.dat')
with open(tmp_file, 'w') as f:
    f.write('datos')
os.unlink(tmp_file)               # Borrar temporal

# ---------- 9. LISTAR ÁRBOLES ----------
for root, dirs, files in os.walk('.'):
    for name in files:
        print(os.path.join(root, name))

# ---------- 10. COMBINAR CON PATHLIB (moderno) ----------
from pathlib import Path
p = Path.cwd() / 'subdir' / 'file.txt'
p.mkdir(parents=True, exist_ok=True)
p.write_text('contenido')
print(p.read_text())

# ---------- 11. JUGAR CON PROCESOS ----------
# Abrir notepad en Windows (headless si se usa Xvfb en Linux)
if platform.system() == 'Windows':
    os.startfile('notepad.exe')
else:
    subprocess.Popen(['gedit'])  # o nano, vim, etc.

# ---------- 12. LIMPIEZA DE CARPETAS TEMPORALES ----------
for root, dirs, files in os.walk('/tmp/mi_bot'):
    for file in files:
        try:
            os.remove(os.path.join(root, file))
        except OSError:
            pass

# ---------- 13. CREAR ARCHIVO DE LOG ----------
log_path = os.path.join(os.getcwd(), 'bot.log')
with open(log_path, 'a') as log:
    log.write(f"{time.strftime('%Y-%m-%d %H:%M:%S')} - Bot iniciado\n")

# ---------- 14. EJEMPLO ÚNICO: RESUMEN DE FUNCIONES ----------
"""
os.getcwd()          -> str
os.listdir(path)     -> list
os.chdir(path)       -> None
os.mkdir(path)       -> None
os.makedirs(path, exist_ok=True) -> None
os.remove(path)      -> None
os.rmdir(path)       -> None
os.rename(src, dst)  -> None
os.path.abspath(rel) -> str
os.path.join(...)    -> str
os.path.dirname(p)   -> str
os.path.basename(p)  -> str
os.path.splitext(p)  -> (root, ext)
os.getenv(key)       -> str|None
os.environ           -> dict
os.system(cmd)       -> int (exit code)
subprocess.run(args, capture_output=True) -> CompletedProcess
os.stat(path)        -> os.stat_result
os.chmod(path, mode) -> None
os.walk(top)         -> generator
os.unlink(path)      -> None
"""
```

Copia y pega cualquier bloque: todos los comandos `os` están **listos para usar** en tus bots headless o scripts de automatización.
