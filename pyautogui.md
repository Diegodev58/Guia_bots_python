# 🤖 Bot **Headless** con todas las **funciones de PyAutoGUI + Python**  
**Sin Selenium**: controla el escritorio, teclado, ratón, pantalla y archivos… todo **sin ventana gráfica** (modo virtual Xvfb) o en segundo plano.

---

## 🧰 Instalación rápida
```bash
pip install pyautogui opencv-python pillow keyboard mouse pygetwindow pynput psutil
# Para modo 100 % headless (Linux)
sudo apt install xvfb
```

---

## 📦 1. Importar todas las librerías
```python
import pyautogui        # Ratón, teclado, pantalla
import cv2              # OpenCV (imágenes, template matching)
import numpy as np
import psutil           # Info del sistema
import keyboard         # Hooks de teclado global
import mouse            # Hooks de ratón global
import pygetwindow      # Gestión de ventanas
from PIL import Image   # Captura y edición de imágenes
import time, os, json, csv
```

---

## 2. Configurar seguridad
```python
pyautogui.FAILSAFE = True        # Mover ratón a la esquina superior izquierda = abortar
pyautogui.PAUSE = 0.05           # Pausa entre acciones (seg)
```

---

## 3. FUNCIÓN MASTER – bot completo
```python
def headless_bot():
    # ---------- 3.1 CONFIGURAR DISPLAY VIRTUAL (Linux) ----------
    # Ejecutar antes: Xvfb :99 -screen 0 1920x1080x24 &
    os.environ["DISPLAY"] = ":99"  # Apunta al display virtual

    # ---------- 3.2 TAMAÑO DE PANTALLA ----------
    w, h = pyautogui.size()        # (1920, 1080) en Xvfb
    print(f"Pantalla virtual: {w}x{h}")

    # ---------- 3.3 CLICK ----------
    x, y = 500, 300
    pyautogui.click(x, y)                          # Click izquierdo
    pyautogui.doubleClick(x, y)                    # Doble-click
    pyautogui.rightClick(x, y)                     # Click derecho
    pyautogui.middleClick(x, y)                    # Click central

    # ---------- 3.4 MOVER SIN CLICK ----------
    pyautogui.moveTo(100, 200, duration=0.5)       # 0.5 s de animación
    pyautogui.moveRel(50, -30)                     # Mover 50 px derecha, 30 arriba

    # ---------- 3.5 ARRASTRAR ----------
    pyautogui.dragTo(600, 400, duration=1, button='left')
    pyautogui.dragRel(-100, 0, duration=0.5)       # Arrastrar relativo

    # ---------- 3.6 SCROLL ----------
    pyautogui.scroll(5)        # 5 “clics” de scroll hacia arriba
    pyautogui.scroll(-5)       # Hacia abajo
    pyautogui.hscroll(10)      # Scroll horizontal (algunos drivers)

    # ---------- 3.7 TECLADO ----------
    pyautogui.write('¡Hola bot headless!', interval=0.05)  # Escribir texto
    pyautogui.press('enter')                               # Presionar tecla única
    pyautogui.hotkey('ctrl', 'c')                          # Atajo
    pyautogui.keyDown('shift')                             # Mantener shift
    pyautogui.press(['left', 'left', 'left'])              # Varias teclas
    pyautogui.keyUp('shift')

    # ---------- 3.8 CAPTURA DE PANTALLA ----------
    screenshot = pyautogui.screenshot()            # PIL Image
    screenshot.save("captura.png")
    screenshot_np = np.array(screenshot)           # Convertir a NumPy
    screenshot_bgr = cv2.cvtColor(screenshot_np, cv2.COLOR_RGB2BGR)

    # ---------- 3.9 BÚSQUEDA POR IMAGEN ----------
    template = cv2.imread('icono.png', 0)          # Plantilla en escala de grises
    gray = cv2.cvtColor(screenshot_bgr, cv2.COLOR_BGR2GRAY)
    res = cv2.matchTemplate(gray, template, cv2.TM_CCOEFF_NORMED)
    _, max_val, _, max_loc = cv2.minMaxLoc(res)
    if max_val > 0.8:                              # Umbral de confianza
        h, w = template.shape
        center = (max_loc[0] + w//2, max_loc[1] + h//2)
        pyautogui.click(center)

    # ---------- 3.10 LEER COLOR DE PÍXEL ----------
    r, g, b = pyautogui.pixel(100, 100)
    print(f"RGB(100,100) = ({r},{g},{b})")

    # ---------- 3.11 VENTANAS ----------
    ventanas = pygetwindow.getAllTitles()
    for t in ventanas:
        if "Bloc de notas" in t:
            wnd = pygetwindow.getWindowsWithTitle(t)[0]
            wnd.activate()          # Traer al frente
            wnd.resizeTo(800, 600)
            wnd.moveTo(0, 0)

    # ---------- 3.12 HOOKS DE TECLADO Y RATÓN ----------
    def on_key(event):
        print("Tecla:", event.name)
    keyboard.on_press(on_key)

    def on_click(event):
        print("Click en", event.x, event.y)
    mouse.on_click(on_click)

    # ---------- 3.13 ESCRIBIR CSV ----------
    data = [("acción", "x", "y", "timestamp"),
            ("click", 500, 300, time.time())]
    with open("log_bot.csv", "w", newline='') as f:
        writer = csv.writer(f)
        writer.writerows(data)

    # ---------- 3.14 INFO DEL SISTEMA ----------
    cpu = psutil.cpu_percent(interval=1)
    mem = psutil.virtual_memory().percent
    print(f"CPU {cpu}% | RAM {mem}%")

    # ---------- 3.15 ESPERAR Y FINALIZAR ----------
    time.sleep(2)
    keyboard.unhook_all()
    mouse.unhook_all()
    print("Bot headless finalizado")

# ---------- 4. EJECUTAR ----------
if __name__ == "__main__":
    headless_bot()
```

---

## 🐳 Ejecutar 100 % headless (Linux)
```bash
# 1. Crear display virtual
Xvfb :99 -screen 0 1920x1080x24 &

# 2. Lanzar bot
DISPLAY=:99 python bot_pyautogui.py
```

---

Con este script tienes **todas las funciones de PyAutoGUI** listas para usar en modo **headless** o segundo plano.
