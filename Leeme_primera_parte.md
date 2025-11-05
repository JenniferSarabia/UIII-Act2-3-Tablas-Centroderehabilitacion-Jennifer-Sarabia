# 🏥 Proyecto: Centro de Rehabilitación – Live Side

### Lenguaje: 🐍 Python
### Framework: 🌐 Django
### Editor: 💻 Visual Studio Code

# 📖 Índice

Configuración Inicial

Creación del Proyecto y Aplicación

Modelos (models.py)

Migraciones

Vistas (views.py)

Templates (HTML)

URLs

Configuración del Proyecto

Panel de Administración

Ejecución del Servidor

Créditos

⚙️ Configuración Inicial
# 1️⃣ Crear carpeta del proyecto
mkdir UIII_Centroderehabilitacion_0368
cd UIII_Centroderehabilitacion_0368

# 2️⃣ Abrir VS Code

Abrir la carpeta UIII_Centroderehabilitacion_0368 desde Visual Studio Code.

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

🚀 Creación del Proyecto y Aplicación
# 8️⃣ Crear el proyecto principal
django-admin startproject backend_centroderehabilitacion .

# 9️⃣ Crear la aplicación principal
python manage.py startapp app_paciente
🧠 Modelos (models.py)

Editar el archivo:
app_paciente/models.py

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
# 🛠️ Migraciones
## 0️⃣ Crear y aplicar migraciones
python manage.py makemigrations
python manage.py migrate

👩‍⚕️ Vistas (views.py)

Editar el archivo:
app_paciente/views.py

    from django.shortcuts import render, redirect, get_object_or_404
    from .models import Paciente
    
    def inicio_live_side(request):
        return render(request, 'inicio.html')
    
    def agregar_paciente(request):
        if request.method == 'POST':
            nom = request.POST.get('nom_pac')
            ape = request.POST.get('ape_pac')
            edad = request.POST.get('edad_pac')
            genero = request.POST.get('genero_pac')
            tel = request.POST.get('tel_pac')
            correo = request.POST.get('correo_pac')
            direccion = request.POST.get('direccion_pac')
            Paciente.objects.create(
                nom_pac=nom, ape_pac=ape, edad_pac=edad, genero_pac=genero,
                tel_pac=tel, correo_pac=correo, direccion_pac=direccion
            )
            return redirect('ver_pacientes')
        return render(request, 'paciente/agregar_paciente.html')
    
    def ver_pacientes(request):
        pacientes = Paciente.objects.all()
        return render(request, 'paciente/ver_pacientes.html', {'pacientes': pacientes})
    
    def actualizar_paciente(request, id):
        paciente = get_object_or_404(Paciente, id=id)
        return render(request, 'paciente/actualizar_paciente.html', {'paciente': paciente})
    
    def realizar_actualizacion_paciente(request, id):
        paciente = get_object_or_404(Paciente, id=id)
        if request.method == 'POST':
            paciente.nom_pac = request.POST.get('nom_pac')
            paciente.ape_pac = request.POST.get('ape_pac')
            paciente.edad_pac = request.POST.get('edad_pac')
            paciente.genero_pac = request.POST.get('genero_pac')
            paciente.tel_pac = request.POST.get('tel_pac')
            paciente.correo_pac = request.POST.get('correo_pac')
            paciente.direccion_pac = request.POST.get('direccion_pac')
            paciente.save()
            return redirect('ver_pacientes')
    
    def borrar_paciente(request, id):
        paciente = get_object_or_404(Paciente, id=id)
        paciente.delete()
        return redirect('ver_pacientes')

# 🎨 Templates (HTML)

Estructura:

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

# 🔹 base.html
    <!DOCTYPE html>
    <html lang="es">
    <head>
        <meta charset="UTF-8">
        <title>Sistema Live Side</title>
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    </head>
    <body>
        {% include 'header.html' %}
        {% include 'navbar.html' %}
        <main class="container mt-4">
            {% block content %}{% endblock %}
        </main>
        {% include 'footer.html' %}
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    </body>
    </html>

# 🔹 header.html
    <header class="bg-light text-center py-3">
        <h2>Centro de Rehabilitación Live Side</h2>
    </header>

# 🔹 navbar.html
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Sistema Live Side</a>
        <div class="collapse navbar-collapse">
          <ul class="navbar-nav ms-auto">
            <li class="nav-item"><a class="nav-link" href="{% url 'inicio_live_side' %}">Inicio</a></li>
            <li class="nav-item dropdown">
              <a class="nav-link dropdown-toggle" data-bs-toggle="dropdown" href="#">Pacientes</a>
              <ul class="dropdown-menu">
                <li><a class="dropdown-item" href="{% url 'agregar_paciente' %}">Agregar Paciente</a></li>
                <li><a class="dropdown-item" href="{% url 'ver_pacientes' %}">Ver Pacientes</a></li>
              </ul>
            </li>
          </ul>
        </div>
      </div>
    </nav>

# 🔹 footer.html
    <footer class="bg-dark text-white text-center py-3 fixed-bottom">
        © <span id="year"></span> Creado por Jennifer Sarabia, CBTis 128
    </footer>
    <script>
      document.getElementById("year").textContent = new Date().getFullYear();
    </script>

# 🔹 inicio.html
    {% extends 'base.html' %}
    {% block content %}
    <div class="text-center">
      <h1>Bienvenido al Sistema Live Side</h1>
      <p>Centro de Rehabilitación Física y Psicológica</p>
      <img src="https://cdn.pixabay.com/photo/2016/11/21/15/58/wheelchair-1840946_1280.jpg" class="img-fluid rounded">
    </div>
    {% endblock %}

# 🔹 paciente/agregar_paciente.html
    {% extends 'base.html' %}
    {% block content %}
    <h2>Agregar Paciente</h2>
    <form method="post">{% csrf_token %}
      <input type="text" name="nom_pac" placeholder="Nombre" class="form-control mb-2">
      <input type="text" name="ape_pac" placeholder="Apellido" class="form-control mb-2">
      <input type="number" name="edad_pac" placeholder="Edad" class="form-control mb-2">
      <input type="text" name="genero_pac" placeholder="Género" class="form-control mb-2">
      <input type="text" name="tel_pac" placeholder="Teléfono" class="form-control mb-2">
      <input type="email" name="correo_pac" placeholder="Correo" class="form-control mb-2">
      <input type="text" name="direccion_pac" placeholder="Dirección" class="form-control mb-2">
      <button class="btn btn-primary">Guardar</button>
    </form>
    {% endblock %}

# 🔹 paciente/ver_pacientes.html
    {% extends 'base.html' %}
    {% block content %}
    <h2>Lista de Pacientes</h2>
    <table class="table table-bordered">
      <thead class="table-dark">
        <tr>
          <th>ID</th>
          <th>Nombre</th>
          <th>Edad</th>
          <th>Teléfono</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
      {% for p in pacientes %}
        <tr>
          <td>{{ p.id }}</td>
          <td>{{ p.nom_pac }} {{ p.ape_pac }}</td>
          <td>{{ p.edad_pac }}</td>
          <td>{{ p.tel_pac }}</td>
          <td>
            <a href="{% url 'actualizar_paciente' p.id %}" class="btn btn-warning btn-sm">Editar</a>
            <a href="{% url 'borrar_paciente' p.id %}" class="btn btn-danger btn-sm">Borrar</a>
          </td>
        </tr>
      {% endfor %}
      </tbody>
    </table>
    {% endblock %}

# 🔹 paciente/actualizar_paciente.html
    {% extends 'base.html' %}
    {% block content %}
    <h2>Actualizar Paciente</h2>
    <form method="post" action="{% url 'realizar_actualizacion_paciente' paciente.id %}">
    {% csrf_token %}
      <input type="text" name="nom_pac" value="{{ paciente.nom_pac }}" class="form-control mb-2">
      <input type="text" name="ape_pac" value="{{ paciente.ape_pac }}" class="form-control mb-2">
      <input type="number" name="edad_pac" value="{{ paciente.edad_pac }}" class="form-control mb-2">
      <input type="text" name="genero_pac" value="{{ paciente.genero_pac }}" class="form-control mb-2">
      <input type="text" name="tel_pac" value="{{ paciente.tel_pac }}" class="form-control mb-2">
      <input type="email" name="correo_pac" value="{{ paciente.correo_pac }}" class="form-control mb-2">
      <input type="text" name="direccion_pac" value="{{ paciente.direccion_pac }}" class="form-control mb-2">
      <button class="btn btn-success">Actualizar</button>
    </form>
    {% endblock %}

# 🔹 paciente/borrar_paciente.html
    {% extends 'base.html' %}
    {% block content %}
    <h2>Eliminar Paciente</h2>
    <p>¿Estás seguro de que deseas eliminar al paciente <b>{{ paciente.nom_pac }} {{ paciente.ape_pac }}</b>?</p>
    <form method="post">{% csrf_token %}
      <button class="btn btn-danger">Sí, eliminar</button>
      <a href="{% url 'ver_pacientes' %}" class="btn btn-secondary">Cancelar</a>
    </form>
    {% endblock %}

🌐 URLs
# 🔹 app_paciente/urls.py
    from django.urls import path
    from . import views
    
    urlpatterns = [
        path('', views.inicio_live_side, name='inicio_live_side'),
        path('agregar/', views.agregar_paciente, name='agregar_paciente'),
        path('ver/', views.ver_pacientes, name='ver_pacientes'),
        path('actualizar/<int:id>/', views.actualizar_paciente, name='actualizar_paciente'),
        path('actualizar_paciente/<int:id>/', views.realizar_actualizacion_paciente, name='realizar_actualizacion_paciente'),
        path('borrar/<int:id>/', views.borrar_paciente, name='borrar_paciente'),
    ]

# ⚙️ Configuración del Proyecto
# 🔹 backend_centroderehabilitacion/settings.py

Asegúrate de registrar tu aplicación:

    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'app_paciente',
    ]

# 🔹 backend_centroderehabilitacion/urls.py
    from django.contrib import admin
    from django.urls import path, include
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('app_paciente.urls')),
    ]

# 🧾 Panel de Administración
# 🔹 app_paciente/admin.py
    from django.contrib import admin
    from .models import Paciente, Terapeuta, Terapia
    
    admin.site.register(Paciente)
    admin.site.register(Terapeuta)
    admin.site.register(Terapia)

# 🖥️ Ejecución del Servidor
## 5️⃣ Ejecutar servidor en puerto 8368
    python manage.py runserver 8368


Abrir en el navegador:
    👉 http://127.0.0.1:8368/

✨ Créditos

Sistema de Administración “Live Side”
📅 Creado por Jennifer Sarabia
🏫 CBTis 128 – Proyecto Django 2025
