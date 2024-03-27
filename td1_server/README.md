# À quoi sert l'uri de la tâche ?
l'uri de la tâche sert à identifier la tâche dans l'API

# Pourquoi faut-il tester les données reçus et leurs types ?
pour éviter les erreurs de type dans le code de l'API et pour éviter les erreurs côté client 
(par exemple, si le client envoie un boolean au lieu d'une chaîne de caractères, l'API doit pouvoir gérer cette erreur)