# 🧪 Guía Completa Comentada de Selenium WebDriver  
Cada línea de código explicada en español paso a paso.

---

## 1. Instalación y configuración inicial (5 min)

| Paso | Acción | Comando / Ejemplo |
|---|---|---|
| 1 | Instalar el driver del navegador | Descarga **ChromeDriver** (v127+) o **GeckoDriver** para Firefox y añádelo al `PATH`. |
| 2 | Añadir dependencia | **Python:** `pip install selenium==4.26.0`  <br>**Java:** Maven/Gradle `org.seleniumhq.selenium:selenium-java:4.26.0` |
| 3 | Verificar versión | `python -c "import selenium; print(selenium.__version__)"` |

```python
# Importamos la clase principal de Selenium para manejar el navegador
from selenium import webdriver
# Importamos Service para indicar la ruta del ChromeDriver
from selenium.webdriver.chrome.service import Service

# Creamos un objeto Service que apunta al ejecutable de ChromeDriver
service = Service("chromedriver.exe")  # o la ruta absoluta

# Instanciamos el navegador Chrome pasándole la configuración del Service
driver = webdriver.Chrome(service=service)

# Abrimos la página web solicitada
driver.get("https://example.com")

# Imprimimos el título de la página en consola para confirmar que cargó
print(driver.title)

# Cerramos completamente el navegador y liberamos recursos
driver.quit()
```

---

## 2. Navegación básica

| Acción | Método | Ejemplo |
|---|---|---|
| Abrir URL | `get()` | `driver.get("https://example.com")` |
| Atrás / Adelante | `back()` / `forward()` | `driver.back()` |
| Actualizar | `refresh()` | `driver.refresh()` |
| Maximizar | `maximize_window()` | `driver.maximize_window()` |
| Cerrar | `quit()` `close()` | `driver.quit()` cierra todo; `close()` la ventana activa |

```python
# Abrimos la página principal
driver.get("https://example.com")

# Vamos a otra página
driver.get("https://google.com")

# Retrocedemos al historial anterior (volvemos a example.com)
driver.back()

# Avanzamos de nuevo en el historial (regresamos a google.com)
driver.forward()

# Recargamos la página actual
driver.refresh()

# Maximizamos la ventana del navegador para mejor visibilidad
driver.maximize_window()
```

---

## 3. Localizadores (locators) – 8 formas

| Tipo | Sintaxis | Ejemplo |
|---|---|---|
| ID | `By.ID` | `driver.find_element(By.ID, "user")` |
| Name | `By.NAME` | `driver.find_element(By.NAME, "email")` |
| Class Name | `By.CLASS_NAME` | `driver.find_element(By.CLASS_NAME, "btn-primary")` |
| Tag Name | `By.TAG_NAME` | `driver.find_element(By.TAG_NAME, "h1")` |
| Link Text | `By.LINK_TEXT` | `driver.find_element(By.LINK_TEXT, "Registro")` |
| Partial Link Text | `By.PARTIAL_LINK_TEXT` | `driver.find_element(By.PARTIAL_LINK_TEXT, "Reg")` |
| CSS Selector | `By.CSS_SELECTOR` | `driver.find_element(By.CSS_SELECTOR, "input[name='q']")` |
| XPath | `By.XPATH` | `driver.find_element(By.XPATH, "//button[text()='Enviar']")` |

💡 **Recomendación:** prioriza CSS Selector (rápido) y XPath (potente).

```python
# Importamos By para usar los localizadores
from selenium.webdriver.common.by import By

# Localizamos por ID el campo de entrada de usuario
campo_usuario = driver.find_element(By.ID, "user")

# Localizamos por NAME el campo de correo electrónico
campo_email = driver.find_element(By.NAME, "email")

# Localizamos por CLASS_NAME el botón principal
boton_enviar = driver.find_element(By.CLASS_NAME, "btn-primary")

# Localizamos por TAG_NAME el primer título h1
titulo = driver.find_element(By.TAG_NAME, "h1")

# Localizamos por LINK_TEXT el enlace exacto que dice "Registro"
enlace_registro = driver.find_element(By.LINK_TEXT, "Registro")

# Localizamos por PARTIAL_LINK_TEXT cualquier enlace que contenga "Reg"
enlace_parcial = driver.find_element(By.PARTIAL_LINK_TEXT, "Reg")

# Localizamos por CSS_SELECTOR usando sintaxis CSS
input_busqueda = driver.find_element(By.CSS_SELECTOR, "input[name='q']")

# Localizamos por XPATH usando sintaxis XPath
boton_enviar2 = driver.find_element(By.XPATH, "//button[text()='Enviar']")
```

---

## 4. Interacciones con elementos

| Acción | Método | Ejemplo completo |
|---|---|---|
| Clic | `.click()` | `driver.find_element(By.ID, "submit").click()` |
| Escribir texto | `.send_keys()` | `driver.find_element(By.NAME, "user").send_keys("admin")` |
| Limpiar campo | `.clear()` | `driver.find_element(By.NAME, "user").clear()` |
| Leer texto | `.text` | `label = driver.find_element(By.TAG_NAME, "h1").text` |
| Obtener atributo | `.get_attribute()` | `href = driver.find_element(By.LINK_TEXT, "Blog").get_attribute("href")` |
| Verificar estado | `.is_enabled()` / `.is_displayed()` / `.is_selected()` | `assert driver.find_element(By.ID, "check").is_selected()` |

```python
# Hacemos clic en el botón de envío
driver.find_element(By.ID, "submit").click()

# Escribimos "admin" en el campo de usuario
driver.find_element(By.NAME, "user").send_keys("admin")

# Limpiamos el campo antes de escribir nuevo texto
driver.find_element(By.NAME, "user").clear()

# Leemos el texto visible del título h1
texto_titulo = driver.find_element(By.TAG_NAME, "h1").text
print(texto_titulo)  # Imprime el contenido del h1

# Obtenemos el valor del atributo href de un enlace
href_blog = driver.find_element(By.LINK_TEXT, "Blog").get_attribute("href")
print(href_blog)  # Imprime la URL del enlace

# Verificamos si un checkbox está seleccionado
esta_seleccionado = driver.find_element(By.ID, "check").is_selected()
print(esta_seleccionado)  # True o False
```

---

## 5. Esperas inteligentes (evita `sleep`)

| Tipo | Código | Uso |
|---|---|---|
| Implícita | `driver.implicitly_wait(10)` | Espera global de 10 s |
| Explícita | `WebDriverWait(driver, 15).until(EC.element_to_be_clickable((By.ID, "ok"))).click()` | Espera hasta que se cumpla una condición |
| Fluent | `Wait(...).until(...)` | Similar a explícita pero configurable en polling |

```python
# Importamos las clases necesarias para esperas explícitas
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# Configuramos una espera implícita global de 10 segundos
driver.implicitly_wait(10)

# Esperamos explícitamente hasta que el botón "ok" sea clickeable (máx 15 seg)
boton_ok = WebDriverWait(driver, 15).until(
    EC.element_to_be_clickable((By.ID, "ok"))
)
boton_ok.click()  # Hacemos clic en el botón una vez que está disponible
```

---

## 6. Dropdowns, checkboxes, radio buttons

```python
# Importamos la clase Select para manejar dropdowns
from selenium.webdriver.support.ui import Select

# Localizamos el dropdown por ID
dropdown = Select(driver.find_element(By.ID, "pais"))

# Seleccionamos la opción por el texto visible
dropdown.select_by_visible_text("España")

# Seleccionamos la opción por el valor del atributo value
dropdown.select_by_value("ES")

# Seleccionamos la opción por su índice (0 es la primera)
dropdown.select_by_index(2)

# Para checkbox y radio button simplemente hacemos clic
driver.find_element(By.ID, "terms").click()
```

---

## 7. Subir archivos

```python
# Localizamos el input de tipo file
upload = driver.find_element(By.CSS_SELECTOR, "input[type='file']")

# Enviamos la ruta absoluta del archivo a subir
# Nota: Selenium no puede interactuar con ventanas del sistema operativo
upload.send_keys(r"C:\docs\cv.pdf")
```

---

## 8. Ventanas, tabs, pop-ups y alerts

| Tarea | Snippet |
|---|---|
| Nueva ventana | `driver.switch_to.new_window('tab')` |
| Cambiar entre ventanas | `driver.switch_to.window(driver.window_handles[-1])` |
| Alert JS | `Alert a = driver.switch_to.alert; a.accept()` |
| Autenticación básica | `driver.get("https://user:pass@example.com")` |

```python
# Abrimos una nueva pestaña en blanco
driver.switch_to.new_window('tab')

# Obtenemos lista de ventanas/pestañas abiertas
handles = driver.window_handles
print(handles)  # Lista de identificadores de ventanas

# Cambiamos al contexto de la última ventana/pestaña abierta
driver.switch_to.window(driver.window_handles[-1])

# Manejamos una alerta de JavaScript
from selenium.webdriver.common.alert import Alert
alert = driver.switch_to.alert  # Cambiamos el foco a la alerta
print(alert.text)  # Imprimimos el texto de la alerta
alert.accept()  # Aceptamos la alerta (clic en OK)

# Para autenticación básica podemos incluir credenciales en la URL
driver.get("https://user:pass@example.com")
```

---

## 9. Frames & iframes

```python
# Entramos al frame por su nombre o ID
driver.switch_to.frame("frameName")

# También podemos entrar por índice (0 es el primer frame)
driver.switch_to.frame(0)

# Para salir del frame y volver al contexto principal de la página
driver.switch_to.default_content()
```

---

## 10. Acciones avanzadas (drag & drop, hover, doble-click)

```python
# Importamos ActionChains para acciones complejas
from selenium.webdriver import ActionChains

# Creamos una instancia de ActionChains con el driver actual
actions = ActionChains(driver)

# Localizamos los elementos involucrados
elem1 = driver.find_element(By.ID, "draggable")  # Elemento a arrastrar
elem2 = driver.find_element(By.ID, "droppable")  # Elemento destino

# Realizamos la acción de arrastrar y soltar
actions.drag_and_drop(elem1, elem2).perform()

# Para hacer hover sobre un elemento
menu = driver.find_element(By.ID, "menu")
actions.move_to_element(menu).perform()  # Movemos el mouse sobre el menú
```

---

## 11. JavaScript y scroll

```python
# Scroll hasta un elemento específico
element = driver.find_element(By.ID, "footer")
driver.execute_script("arguments[0].scrollIntoView();", element)

# Hacer clic via JavaScript cuando WebDriver falla (elemento oculto)
driver.execute_script("arguments[0].click();", element)

# Obtener información con JavaScript
titulo_js = driver.execute_script("return document.title")
print(titulo_js)  # Imprime el título obtenido via JS
```

---

## 12. Screenshots & evidencias

```python
# Captura de pantalla completa
driver.save_screenshot("captura.png")

# Captura de solo un elemento
element.screenshot("button.png")
```

---

## 13. Patrón Page Object Model (POM) – escalabilidad

Estructura recomendada:

```
pages/
    login_page.py
tests/
    test_login.py
```

```python
# pages/login_page.py
class LoginPage:
    def __init__(self, driver):
        self.driver = driver
        # Definimos los localizadores como tuplas (By.ID, "valor")
        self.user = (By.ID, "username")
        self.pwd  = (By.ID, "password")
        self.btn  = (By.ID, "login-button")

    def login(self, u, p):
        # Encuentra el campo usuario y escribe el valor u
        self.driver.find_element(*self.user).send_keys(u)
        # Encuentra el campo contraseña y escribe el valor p
        self.driver.find_element(*self.pwd).send_keys(p)
        # Encuentra el botón y hace clic
        self.driver.find_element(*self.btn).click()
```

---

## 14. Ejercicio integrador (automatizar login + compra)

```python
# Abrimos la URL del sitio demo
driver.get("https://www.saucedemo.com")

# Escribimos el usuario en el campo correspondiente
driver.find_element(By.ID, "user-name").send_keys("standard_user")

# Escribimos la contraseña
driver.find_element(By.ID, "password").send_keys("secret_sauce")

# Hacemos clic en el botón de login
driver.find_element(By.ID, "login-button").click()

# Añadimos al carrito el primer producto
driver.find_element(By.ID, "add-to-cart-sauce-labs-backpack").click()

# Abrimos el carrito haciendo clic en el icono
driver.find_element(By.CLASS_NAME, "shopping_cart_link").click()

# Procedemos al checkout
driver.find_element(By.ID, "checkout").click()

# Rellenamos el formulario de información
driver.find_element(By.ID, "first-name").send_keys("Ana")
driver.find_element(By.ID, "last-name").send_keys("Lopez")
driver.find_element(By.ID, "postal-code").send_keys("28001")
driver.find_element(By.ID, "continue").click()

# Validamos el total de la compra
total = driver.find_element(By.CLASS_NAME, "summary_total_label").text
print(total)  # Imprime el total para verificación
assert total == "Total: $43.18"  # Verificamos que el total sea correcto
```

---

## 15. Buenas prácticas y errores comunes

| Buena práctica | Evita |
|---|---|
| Usa Page Objects | No uses `sleep()` fijo |
| Nombra tests descriptivos | No localices por XPath absoluto |
| Incluye esperas inteligentes | Evita código duplicado |
| Ejecuta en CI (GitHub Actions, Jenkins) | No guardes credenciales en el repo |
| Genera reportes (Allure, pytest-html) | No ignores fallos intermitentes |

```python
# Ejemplo de buena práctica con espera explícita
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# Esperamos hasta que el elemento esté presente en lugar de usar sleep
elemento = WebDriverWait(driver, 10).until(
    EC.presence_of_element_located((By.ID, "mi-elemento"))
)
```

---

## 16. Recursos extra oficiales

- [BrowserStack Selenium Guide](https://www.browserstack.com/guide/selenium-tutorial)   
- [Software Testing Material – Tutorial completo](https://www.softwaretestingmaterial.com/selenium-tutorial/)   
- [HeadSpin – Advanced examples](https://www.headspin.io/blog/selenium-webdriver-tutorial-to-conduct-efficient-webdriver-automation-testing) 

---

Con esta guía ya puedes pasar de principiante a experto en Selenium WebDriver. ¡A automatizar!
