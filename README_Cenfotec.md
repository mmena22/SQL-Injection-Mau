# Universidad Cenfotec
**Asignación #3: Desarrollo de aplicaciones seguras**  
**Profesor:** Mario Segura  
**Estudiante:** Mauricio Mena  

---

# SQL Injection Demo

Este proyecto demuestra una vulnerabilidad clásica de **SQL Injection** en una aplicación web desarrollada con **Python + Flask + SQLite**, y cómo solucionarla con buenas prácticas.

## 📁 Estructura del proyecto

```
sql_injection_demo/
├── app.py               # Versión vulnerable
├── app_safe.py          # Versión protegida (2.0)
└── templates/
    └── login.html       # Formulario de inicio de sesión
```

---

## 💣 Versión Vulnerable (`app.py`)

En esta versión, se construyen las consultas SQL directamente con los datos que el usuario envía a través del formulario.  
Esto permite explotar la aplicación mediante SQL Injection.

### Ejemplo de ataque manual:

- Usuario: `' OR '1'='1`
- Contraseña: (cualquier valor)

Resultado: acceso sin credenciales válidas.

### Ejemplo con `sqlmap`:

```bash
python sqlmap.py -u "http://127.0.0.1:5000/login" --data="username=admin&password=123" --dump
```

---

## 🔐 Versión Segura (`app_safe.py`)

En esta versión se aplica:

✅ Parametrización de consultas SQL  
✅ Evita por completo la inyección SQL

```python
c.execute("SELECT * FROM users WHERE username = ? AND password = ?", (username, password))
```

Con esta protección, `sqlmap` ya no puede extraer datos.

---

## 🚀 Cómo ejecutar

1. Instalar Flask:
```bash
pip install flask
```

2. Ejecutar versión vulnerable:
```bash
python app.py
```

3. Ejecutar versión segura:
```bash
python app_safe.py
```

4. Navegar a:
```
http://127.0.0.1:5000
```

---

## 📌 Requisitos

- Python 3.x
- Flask
- Navegador Web
- (Opcional) sqlmap

---

## 📸 Evidencia del ataque

- Base de datos extraída:
```
+----+----------+-----------+
| id | password | username |
+----+----------+-----------+
| 1  | admin123 | admin     |
| 2  | admin123 | admin     |
+----+----------+-----------+
```

> El ataque fue exitoso usando `sqlmap`, y se corrigió en la versión 2.0 usando consultas parametrizadas.

---

## 👨‍💻 Autor

Desarrollado por Mau como parte de una demostración práctica de seguridad ofensiva y defensiva.
