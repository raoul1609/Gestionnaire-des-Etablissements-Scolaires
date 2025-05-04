
FROM python:3.12-slim


ENV PYTHONDONTWRITEBYTECODE 1
# Assure que les sorties Python (comme print) sont envoyées directement au terminal sans buffer
ENV PYTHONUNBUFFERED 1

# Étape 3: Mettre à jour apt et installer les dépendances système (si nécessaire)
# Ajoutez ici les paquets dont votre projet pourrait dépendre
# Exemple pour PostgreSQL: RUN apt-get update && apt-get install -y --no-install-recommends libpq-dev gcc && rm -rf /var/lib/apt/lists/*
# Exemple pour MySQL: RUN apt-get update && apt-get install -y --no-install-recommends default-libmysqlclient-dev build-essential && rm -rf /var/lib/apt/lists/*
# Si vous n'avez pas de dépendances système lourdes, vous pouvez omettre cette étape ou simplement faire:
RUN apt-get update && apt-get install -y --no-install-recommends gcc && rm -rf /var/lib/apt/lists/*


# Étape 4: Définir le répertoire de travail dans le conteneur
WORKDIR /app

# Étape 5: Copier le fichier des dépendances et installer les dépendances Python
# Copier d'abord requirements.txt permet de profiter du cache Docker si ce fichier ne change pas
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# (Optionnel: Si vous utilisez Poetry)
# COPY pyproject.toml poetry.lock* ./
# RUN pip install poetry && \
#    poetry config virtualenvs.create false && \
#    poetry install --no-interaction --no-ansi --no-dev


# Étape 6: Copier tout le code du projet dans le répertoire de travail
COPY . .

# Étape 7: (Optionnel mais courant) Collecter les fichiers statiques
# Décommentez si votre application sert des fichiers statiques ou si vous utilisez collectstatic
# RUN python manage.py collectstatic --noinput

# Étape 8: (Bonne pratique) Créer un utilisateur non-root pour exécuter l'application
# RUN addgroup --system app && adduser --system --ingroup app app
# USER app
# Si vous décommentez ceci, assurez-vous que l'utilisateur 'app' a les permissions nécessaires sur les fichiers/dossiers

# Étape 9: Exposer le port sur lequel Gunicorn va écouter
# Gunicorn écoutera sur ce port à l'intérieur du conteneur
EXPOSE 8000

# Étape 10: Définir la commande pour démarrer l'application avec Gunicorn
# Remplacez 'votre_projet.wsgi:application' par le chemin correct vers votre application WSGI
# votre_projet est généralement le nom du dossier contenant settings.py et wsgi.py
#CMD ["gunicorn", "--bind", "0.0.0.0:8000", "votre_projet.wsgi:application"]

# Alternative pour le développement (NE PAS UTILISER EN PRODUCTION)
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]