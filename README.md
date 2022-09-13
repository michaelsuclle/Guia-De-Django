# Guia-De-Django
Esta es una guía práctica de Django y la teoría puesta aquí es teoría que aprendí en la universidad, para más información consultar la página oficial de Django: https://docs.djangoproject.com/es/4.1/

#

## Indice

1. [Como crear un proyecto en Django](#como-crear-un-proyecto-en-django)
1. [Como correr el Servidor](#como-correr-el-servidor)
1. [Como entrar a el apartado de administrador](#como-entrar-a-el-apartado-de-administrador)

#

1. [Como cambiar el tipo de dato ](#como-cambiar-el-tipo-de-dato)
1. [Como crear una vista](#como-crear-una-vista)
1. [Como generar plantillas](#como-generar-plantillas)
1. [Como usar variables en el html](#como-usar-variables-en-el-html)
1. [Como pasar Contexto a los html(para datos dinamicos)](#como-pasar-contexto-a-los-htmlpara-datos-dinamicos)

#

1. [Se Puede Usar For En Django](#se-puede-usar-for-en-django)
1. [Se Puede Usar If En Django](#se-puede-usar-if-en-django)
1. [Se Pueden Usar Filtros "Funciones" En Django](#se-pueden-usar-filtros-funciones-en-django)
1. [Como examinar los objetos de los modelos de la base de datos?](#como-examinar-los-objetos-de-los-modelos-de-la-base-de-datos)

#

_Nota: Se debe tener instalado python_

_Nota: Esta guia es para Windows_

## Como crear un proyecto en Django
- Paso 1(opcional): Ejecuta en cmd `python -m venv "nombre de entorno virtual"` para crear un entorno virtual

- Paso 2: Instalas django con : `pip install django`

- Paso 3: Abres un proyecto: `django-admin startproject "nombre"`

- Paso 4: Ahora modifica el archivo setting en \nombre\nombre, tienes que cambiar el idioma a `"es"` y y la ubicacion a `"America/Lima"`

- Paso 5: Para migrar lo poco que tienes `python manage.py migrate`

## Como correr el Servidor

Si quieres correr servidor: `python manage.py runserver`

## Como crear crear una APP 

Tanto los modelos de base de datos como las vistas usan apps

Añadiremos un modelo de base de datos basico

- Paso 1: Para crear una app: `django-admin startapp "nombre"`
o
`python manage.py startapp inicio` da lo mismo

- Paso 2: Añade en nombre\models\ la estructura de tus columnas

      tipos de datos
      https://docs.djangoproject.com/en/3.2/ref/models/fields/#field-types
      crear
      https://docs.djangoproject.com/es/3.2/intro/tutorial02/#creating-models
      activar
      https://docs.djangoproject.com/es/3.2/intro/tutorial02/#activating-models
  - Ejemplo:
  En models dentro de "nombre"
  ```py
  class Paquete(models.Model):
    titulo = models.TextField()
    descripcion = models.TextField()
    descuento = models.IntegerField(default = 0)
  ```

- Paso 3: No olvides añadir tu app a el Path de settings

- Paso 4: No olvides 
```shell
python manage.py makemigrations
python manage.py migrate
```

### Listo!


## Como entrar a el apartado de administrador

- Paso1: `python manage.py createsuperuser`

Y rellenas los datos de usuario y contraseña que te aparecerán

- Paso 2: Para que tus apps aparescan en admin debes añadir algo como:
  En el archivo view.py
  ```py
  from .models import Paquete
  admin.site.register(Paquete)
  ```

  Paso 3: Si quieres ver los campos de algo puedes:
  ```shell
  python manage.py shell
  import app.models import class
  class.objects.all()
  ```

#

## Como cambiar el tipo de dato 

_Si quieres empezar de 0 un modelo debes de borrar los archivos dentro de “migrations: 00...”, la carpeta “_pycache_” y “db.sqlite3” pero es preferible modificar_

- Paso 1: Cambia y luego no te olvides `makemigrations` y `migrate` y eso es todo

      atributos de modelos
      https://docs.djangoproject.com/en/3.2/ref/models/fields/


## Como crear una vista

- Paso 1: Creamos una app
`python manage.py startapp "vista"`

- Paso 2: Añadimos a settings la app

- Paso 3: Editamos vista\views.py añadimos algo como algo como:
  ```py
  from django.http import HttpResponse
  def home(*args, **kwargs):
      return HttpResponse('<h1>Bienvenido</h1>')
  ```

- Paso 4: Y tenemos que registrar la url en "nombre de proyecto"\urls.py
  ```py
  from inicio.views import home
  urlpatterns = [
      path('admin/', admin.site.urls),
      path('', home, name="Casa del Turismo"),
  ]
  ```

## Como generar plantillas

    NOTA:
    - URL es la ruta
    - Views es la lógica
    - Template es el HTML

- Paso 1: Crear una carpeta que se llame templates donde estaran tus plantillas

- Paso 2: Edita settings.py, y añade:
`import os`
y en donde esta TEMPLATES en DIRS algo como: 
`'DIRS': [os.path.join(BASE_DIR, "templates")],`

- Paso 3: Crea un html dentro de templates

- Paso 4: Edita el views.py y en la funcion pon algo como:
  ```py
  def home(request, *args, **kwargs):
    print(args, kwargs)  
    print(request.user)  
    #return HttpResponse('<h1>Bienvenido</h1>') # estoy ya no va
    return render(request, "home.html")
  ```

- Paso 5: No olvides colocar el path de urls.py que esta en la misma carpeta que templates

      views tiene argumentos tambien
      https://docs.djangoproject.com/en/3.2/ref/request-response/


## Como usar variables en el html:

- Paso 1: Elemental mi querido watson, {% es codigo,  {{ es atributo 
https://docs.djangoproject.com/en/3.2/ref/templates/language/#template-inheritance

- Paso 2: se mete lo que quieres entre {% %} ejm:

  Para herencia con 
  `{% extends 'base.html' %}`

  Para remplazar partes con
  `{% include 'nav.html' %}` 


Algo asi pondriamos en el html que mostraremos

```html
{% extends 'base.html' %}

{% block content %}
{{ request.user }}<br>
{{ request.user.is_authenticated }}
{% endblock %}
```


Mira lo que puedes hacer
https://docs.djangoproject.com/en/3.2/ref/templates/builtins/#include


#

## Como pasar Contexto a los html(para datos dinamicos)
_En el view.py se puede pasar contexto_

En {} se pasa el contexto, lo que es equivaletente a decir un diccionario

```py
def home(request, *args, **kwargs):
  return render(request, 'home.html', {})
```

```py
def home(request, *args, **kwargs):
  micontexto = {
      'texto' : 'texto de contexto',
      'numero' : 123,
      }
  return render(request, "home.html", micontexto)
```

Y para usarlo en el html se debe usar {{texto}} pero recuerda que solo funciona dentro de un {% block rra %} {%endblock%}
 
 
### Se Puede Usar For En Django
 
  Supongamos que tenemos en nuestro contexto 
    ```py
    'lista' : [mono, raton, flamenco],
    ```

  En el html podemos poner

  ```html
  <ul>
  <h4>Lista con For</h4>
  {% for items in lista %}
  <li>{{items}}</li>
  {% endfor %}
  </ul>
  ```
  Ademas el for tiene atributos como forloop.counter

  ```html
  <ul>
  <h4>Lista con For</h4>
  {% for items in lista %}
  <li>{{forloop.counter}} - {{items}}</li>
  {% endfor %}
  </ul>
  ```
### Se Puede Usar If En Django
  ```html
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
  ```
### Se Pueden Usar Filtros "Funciones" En Django

https://docs.djangoproject.com/en/3.2/ref/templates/builtins/#built-in-filter-reference

Algo como esto
```html
  <li>{{forloop.counter}} - {{items|upper}}<li>
```

## Como examinar los objetos de los modelos de la base de datos?

- Paso 1

_Nota: este es la salida de mi cmd, en vuestro caso puede variar un poco_

Entramos a la consola escribiendo `python manage.py shell` en el cmd

```shell
>>> python manage.py
```

- Paso 2
```shell
>>> from PaquetesTuristicos.models import Paquete
```

- Paso 3
```py
>>> Paquete.objects.all()
<QuerySet [<Paquete: Paquete object (1)>, <Paquete: Paquete object (2)>]>
```

- Paso 4
```py
>>> obj = Paquete.objects.get(id =2)
```

- Paso 5:	Como vemos dir nos muestra todos los atributos y metodos de nuestros objetos
```shell
>>> dir(obj)
['DoesNotExist', 'MultipleObjectsReturned', '__class__', '__delattr__', '__dict_ _', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__',  '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__ repr__', '__setattr__', '__setstate__', '__sizeof__', '__str__', '__subclasshook __', '__weakref__', '_check_column_name_clashes', '_check_constraints', '_check_ default_pk', '_check_field_name_clashes', '_check_fields', '_check_id_field', '_ check_index_together', '_check_indexes', '_check_local_fields', '_check_long_col umn_names', '_check_m2m_through_same_relationship', '_check_managers', '_check_m odel', '_check_model_name_db_lookup_clashes', '_check_ordering', '_check_propert y_name_related_field_accessor_clashes', '_check_single_primary_key', '_check_swa ppable', '_check_unique_together', '_do_insert', '_do_update', '_get_FIELD_displ ay', '_get_expr_references', '_get_next_or_previous_by_FIELD', '_get_next_or_pre vious_in_order', '_get_pk_val', '_get_unique_checks', '_meta', '_perform_date_ch ecks', '_perform_unique_checks', '_prepare_related_fields_for_save', '_save_pare nts', '_save_table', '_set_pk_val', '_state', 'check', 'clean', 'clean_fields', 'comprado', 'date_error_message', 'delete', 'descripcion', 'descuento', 'from_db ', 'full_clean', 'get_deferred_fields', 'id', 'objects', 'pk', 'prepare_database _save', 'refresh_from_db', 'save', 'save_base', 'serializable_value', 'titulo', 'unique_error_message', 'validate_unique']
```


Como crear un view en una app?

Basicamente es lo mismo que se hizo con la app inicio:D



 
 
 




