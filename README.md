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

def delete_task(request, pk):
    task = Task.objects.get(id=pk)
    if request.method == 'POST':
        task.delete()
        return redirect('/')
    context = {'task': task}
    return render(request, 'todo/delete_task.html', context)
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



