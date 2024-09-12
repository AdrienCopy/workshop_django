# - Workshop Django - Becode 2024 -

## Créer un projet Django.

### Créer un environnement virtuel.

La plupart des versions récentes de Python viennent avec venv inclus.

Naviguez vers le répertoire où vous souhaitez créer votre projet Django et créez un environnement virtuel.

```
python3 -m venv nom_de_lenv
```
Remplacez nom_de_lenv par le nom que vous souhaitez donner à votre environnement virtuel (par exemple, env).

### Activer l'environnement virtuel.

Une fois l'environnement virtuel créé, vous devez l'activer :

- Sur Windows.
```
nom_de_lenv\Scripts\activate
```

- Sur macOS/Linux.
```
source nom_de_lenv/bin/activate
```

Lorsque l'environnement est activé, vous verrez le nom de l'environnement entre parenthèses dans votre terminal, ce qui indique que vous utilisez maintenant cet environnement virtuel.

Lorsque vous avez terminé de travailler, vous pouvez désactiver l'environnement virtuel en utilisant.
```
deactivate
```

### Installer Django dans l'environnement virtuel.

Avec l'environnement virtuel activé, installez Django.

```
pip install django
```

Pour partager l'environnement de votre projet avec d'autres personnes ou sur un autre serveur, vous pouvez générer un fichier requirements.txt.

```
pip freeze > requirements.txt
```
Cette commande enregistre la sortie de pip freeze dans un fichier nommé requirements.txt. Le fichier requirements.txt contiendra donc une liste de tous les paquets installés et leurs versions.

Voici un exemple de la sortie que vous pourriez obtenir.

```
asgiref==3.8.1
Django==4.2.15
sqlparse==0.5.1
typing_extensions==4.12.2
```

Si vous avez un fichier requirements.txt et que vous souhaitez installer toutes les dépendances qu'il contient, utilisez la commande suivante.

```
pip install -r requirements.txt
```

### Créer votre projet Django.

Une fois Django installé, vous pouvez créer un nouveau projet.

```
django-admin startproject nom_du_projet
```

Remplacez nom_du_projet par le nom que vous souhaitez donner à votre projet.

### Démarrer le serveur de développement.

Entrez dans le répertoire du projet que vous venez de créer.
```
cd nom_du_projet
```
Dans ce répertoire, vous trouverez des fichiers et des dossiers importants comme manage.py et un sous-dossier portant le nom de votre projet.
Vous pouvez maintenant démarrer le serveur de développement pour vérifier que tout fonctionne correctement.
```
python manage.py runserver
```
Vous verrez un message indiquant que le serveur a démarré, ainsi qu'une URL locale, généralement http://127.0.0.1:8000/. Vous pouvez y accéder via votre navigateur pour voir la page de bienvenue de Django.

### Créer une application Django

Pour ajouter des fonctionnalités spécifiques à votre projet, vous devrez créer une ou plusieurs applications Django.

```
python manage.py startapp nom_de_lapplication
```

Remplacez nom_de_lapplication par le nom de l'application que vous voulez créer. Cela génère un dossier avec les fichiers nécessaires pour développer votre application.

### Configurer l'application dans le projet

Pour que Django reconnaisse l'application que vous venez de créer, vous devez l'ajouter au fichier settings.py dans la section INSTALLED_APPS.

```
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "nom_de_lapplication",
]
```

## To-Do List 

###  Définir un modèle pour la tâche

Créez un modèle simple pour représenter une tâche dans models.py de l'application todo.

```python
# todo/models.py
from django.db import models

class Task(models.Model):
    title = models.CharField(max_length=200)
    completed = models.BooleanField(default=False)

    def __str__(self):
        return self.title
```
**_Explication :_**

```python
from django.db import models
```
Cette ligne importe le module models de Django, qui contient des classes de base pour définir des modèles dans Django. Les modèles en Django sont des classes Python qui représentent les tables dans la base de données.

```python
class Task(models.Model):
```
Ici, nous définissons une nouvelle classe Task qui hérite de models.Model. Cela signifie que Task est un modèle Django, et Django le reconnaîtra comme une table dans la base de données. La classe Task va devenir une table appelée "task" par défaut, mais ce nom peut être modifié.

```python
title = models.CharField(max_length=200)
completed = models.BooleanField(default=False)
```

Les deux lignes ci-dessus définissent les champs du modèle Task. Chaque champ devient une colonne dans la table de base de données.

- title : Il s'agit d'un champ de type CharField, qui est utilisé pour stocker des chaînes de caractères (texte). Le paramètre max_length=200 définit la longueur maximale de la chaîne à 200 caractères.

- completed : Il s'agit d'un champ de type BooleanField, qui stocke une valeur booléenne (Vrai ou Faux). Le paramètre default=False signifie que, par défaut, toute nouvelle tâche créée aura la valeur False pour le champ completed, indiquant que la tâche n'est pas encore complétée.


```python
def __str__(self):
    return self.title
```

La méthode __str__ est une méthode spéciale en Python qui définit ce qui est renvoyé lorsque vous appelez str() sur une instance de l'objet ou que vous imprimez l'objet. Dans ce cas, la méthode renvoie le titre de la tâche (self.title). Cela est utile lorsque vous travaillez avec des instances de Task dans la console Django ou dans l'administration Django, car cela rend les objets plus faciles à identifier.


Après avoir défini un modèle Django, comme la classe Task, il est nécessaire de créer et d'appliquer des migrations de la base de données pour que Django puisse créer ou modifier la structure de la base de données en fonction de ces modèles.

## Migrations dans Django

```python
python manage.py makemigrations
python manage.py migrate
```

#### 1. python manage.py makemigrations
Cette commande est utilisée pour créer de nouvelles migrations basées sur les modifications apportées aux modèles.

- **Ce que ça fait :** Lorsque vous exécutez python manage.py makemigrations, Django examine tous les modèles définis dans votre projet (comme le modèle Task dans todo/models.py) et vérifie s'il y a eu des changements depuis la dernière migration. Si Django détecte des changements (comme un nouveau modèle ou des modifications à un modèle existant), il crée un fichier de migration dans le dossier migrations de l'application concernée.

- **Fichier de migration :** Ce fichier de migration contient des instructions Python qui indiquent à Django comment appliquer ces changements à la base de données. Par exemple, si vous ajoutez un nouveau modèle, le fichier de migration contiendra des instructions pour créer une nouvelle table dans la base de données. Si vous modifiez un modèle existant, il contiendra des instructions pour modifier une table existante.

#### 2. python manage.py migrate
Cette commande applique les migrations à la base de données.

- **Ce que ça fait :** Lorsque vous exécutez python manage.py migrate, Django lit les fichiers de migration qui ont été créés (mais pas encore appliqués) et applique les changements à la base de données réelle. Cela signifie que Django va exécuter le SQL nécessaire pour créer ou modifier les tables et autres objets de la base de données selon les instructions des fichiers de migration.

- **Appliquer les changements :** Par exemple, après avoir exécuté makemigrations pour notre modèle Task, Django crée un fichier de migration qui, lorsqu'il est exécuté avec migrate, crée une table task avec les colonnes title et completed.

## Créer des formulaires pour gérer les entrées utilisateur
Créez un formulaire Django pour le modèle Task dans forms.py.
```python
from django import forms
from .models import Task

class TaskForm(forms.ModelForm):
    class Meta:
        model = Task
        fields = ['title', 'completed']

```

**_Explication_**
#### Importation des modules nécessaires

```python
from django import forms
from .models import Task
```
- from django import forms : Cette ligne importe le module forms de Django. Ce module contient des classes et des fonctionnalités pour créer et gérer des formulaires dans Django.

- from .models import Task : Cette ligne importe le modèle Task depuis le fichier models.py de l'application. Ce modèle représente une tâche dans votre base de données.

#### Définition de la classe TaskForm
```python
class TaskForm(forms.ModelForm):
```
- class TaskForm(forms.ModelForm): : Cette ligne définit une nouvelle classe appelée TaskForm, qui hérite de forms.ModelForm. ModelForm est une classe de formulaire spéciale dans Django qui simplifie la création de formulaires liés à des modèles de base de données. En utilisant ModelForm, vous pouvez facilement générer un formulaire basé sur un modèle Django, avec tous les champs nécessaires.
#### Définition de la classe interne Meta
```python
class Meta:
    model = Task
    fields = ['title', 'completed']
```
- class Meta: : La classe interne Meta est une convention en Django utilisée pour fournir des informations supplémentaires à propos du formulaire. Elle permet de spécifier quel modèle ce formulaire représente et quels champs du modèle doivent être inclus dans le formulaire.

- model = Task : Indique que le formulaire TaskForm est lié au modèle Task. Cela signifie que Django va utiliser ce modèle pour générer automatiquement les champs du formulaire.

- fields = ['title', 'completed'] : Spécifie les champs du modèle Task qui doivent être inclus dans le formulaire. Dans ce cas, le formulaire inclura les champs title et completed de l'objet Task. Si vous omettez des champs ou les ajoutez tous, le formulaire comprendra seulement les champs listés ici.
  
## Créer des vues pour gérer les tâches

Créez des vues pour afficher la liste des tâches, ajouter une nouvelle tâche, mettre à jour une tâche existante et supprimer une tâche dans views.py.

```python
# todo/views.py
from django.shortcuts import render, redirect
from .models import Task
from .forms import TaskForm

def index(request):
    tasks = Task.objects.all()
    form = TaskForm()
    if request.method == 'POST':
        form = TaskForm(request.POST)
        if form.is_valid():
            form.save()
        return redirect('/')
    context = {'tasks': tasks, 'form': form}
    return render(request, 'todo/index.html', context)
```

**_Explication_**
#### Importations nécessaires

```python
from django.shortcuts import render, redirect
from .models import Task
from .forms import TaskForm
```
- from django.shortcuts import render, redirect :
  
    - render est une fonction utilitaire de Django utilisée pour générer une réponse HTTP en utilisant un modèle (template). Elle prend la requête, le nom du template, et un dictionnaire de contexte (données à passer au template).
    - redirect est une fonction utilitaire de Django utilisée pour rediriger l'utilisateur vers une autre URL.
      
- from .models import Task : Importe le modèle Task que vous avez défini précédemment dans models.py. Ce modèle représente une tâche dans la base de données.
  
- from .forms import TaskForm : Importe le formulaire TaskForm que vous avez probablement défini dans un fichier forms.py. TaskForm est un formulaire Django lié au modèle Task, qui est utilisé pour créer et modifier des objets Task.

#### Définition de la vue index

```python
def index(request):
```

La fonction index est une vue Django qui prend un paramètre, request. Ce paramètre représente l'objet de requête HTTP qui est passé automatiquement par Django lorsque la vue est appelée.

#### Récupérer toutes les tâches de la base de données

```python
tasks = Task.objects.all()
```
- Task.objects.all() : Utilise le gestionnaire d'objets (objects) du modèle Task pour récupérer tous les objets Task de la base de données. all() retourne un QuerySet contenant toutes les instances de Task.

#### Créer une instance du formulaire TaskForm

```python
form = TaskForm()
```

- TaskForm() : Crée une instance vide du formulaire TaskForm, qui sera utilisée pour permettre à l'utilisateur de saisir les données pour une nouvelle tâche.

#### Gérer les soumissions de formulaire

```python
if request.method == 'POST':
    form = TaskForm(request.POST)
    if form.is_valid():
        form.save()
    return redirect('/')
```

- if request.method == 'POST': : Vérifie si la méthode HTTP de la requête est POST. Cela signifie que l'utilisateur a soumis le formulaire (puisque les formulaires HTML utilisent généralement la méthode POST pour soumettre des données).

- form = TaskForm(request.POST) : Crée une nouvelle instance du formulaire TaskForm en utilisant les données soumises dans la requête (request.POST). request.POST est un dictionnaire contenant toutes les données du formulaire envoyées par l'utilisateur.

- if form.is_valid(): : Vérifie si les données soumises dans le formulaire sont valides (par exemple, tous les champs obligatoires sont remplis, les données sont du bon type, etc.). La méthode is_valid() fait cette vérification.

- form.save() : Si le formulaire est valide, cette méthode sauvegarde les données du formulaire dans la base de données en créant un nouvel objet Task.

- return redirect('/') : Après avoir sauvegardé les données, l'utilisateur est redirigé vers la page d'accueil ('/'). La fonction redirect prend l'URL de redirection comme argument.

#### Préparer le contexte et rendre le template

```python
context = {'tasks': tasks, 'form': form}
return render(request, 'todo/index.html', context)
```


- context = {'tasks': tasks, 'form': form} : Crée un dictionnaire nommé context qui contient les données à envoyer au template. Ici, tasks contient tous les objets Task de la base de données et form est le formulaire TaskForm (vide ou avec des erreurs si le formulaire n'était pas valide).

- return render(request, 'todo/index.html', context) : Utilise la fonction render pour générer une réponse HTTP en utilisant le template todo/index.html et en passant le contexte contenant les tâches et le formulaire.

Défi : Implémenter la fonctionnalité de mise à jour des tâches ! 🚀

<details> <summary>Voir la solution potentielle</summary>



```python
def update_task(request, pk):
    task = Task.objects.get(id=pk)
    form = TaskForm(instance=task)
    if request.method == 'POST':
        form = TaskForm(request.POST, instance=task)
        if form.is_valid():
            form.save()
            return redirect('/')
    context = {'form': form}
    return render(request, 'todo/update_task.html', context)
```
</details>

Défi : Implémenter la fonctionnalité de suppression des tâches ! 🚀

<details> <summary>Voir la solution potentielle</summary>
    
```python
def delete_task(request, pk):
    task = Task.objects.get(id=pk)
    if request.method == 'POST':
        task.delete()
        return redirect('/')
    context = {'task': task}
    return render(request, 'todo/delete_task.html', context)
```
</details>


## Configurer les templates pour les pages HTML

Créez un dossier nommé templates dans le répertoire todo et ajoutez un sous-dossier nommé todo. Créez ensuite trois fichiers HTML : index.html, update_task.html, et delete_task.html.

```
todo/
├── templates/
│   └── todo/
│       └── index.html
│       └── update_task.html
│       └── delete_task.html
├── views.py
├── models.py
└── ...
```


**index.html**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>To-Do List</title>
</head>
<body>
    <h1>To-Do List</h1>
    <form method="POST" action="{% url 'index' %}">
        {% csrf_token %}
        {{ form }}
        <button type="submit">Add Task</button>
    </form>
    <ul>
        {% for task in tasks %}
        <li>
            {{ task.title }}
            {% if task.completed %}
                (Completed)
            {% endif %}
            <a href="{% url 'update_task' task.id %}">Edit</a>
            <a href="{% url 'delete_task' task.id %}">Delete</a>
        </li>
        {% endfor %}
    </ul>
</body>
</html>
```
**update_task.html**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Update Task</title>
</head>
<body>
    <h1>Update Task</h1>
    <form method="POST" action="">
        {% csrf_token %}
        {{ form }}
        <button type="submit">Update</button>
    </form>
</body>
</html>
```
**delete_task.html**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Delete Task</title>
</head>
<body>
    <h1>Are you sure you want to delete "{{ task.title }}"?</h1>
    <form method="POST" action="">
        {% csrf_token %}
        <button type="submit">Yes, Delete</button>
    </form>
    <a href="/">Cancel</a>
</body>
</html>
```

## Définir les URLs pour mapper les vues

Configurez les URLs dans urls.py dans le dossier du projet.

```python
from django.contrib import admin
from django.urls import path
import todo.views

urlpatterns = [
    path("admin/", admin.site.urls),
    path('', todo.views.index, name='index'),
    path('todo/update_task/<str:pk>/', todo.views.update_task, name='update_task'),
    path('todo/delete_task/<str:pk>/', todo.views.delete_task, name='delete_task'),
]
```

**_Explication_**

#### Importation des modules nécessaires

```python
from django.contrib import admin
from django.urls import path
import todo.views
```

- from django.contrib import admin : Importe le module d'administration de Django. Ce module est utilisé pour gérer le site d'administration de Django, qui est une interface graphique permettant de gérer les données de votre application.

- from django.urls import path : Importe la fonction path du module django.urls. path est utilisé pour définir des routes URL et les associer à des vues spécifiques.

- import todo.views : Importe le module views de l'application todo. Ce module contient les fonctions de vue (comme index, update_task, delete_task) que vous avez définies dans votre application. Ces vues déterminent la logique à exécuter lorsqu'une URL spécifique est demandée.

#### Définition du urlpatterns 

```python
urlpatterns = [
    path("admin/", admin.site.urls),
    path('', todo.views.index, name='index'),
    path('todo/update_task/<str:pk>/', todo.views.update_task, name='update_task'),
    path('todo/delete_task/<str:pk>/', todo.views.delete_task, name='delete_task'),
]
```

- urlpatterns est une liste de routes URL que Django utilise pour faire correspondre les URLs demandées par les utilisateurs aux vues correspondantes.

#### Route pour le site d'administration de Django

```python
path("admin/", admin.site.urls),
```
- path("admin/", admin.site.urls) : Cette ligne associe l'URL "admin/" à l'interface d'administration par défaut de Django. Lorsque vous accédez à http://yourdomain.com/admin/, Django charge le site d'administration, où vous pouvez gérer vos modèles et données.

#### Route pour la vue index

```python
path('', todo.views.index, name='index'),
```
- path('', todo.views.index, name='index') :
  
    - '' : La chaîne vide '' représente l'URL racine du site web. Cela signifie que lorsque les utilisateurs visitent http://yourdomain.com/, Django exécutera la vue index de l'application todo.
      
    -  todo.views.index : Indique que Django doit appeler la fonction index définie dans todo/views.py pour gérer cette URL.
      
    - name='index' : Attribue un nom à cette route URL. Les noms d'URL sont utiles pour créer des liens internes dans les templates Django, car ils permettent de faire référence aux routes URL de manière symbolique plutôt que d'utiliser des URLs statiques.

#### Route pour la vue update_task

```python
path('todo/update_task/<str:pk>/', todo.views.update_task, name='update_task'),
```

- path('todo/update_task/<str:pk>/', todo.views.update_task, name='update_task') :
    - 'todo/update_task/<str:pk>/' : Ce chemin d'URL inclut une partie dynamique <str:pk>. Ici, <str:pk> capture un segment de l'URL comme une variable pk (primary key), qui est passée à la vue update_task en tant qu'argument. str indique que pk doit être une chaîne de caractères.
  
    - todo.views.update_task : Indique que Django doit appeler la fonction update_task définie dans todo/views.py pour gérer cette URL.

    - name='update_task' : Attribue un nom à cette route URL, ce qui permet de la référencer dans les templates et le code Python.


## Gestion des fichiers CSS

Django vous permet de centraliser et de gérer les fichiers CSS (ainsi que les autres fichiers statiques) dans des répertoires spécifiques.

- Répertoires statiques des applications : Chaque application Django peut avoir son propre répertoire static, où vous pouvez placer les fichiers CSS spécifiques à cette application.
  
- Répertoire statique global : Vous pouvez également avoir un répertoire static global au niveau du projet, souvent utilisé pour les fichiers CSS, JS ou images communs à plusieurs applications.

#### Pour notre app todo 

Crée le dossier static, todo et le fichier style.css

```
todo/
├── static/
│   └── todo/
│       └── style.css
├── templates/
├── views.py
├── models.py
└── ...

```
#### Paramétrage des fichiers statiques dans settings.py

Dans votre fichier settings.py, vous devez configurer certains paramètres pour que Django sache où trouver les fichiers statiques.

```python
STATIC_URL = '/static/'
```

- STATIC_URL : C'est l'URL de base où les fichiers statiques seront servis. Pendant le développement.
  

#### Utilisation des fichiers CSS dans les templates

Pour utiliser un fichier CSS dans un template Django, vous devez d'abord charger les fichiers statiques en utilisant la balise {% load static %} et ensuite l'inclure dans le template.

Voici comment vous pourriez structurer un template HTML pour inclure un fichier CSS.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Django App</title>
    {% load static %}
    <link rel="stylesheet" href="{% static 'myapp/style.css' %}">
</head>
<body>
    <h1>Bienvenue sur ma page Django</h1>
</body>
</html>
```

## Créer un superutilisateur et afficher l'app todo (dans l'interface d'administration de Django)

Assurez-vous que vous êtes dans le répertoire de votre projet Django, où se trouve le fichier manage.py.

#### Exécutez la commande suivante pour créer un superutilisateur

```
python manage.py createsuperuser
```

#### Saisir les informations du superutilisateur

Username, Email address, Password et Password (again) !

Une fois les informations fournies, Django créera le superutilisateur. Vous verrez un message de confirmation indiquant que le superutilisateur a été créé avec succès.

Vous pouvez maintenant utiliser ce superutilisateur pour vous connecter à l'interface d'administration de Django à l'adresse http://127.0.0.1:8000/admin/

#### Afficher l'application todo dans l'interface d'administration

- Ouvrir le fichier admin.py de l'application todo
- Enregistrer le modèle Task dans l'admin

```python
from django.contrib import admin
from .models import Task

@admin.register(Task)
class TaskAdmin(admin.ModelAdmin):
    list_display = ('title', 'completed')
```

