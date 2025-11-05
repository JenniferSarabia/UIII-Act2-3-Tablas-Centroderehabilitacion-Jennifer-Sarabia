## 🏥 Proyecto: Centro de Rehabilitación

Lenguaje: 🐍 Python
Framework: 🌐 Django
Editor: 💻 Visual Studio Code

⚙️ Procedimiento Inicial

# 1️⃣ Crear carpeta del proyecto
UIII_Centroderehabilitacion_0368

# 2️⃣ Abrir Visual Studio Code

Abrir la carpeta UIII_Centroderehabilitacion_0368 desde VS Code.

# 3️⃣ Abrir la terminal en VS Code

Ir a Terminal → Nueva terminal.

# 4️⃣ Crear entorno virtual .venv
python -m venv .venv

# 5️⃣ Activar entorno virtual
En Windows:
.venv\Scripts\activate

En Mac/Linux:
source .venv/bin/activate

# 6️⃣ Seleccionar intérprete de Python

Presionar Ctrl + Shift + P → Python: Select Interpreter
Seleccionar el intérprete dentro de .venv.

# 7️⃣ Instalar Django
pip install django

# 8️⃣ Crear el proyecto sin duplicar carpeta
django-admin startproject backend_centroderehabilitacion .

# 9️⃣ Ejecutar el servidor en el puerto 8368
python manage.py runserver 8368

# 0️⃣ Copiar el enlace en el navegador
http://127.0.0.1:8368/

🧩 Creación de la Aplicación
# 1️⃣ Crear la aplicación app_paciente
python manage.py startapp app_paciente

🧠 Archivo models.py
# 2️⃣ Crear los modelos de la base de datos

Abrir app_paciente/models.py y copiar lo siguiente:

from django.db import models

# ==========================================
# MODELO: PACIENTE
# ==========================================
class Paciente(models.Model):
    nom_pac = models.CharField(max_length=100)
    ape_pac = models.CharField(max_length=100)
    edad_pac = models.PositiveIntegerField()
    genero_pac = models.CharField(max_length=20)
    tel_pac = models.CharField(max_length=15)
    correo_pac = models.EmailField(max_length=80, unique=True)
    direccion_pac = models.CharField(max_length=120)
    fecha_reg_pac = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f"{self.nom_pac} {self.ape_pac}"


# ==========================================
# MODELO: TERAPEUTA
# ==========================================
class Terapeuta(models.Model):
    nom_tep = models.CharField(max_length=100)
    ape_tep = models.CharField(max_length=100)
    especialidad_tep = models.CharField(max_length=100, choices=[
        ('Física', 'Física'),
        ('Psicológica', 'Psicológica'),
        ('Ocupacional', 'Ocupacional'),
        ('Lenguaje', 'Lenguaje'),
        ('Otra', 'Otra')
    ])
    tel_tep = models.CharField(max_length=15)
    correo_tep = models.EmailField(max_length=80, unique=True)
    horario_tep = models.CharField(max_length=50, default='Lunes a Viernes')
    experiencia_tep = models.PositiveIntegerField(help_text="Años de experiencia")

    def __str__(self):
        return f"{self.nom_tep} {self.ape_tep} - {self.especialidad_tep}"


# ==========================================
# MODELO: TERAPIA
# ==========================================
class Terapia(models.Model):
    nom_ter = models.CharField(max_length=100)
    desc_ter = models.TextField(blank=True, null=True)
    duracion_ter = models.PositiveIntegerField(help_text="Duración en minutos")
    costo_ter = models.DecimalField(max_digits=8, decimal_places=2)
    frecuencia_ter = models.CharField(max_length=50)
    nivel_ter = models.CharField(max_length=50, choices=[
        ('Física', 'Física'),
        ('Psicológica', 'Psicológica'),
        ('Ocupacional', 'Ocupacional'),
        ('Lenguaje', 'Lenguaje')
    ])
    fecha_inicio = models.DateField()
    paciente = models.ForeignKey(Paciente, on_delete=models.CASCADE, related_name="terapias")
    terapeutas = models.ManyToManyField(Terapeuta, related_name="terapias")

    def __str__(self):
        return self.nom_ter

🛠️ Migraciones
# 3️⃣ Crear y aplicar migraciones
python manage.py makemigrations
python manage.py migrate

🧑‍⚕️ Trabajo con el Modelo: PACIENTE
# 4️⃣ Crear funciones en views.py

Agregar las siguientes vistas:

inicio_live_side

agregar_paciente

actualizar_paciente

realizar_actualizacion_paciente

borrar_paciente

🎨 Plantillas HTML
# 5️⃣ Crear la carpeta templates dentro de app_paciente
# 6️⃣ Dentro de templates, crear los archivos:
base.html
header.html
navbar.html
footer.html
inicio.html

# 7️⃣ En base.html

Agregar Bootstrap (CSS y JS) para el diseño y componentes.

# 8️⃣ En navbar.html

Agregar las opciones del sistema:

🔹 Sistema de Administración Live Side
🔹 Inicio
🔹 Pacientes

Agregar Paciente

Ver Pacientes

Actualizar Paciente

Borrar Paciente

🔹 Terapeutas

Agregar Terapeuta

Ver Terapeutas

Actualizar Terapeuta

Borrar Terapeuta

🔹 Terapias

Agregar Terapia

Ver Terapias

Actualizar Terapia

Borrar Terapia

# 9️⃣ En footer.html

Agregar:

Derechos de autor

Fecha del sistema

Texto:

Creado por Jennifer Sarabia, CBTis 128


Hacer que el footer esté fijo al final de la página.

# 0️⃣ En inicio.html

Colocar:

Información del sistema

Una imagen representativa de un centro de rehabilitación

📂 Estructura de Carpetas
## 1️⃣ Estructura final esperada:
app_paciente/
│
├── templates/
│   ├── base.html
│   ├── header.html
│   ├── navbar.html
│   ├── footer.html
│   ├── inicio.html
│   └── paciente/
│       ├── agregar_paciente.html
│       ├── ver_pacientes.html
│       ├── actualizar_paciente.html
│       └── borrar_paciente.html

🌐 Configuraciones
# 2️⃣ Crear urls.py dentro de app_paciente

Enlazar las funciones de views.py para las operaciones CRUD.

# 3️⃣ Registrar app_paciente en settings.py

Agregar:

'app_paciente',


dentro de INSTALLED_APPS.

# 4️⃣ Configurar el urls.py principal del proyecto

Enlazarlo con app_paciente/urls.py.

# 5️⃣ Registrar modelos en admin.py

Y ejecutar nuevamente:

python manage.py makemigrations
python manage.py migrate

💅 Diseño y Estilo
# 6️⃣ Indicaciones visuales

Usar colores suaves y profesionales (azules, grises, blancos).

Diseño limpio y ordenado.

Evitar validaciones complejas.

Crear todas las carpetas antes de correr el servidor.

El sistema debe ser 100% funcional.

🚀 Ejecución Final
## 7️⃣ Ejecutar servidor
python manage.py runserver 8368


Abrir en el navegador:

http://127.0.0.1:8368/

✨ Créditos

Sistema de Administración Live Side
📅 Creado por Jennifer Sarabia | CBTis 128
