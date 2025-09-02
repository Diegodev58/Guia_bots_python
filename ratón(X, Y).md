# 📍 Posición **exacta** del ratón (X, Y) en **tiempo real** con Python

## ✅ Opción 1: con `pyautogui` (1 línea)
```python
import pyautogui, time

try:
    while True:                      # Bucle infinito
        x, y = pyautogui.position()  # Tupla (x, y)
        print(f'\rX: {x:4}  Y: {y:4}', end='', flush=True)
        time.sleep(0.05)             # 50 ms de refresco
except KeyboardInterrupt:
    print("\nFinalizado.")
```

Ejecuta el script y mueve el ratón: verás las coordenadas actualizándose **instantáneamente** en la terminal.

---

## ✅ Opción 2: con `pynput` (listeners modernos)
```python
from pynput.mouse import Listener
import time

def on_move(x, y):
    print(f'\rX: {x}  Y: {y}', end='', flush=True)

with Listener(on_move=on_move) as listener:
    try:
        listener.join()
    except KeyboardInterrupt:
        listener.stop()
```

---

### 🚀 **Uso rápido**
Guarda el bloque en `mouse_pos.py` y ejecuta:
```bash
python mouse_pos.py
```
Mueve el ratón y obtendrás su posición **exacta y en tiempo real**.
