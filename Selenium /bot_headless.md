# 🧪 Funciones esenciales para un bot **headless** en Python (Selenium)  
Todas las operaciones que necesitas: abrir, click, escribir, extraer, scroll, cookies, etc.  
Cada línea está comentada para que sepas exactamente qué hace.

```python
# ---------- 1. Importar bibliotecas ----------
from selenium import webdriver                      # Controlador principal
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver import ActionChains         # Acciones avanzadas
import time, csv, json, os

# ---------- 2. Configurar modo headless ----------
options = Options()
options.add_argument("--headless")                 # Sin ventana gráfica
options.add_argument("--no-sandbox")
options.add_argument("--disable-dev-shm-usage")
options.add_argument("--disable-gpu")
options.add_argument("--window-size=1920,1080")    # Resolución virtual
options.add_argument("--user-agent=Mozilla/5.0 ...")  # UA personalizado

# ---------- 3. Iniciar driver ----------
service = Service("chromedriver")                  # Ruta al ejecutable
driver = webdriver.Chrome(service=service, options=options)

# ---------- 4. Abrir página ----------
driver.get("https://example.com")                  # Navega a URL
driver.set_page_load_timeout(30)                   # Espera máx 30 s

# ---------- 5. ESCRIBIR texto ----------
campo = driver.find_element(By.ID, "username")
campo.clear()                                      # Limpia antes
campo.send_keys("mi_usuario")                      # Escribe

campo_pwd = driver.find_element(By.NAME, "password")
campo_pwd.send_keys("secreto")                     # Escribe contraseña

# ---------- 6. HACER CLICK ----------
boton_login = driver.find_element(By.CSS_SELECTOR, "button[type='submit']")
boton_login.click()                                # Click simple

# Click con espera explícita
boton = WebDriverWait(driver, 10).until(
    EC.element_to_be_clickable((By.ID, "buy-now"))
)
boton.click()

# ---------- 7. SCROLL ----------
driver.execute_script("window.scrollTo(0, 500);")               # Scroll vertical
elemento = driver.find_element(By.CLASS_NAME, "comments")
driver.execute_script("arguments[0].scrollIntoView();", elemento)

# ---------- 8. EXTRAER DATOS ----------
titulo = driver.title                              # Título de la página
html   = driver.page_source                        # HTML completo
texto  = driver.find_element(By.TAG_NAME, "h1").text  # Texto visible

# Extraer atributo
enlace = driver.find_element(By.LINK_TEXT, "Contacto")
url_contacto = enlace.get_attribute("href")

# ---------- 9. COOKIES ----------
driver.add_cookie({"name": "session", "value": "abc123", "domain": "example.com"})
all_cookies = driver.get_cookies()                 # Lista de dicts
driver.delete_all_cookies()                        # Limpiar

# ---------- 10. SCREENSHOT ----------
driver.save_screenshot("captura.png")              # PNG entera
elemento.screenshot("logo.png")                    # PNG de un solo elemento

# ---------- 11. ACTION CHAINS (drag, hover, doble-click) ----------
actions = ActionChains(driver)
fuente  = driver.find_element(By.ID, "draggable")
destino = driver.find_element(By.ID, "droppable")
actions.drag_and_drop(fuente, destino).perform()  # Drag & Drop

menu = driver.find_element(By.ID, "menu")
actions.move_to_element(menu).perform()            # Hover (mouse encima)

btn = driver.find_element(By.ID, "btn")
actions.double_click(btn).perform()                # Doble-click

# ---------- 12. DROPDOWN ----------
from selenium.webdriver.support.ui import Select
select = Select(driver.find_element(By.ID, "country"))
select.select_by_visible_text("España")            # O select_by_value / select_by_index

# ---------- 13. SUBIR ARCHIVO ----------
upload = driver.find_element(By.CSS_SELECTOR, "input[type='file']")
upload.send_keys(os.path.abspath("cv.pdf"))        # Ruta absoluta

# ---------- 14. JAVA SCRIPT DIRECTO ----------
resultado = driver.execute_script("return document.querySelector('.price').innerText")
driver.execute_script("arguments[0].style.border='3px solid red'", elemento)

# ---------- 15. GUARDAR DATOS EN CSV ----------
quotes = driver.find_elements(By.CLASS_NAME, "quote")
rows = [[q.find_element(By.CLASS_NAME, "text").text,
         q.find_element(By.CLASS_NAME, "author").text] for q in quotes]
with open("quotes.csv", "w", newline='', encoding='utf-8') as f:
    writer = csv.writer(f)
    writer.writerow(["Texto", "Autor"])
    writer.writerows(rows)

# ---------- 16. CERRAR ----------
driver.quit()                                      # Cierra todo
```

Copia el bloque que necesites: cada función está **lista para usar** en cualquier bot headless.
