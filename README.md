# Guia-De-Django
Esta es una guía práctica de Django y la teoría puesta aquí es teoría que aprendí en la universidad, para más información consultar la página oficial de Django: https://docs.djangoproject.com/es/4.1/



Para crear un proyecto en Django
Paso 1:Ejecuta en cmd python -m venv "nombre de entorno virtual" para crear el entorno

2:instalas django con : pip install django

3:abres un proyecto: django-admin startproject "nombre"

4:ahora modifica el setting en \nombre\nombre a es y "America/Lima"

si quieres correr servidor: python manage.py runserver

5:luego: python manage.py migrate


Para crear una APP 

tanto los modelos de base de datos como las vistas con apps

Paso1: para crear una app: django-admin startapp "nombre"
o
python manage.py startapp inicio

2: añade en nombre\models\ la estructura de tus columnas
tipos de datos https://docs.djangoproject.com/en/3.2/ref/models/fields/#field-types
crear
https://docs.djangoproject.com/es/3.2/intro/tutorial02/#creating-models
activar
https://docs.djangoproject.com/es/3.2/intro/tutorial02/#activating-models
ejm:
class Paquete(models.Model):
  titulo = models.TextField()
  descripcion = models.TextField()
  descuento = models.IntegerField(default = 0)

3: no olvides añadir tu app a el path de settings

4: no olvides 
python manage.py makemigrations
python manage.py migrate

Listo!


Para administrar

Paso1:python manage.py createsuperuser
Y rellenas los datos

2:Para que tus apps aparescan en admin debes añadir algo como:
from .models import Paquete
admin.site.register(Paquete)

3:Si quieres ver los campos de algo puedes:
python manage.py shell
import app.models import class
class.objects.all()



DJANGO 2

Cambiar tipo de campo

Si quieres empezar de 0 un modelo
Borrar los archivos dentro de “migrations: 00...”, la carpeta “_pycache_” y “db.sqlite3” pero es preferible modificar

Paso1: cambia y luego no te olvides makemigrations y migrate 	namas jeje
atributos de modelos
https://docs.djangoproject.com/en/3.2/ref/models/fields/


Crear una vista

Paso1: creamos una app
python manage.py startapp "vista"

2: añadimos a settings la app

3: editamos vista\views.py añadimos algo como algo como:
from django.http import HttpResponse
def home(*args, **kwargs):
    return HttpResponse('<h1>Bienvenido</h1>')

4: y tenemos que registrar la url en "nombre de proyecto"\urls.py
from inicio.views import home
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', home, name="Casa del Turismo"),

]

Generar Plantillas
NOTA:
? URL es la ruta
? Views es la lógica
? Template es el HTML

Paso1: Crear un archivo que se llame templates donde estaran tus plantillas

2: edita settings.py, añade:
import os
y en donde esta TEMPLATES en DIRS algo como:
        'DIRS': [os.path.join(BASE_DIR, "templates")],

3: Crea un html dentro de templates

4: edita el views.py y en la funcion pon algo como:
def home(request, *args, **kwargs):
  print(args, kwargs)  
  print(request.user)  
  #return HttpResponse('<h1>Bienvenido</h1>') # estoy ya no va
  return render(request, "home.html")

5: No olvides colocar el path de urls.py que esta en la misma carpeta que templates

views tiene argumentos tambien
https://docs.djangoproject.com/en/3.2/ref/request-response/


Como usar variables en el html:

Paso1: elemental mi querido watson, {% es codigo,  {{ es atributo https://docs.djangoproject.com/en/3.2/ref/templates/language/#template-inheritance

2: se mete lo que quieres entre {% %} ejm:
herencia con 
{% extends 'base.html' %} 
para remplazar partes con
{% include 'nav.html' %} 


Algo asi pondriamos en el html que mostraremos
{% extends 'base.html' %}

{% block content %}
{{ request.user }}<br>
{{ request.user.is_authenticated }}
{% endblock %}


3: mira lo que puede hacer
https://docs.djangoproject.com/en/3.2/ref/templates/builtins/#include


Como enviar contexto a nuestro


DJANGO 3

Como pasar Contexto?
en el view.py se puede pasar contexto
en {} se pasa el contexto, lo que es equivaletente a decir un diccionario

def home(request, *args, **kwargs):
  return render(request, 'home.html', {})

def home(request, *args, **kwargs):
  micontexto = {
      'texto' : 'texto de contexto',
      'numero' : 123,
      }
  return render(request, "home.html", micontexto)
  
 y para usarlo en el html se debe usar {{texto}} pero recuerda que solo funciona dentro de un {% block rra %} {%endblock%}
 
 
 SE PUEDE USAR FOR EN Django
 
 supongamos que tenemos en nuestro contexto 
 
   'lista' : [mono, raton, flamenco],

en el html podemos poner

  <ul>
  <h4>Lista con For</h4>
  {% for items in lista %}
  <li>{{items}}</li>
  {% endfor %}
  </ul>

ademas el for tiene atributos como forloop.counter
  <ul>
  <h4>Lista con For</h4>
  {% for items in lista %}
  <li>{{forloop.counter}} - {{items}}</li>
  {% endfor %}
  </ul>

TAMBIEN SE PUEDE USAR IF
 <ul>
  <h4>Lista con For</h4>
  {% for items in lista %}
  <li>{{forloop.counter}} - {{items}}
  {% if forloop.counter == 2 %}
  <h4>este es par</h4>
  {% elif forloop.counter != 0 %}
  <h4>este es impar</h4>
  {% endif %}</li>
  {% endfor %}
  </ul>

TAMBIEN SE PUEDEN USAR FILTROS "FUNCIONES"

https://docs.djangoproject.com/en/3.2/ref/templates/builtins/#built-in-filter-reference

algo como eso
  <li>{{forloop.counter}} - {{items|upper}}



Como examinar los objetos de los modelos de la base de datos?

Paso 1

(Entorno) G:\Archivos benjimy\carpeta del programador\Semestre 3\Programación We
b 2\Proyecto Personal de Turismo usando DJango\Pagina_Turismo>python manage.py s
hell
Python 3.9.13 (tags/v3.9.13:6de2ca5, May 17 2022, 16:36:42) [MSC v.1929 64 bit (
AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)

Paso 2
>>> from PaquetesTuristicos.models import Paquete

Paso 3
>>> Paquete.objects.all()
<QuerySet [<Paquete: Paquete object (1)>, <Paquete: Paquete object (2)>]>

Paso 4
>>> obj = Paquete.objects.get(id =2)

Paso 5	como vemos dir nos muestra todos los atributos y metodos de nuestros objetos
```
>>> dir(obj)
['DoesNotExist', 'MultipleObjectsReturned', '__class__', '__delattr__', '__dict_ _', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__',  '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__ repr__', '__setattr__', '__setstate__', '__sizeof__', '__str__', '__subclasshook __', '__weakref__', '_check_column_name_clashes', '_check_constraints', '_check_ default_pk', '_check_field_name_clashes', '_check_fields', '_check_id_field', '_ check_index_together', '_check_indexes', '_check_local_fields', '_check_long_col umn_names', '_check_m2m_through_same_relationship', '_check_managers', '_check_m odel', '_check_model_name_db_lookup_clashes', '_check_ordering', '_check_propert y_name_related_field_accessor_clashes', '_check_single_primary_key', '_check_swa ppable', '_check_unique_together', '_do_insert', '_do_update', '_get_FIELD_displ ay', '_get_expr_references', '_get_next_or_previous_by_FIELD', '_get_next_or_pre vious_in_order', '_get_pk_val', '_get_unique_checks', '_meta', '_perform_date_ch ecks', '_perform_unique_checks', '_prepare_related_fields_for_save', '_save_pare nts', '_save_table', '_set_pk_val', '_state', 'check', 'clean', 'clean_fields', 'comprado', 'date_error_message', 'delete', 'descripcion', 'descuento', 'from_db ', 'full_clean', 'get_deferred_fields', 'id', 'objects', 'pk', 'prepare_database _save', 'refresh_from_db', 'save', 'save_base', 'serializable_value', 'titulo', 'unique_error_message', 'validate_unique']
```


Como crear un view en una app?

Basicamente es lo mismo que se hizo con la app inicio:D



 
 
 




