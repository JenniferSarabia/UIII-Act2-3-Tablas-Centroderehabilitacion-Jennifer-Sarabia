🏥 Proyecto: Centro de Rehabilitación

Lenguaje: Python
Framework: Django
Editor: Visual Studio Code

⚙️ Procedimiento Inicial

Crear carpeta del proyecto llamada:

UIII_Centroderehabilitacion_0368


Abrir VS Code sobre la carpeta UIII_Centroderehabilitacion_0368.

Abrir la terminal integrada en VS Code.

Crear el entorno virtual llamado .venv desde la terminal:

python -m venv .venv


Activar el entorno virtual:

En Windows:

.venv\Scripts\activate


En Mac/Linux:

source .venv/bin/activate


Activar el intérprete de Python en VS Code (Ctrl + Shift + P → Python: Select Interpreter).

Instalar Django:

pip install django


Crear el proyecto sin duplicar carpeta:

django-admin startproject backend_centroderehabilitacion .


Ejecutar el servidor en el puerto 8368:

python manage.py runserver 8368


Copiar y pegar el link en el navegador para comprobar el funcionamiento.

http://127.0.0.1:8368/

🧩 Crear Aplicación

Crear la aplicación llamada app_paciente:

python manage.py startapp app_paciente

🧠 Archivo models.py
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

    # Relación 1 a muchos: un paciente puede tener muchas terapias
    paciente = models.ForeignKey(Paciente, on_delete=models.CASCADE, related_name="terapias")

    # Relación muchos a muchos: una terapia puede tener varios terapeutas y viceversa
    terapeutas = models.ManyToManyField(Terapeuta, related_name="terapias")

    def __str__(self):
        return self.nom_ter

🛠️ Migraciones

12.5. Realizar las migraciones:

python manage.py makemigrations
python manage.py migrate

🧑‍⚕️ Trabajando con el Modelo PACIENTE

En el archivo views.py de la aplicación app_paciente, crear las funciones:

inicio_live_side

agregar_paciente

actualizar_paciente

realizar_actualizacion_paciente

borrar_paciente

Crear la carpeta templates dentro de app_paciente.

Dentro de templates, crear los siguientes archivos HTML:

base.html
header.html
navbar.html
footer.html
inicio.html


En base.html, agregar Bootstrap para CSS y JS.

En navbar.html, incluir las opciones:

Sistema de Administración Live Side

Inicio

Pacientes

Agregar Paciente

Ver Pacientes

Actualizar Paciente

Borrar Paciente

Terapeutas

Agregar Terapeuta

Ver Terapeutas

Actualizar Terapeuta

Borrar Terapeuta

Terapias

Agregar Terapia

Ver Terapias

Actualizar Terapia

Borrar Terapia

En footer.html, incluir:

Derechos de autor

Fecha del sistema

Texto: Creado por Jennifer Sarabia, CBTis 128

Debe mantenerse fijo al final de la página.

En inicio.html, incluir información del sistema y una imagen sobre el centro de rehabilitación (tomada de la red).

📂 Estructura de Archivos y Carpetas
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

🌐 Configuración y Enlaces

Crear el archivo urls.py dentro de app_paciente y enlazar las funciones de views.py para permitir las operaciones CRUD.

Registrar app_paciente en INSTALLED_APPS dentro del archivo settings.py.

Enlazar las rutas en el urls.py principal del proyecto (backend_centroderehabilitacion/urls.py).

Registrar los modelos en admin.py y volver a realizar migraciones.

🎨 Diseño y Funcionalidad

Utilizar colores suaves, atractivos y modernos.

Las páginas deben ser sencillas y funcionales.

No validar entrada de datos (por el momento).

Al inicio, crear la estructura completa de carpetas y archivos.

Proyecto totalmente funcional.

🚀 Ejecución Final

Ejecutar el servidor en el puerto 8368:

python manage.py runserver 8368
