# Userflows UX - Espace Documentaire

Date: 2026-05-07

## Objectif UX

L’espace documentaire doit permettre à un utilisateur de trouver rapidement un document accessible selon ses droits, puis à un contributeur ou administrateur de publier, organiser, sécuriser, auditer et restaurer les contenus sans ambiguïté.

Le parcours est pensé du premier usage vers les fonctions les plus avancées : consultation, recherche, ouverture, contribution, administration, sécurité, suivi et maintenance.

## Acteurs

- Lecteur : consulte les dossiers visibles et ouvre les liens ou fichiers autorisés.
- Contributeur : ajoute des documents dans les dossiers où il a au moins le niveau `contribute`.
- Gestionnaire documentaire : crée des sous-dossiers, ajoute des documents et supprime des contenus dans les dossiers où il a le niveau `manage`.
- Administrateur : gère toute la bibliothèque, les dossiers racine, les accès, la corbeille, les statistiques, l’activité et la sécurité module.

## UF-01 - Accéder à l’espace documentaire

Déclencheur : l’utilisateur ouvre le module Documents depuis l’accueil ou la navigation.

Parcours nominal :

1. Le système vérifie que l’utilisateur est authentifié.
2. Le système charge les dossiers racine.
3. Le système filtre les dossiers selon les droits de l’utilisateur.
4. L’utilisateur arrive sur `Bibliothèque documentaire`.
5. Le système affiche la recherche, les filtres par tags et les dossiers visibles.

États UX :

- Si aucun dossier n’est visible : afficher un état vide “Aucun dossier disponible pour votre profil.”
- Si l’utilisateur n’est pas connecté : refuser l’accès.
- Si certains dossiers sont sécurisés : ne jamais afficher les dossiers non autorisés, même grisés.

Succès : l’utilisateur voit uniquement les contenus auxquels il a droit.

## UF-02 - Parcourir les dossiers visibles

Déclencheur : l’utilisateur consulte la bibliothèque sans recherche active.

Parcours nominal :

1. L’utilisateur voit les dossiers racine autorisés.
2. Il identifie un dossier par son nom, sa description et ses éventuels indicateurs de sécurité.
3. Il parcourt les sous-dossiers visibles.
4. Le système masque automatiquement les sous-dossiers non accessibles.
5. L’utilisateur visualise les documents présents dans chaque dossier visible.

États UX :

- Dossier vide : afficher un espace calme plutôt qu’un tableau vide.
- Dossier sécurisé visible : afficher un indicateur discret de protection.
- Sous-dossier inaccessible : ne pas le rendre perceptible dans l’arborescence.

Succès : l’utilisateur comprend où il se trouve et ce qui est consultable.

## UF-03 - Rechercher un document

Déclencheur : l’utilisateur saisit un mot-clé ou sélectionne un ou plusieurs tags.

Parcours nominal :

1. L’utilisateur saisit une recherche dans le champ “Rechercher un document”.
2. Il peut ajouter un ou plusieurs tags.
3. Il lance la recherche.
4. Le système recherche les documents actifs.
5. Le système filtre ensuite les résultats selon les droits de consultation.
6. Les résultats affichent le titre, le dossier parent, les tags et l’action d’ouverture.

États UX :

- Aucun résultat : “Aucun document ne correspond à votre recherche.”
- Recherche sans tag : filtrer seulement par texte.
- Tags sans texte : filtrer par tags.
- Résultat dans dossier non autorisé : ne pas afficher.

Succès : l’utilisateur obtient une liste courte et exploitable, sans fuite d’information sur les dossiers privés.

## UF-04 - Ouvrir un fichier ou un lien

Déclencheur : l’utilisateur clique sur “Ouvrir le fichier” ou “Ouvrir le lien”.

Parcours nominal fichier :

1. L’utilisateur clique sur un document de type fichier.
2. Le système vérifie que le document n’est pas supprimé.
3. Le système vérifie que le dossier parent est consultable.
4. Le système vérifie que le fichier physique existe.
5. Le fichier s’ouvre en lecture inline quand le navigateur le permet.
6. L’activité de téléchargement/consultation est journalisée.

Parcours nominal lien :

1. L’utilisateur clique sur un document de type lien externe ou vidéo.
2. Le lien s’ouvre dans un nouvel onglet sécurisé.

États UX :

- Fichier absent : afficher une erreur “Fichier introuvable.”
- Droits insuffisants : accès refusé.
- Vidéo : le lien doit provenir de YouTube ou Vimeo lors de la création.

Succès : l’utilisateur accède au contenu sans sortir du cadre de sécurité.

## UF-05 - Ajouter un document depuis l’espace utilisateur

Déclencheur : l’utilisateur dispose du droit de contribution sur un dossier.

Parcours nominal :

1. Le système affiche le panneau d’ajout uniquement si `canCreateItem` est vrai.
2. L’utilisateur saisit un titre.
3. Il choisit le type : lien, vidéo ou fichier.
4. Pour un lien ou une vidéo, il saisit une URL.
5. Pour un fichier, il sélectionne un fichier local.
6. Il valide l’ajout.
7. Le système vérifie le jeton CSRF, le titre, le type et la donnée attendue.
8. Le système stocke le fichier ou l’URL.
9. Le système journalise la création.
10. Le système affiche un message de succès.

États UX :

- Titre absent : “Le titre du document est obligatoire.”
- Fichier absent pour type fichier : “Veuillez importer un fichier.”
- URL invalide : “Veuillez saisir un lien valide.”
- Vidéo non YouTube/Vimeo : “Le lien vidéo doit provenir de YouTube ou Vimeo.”

Succès : le nouveau document apparaît dans le dossier cible.

## UF-06 - Créer un sous-dossier depuis l’espace utilisateur

Déclencheur : l’utilisateur dispose du droit de gestion sur un dossier.

Parcours nominal :

1. Le système affiche l’action “Créer un sous-dossier” uniquement si `canManageFolder` est vrai.
2. L’utilisateur saisit le nom du sous-dossier.
3. Il valide.
4. Le système vérifie le jeton CSRF.
5. Le système crée le sous-dossier sous le dossier courant.
6. Le sous-dossier hérite de la visibilité par son parent, sauf configuration future spécifique.
7. Le système journalise la création.

États UX :

- Nom vide : afficher “Le nom du dossier est obligatoire.”
- Droits insuffisants : accès refusé.

Succès : le sous-dossier apparaît dans l’arborescence visible.

## UF-07 - Supprimer un document depuis l’espace utilisateur

Déclencheur : un gestionnaire documentaire clique sur supprimer.

Parcours nominal :

1. Le système affiche l’action seulement si `canDeleteItem` est vrai.
2. L’utilisateur confirme la suppression.
3. Le système vérifie le jeton CSRF.
4. Le document est déplacé dans la corbeille.
5. Le système journalise la suppression.
6. Le système affiche un message de succès.

États UX :

- Annulation de confirmation : aucun changement.
- Droits insuffisants : accès refusé.

Succès : le document disparaît de la bibliothèque active sans être purgé définitivement.

## UF-08 - Administrer la bibliothèque documentaire

Déclencheur : l’administrateur ouvre `Administration > Documents > Bibliothèque`.

Parcours nominal :

1. Le système affiche les tabs du module Documents.
2. L’administrateur voit les dossiers racine si aucun dossier n’est sélectionné.
3. Il ouvre un dossier racine ou un sous-dossier.
4. Le système affiche le fil d’Ariane, les sous-dossiers et les documents du dossier courant.
5. L’administrateur peut créer un dossier, ajouter un document, modifier, supprimer ou configurer les accès selon le contexte.

États UX :

- Dossier demandé introuvable : redirection vers la racine admin avec message d’erreur.
- Aucun dossier racine : encourager la création du premier dossier.

Succès : l’administrateur comprend le niveau courant et les actions possibles.

## UF-09 - Créer un dossier racine ou un sous-dossier en administration

Déclencheur : l’administrateur clique sur créer un dossier.

Parcours nominal :

1. L’administrateur choisit le nom, la description, le parent éventuel et les tags.
2. Pour un dossier racine, il choisit la règle d’accès par rôle si nécessaire.
3. Il valide le formulaire.
4. Le système vérifie la cohérence sécurité.
5. Le système enregistre le dossier et ses tags.
6. Le système journalise la création.
7. L’administrateur revient dans le dossier concerné.

États UX :

- Dossier racine sécurisé sans rôle ni accès utilisateur : prévenir que personne hors admin ne pourra y accéder.
- Parent invalide ou boucle potentielle : bloquer.
- Champs requis manquants : afficher erreurs inline.

Succès : le dossier apparaît au bon niveau avec ses règles de visibilité.

## UF-10 - Ajouter ou modifier un document en administration

Déclencheur : l’administrateur ajoute ou édite un document dans un dossier.

Parcours nominal :

1. L’administrateur choisit le dossier cible.
2. Il renseigne titre, description, type, tags et contenu.
3. Pour un lien externe, il renseigne une URL valide.
4. Pour une vidéo, il renseigne une URL YouTube/Vimeo.
5. Pour un fichier, il importe un fichier local.
6. Il valide.
7. Le système stocke le fichier ou l’URL.
8. Le système synchronise les tags existants et les tags créés à la volée.
9. Le système journalise l’action.

États UX :

- Fichier remplacé en édition : l’ancien fichier doit être retiré du stockage si nécessaire.
- Type changé de fichier vers lien : nettoyer les métadonnées fichier.
- URL invalide : bloquer la soumission.

Succès : le document est disponible dans le dossier cible et dans la recherche.

## UF-11 - Modifier ou supprimer un dossier en administration

Déclencheur : l’administrateur ouvre le menu d’un dossier.

Parcours nominal modification :

1. L’administrateur ouvre “Modifier”.
2. Il ajuste nom, description, parent, sécurité et tags.
3. Le système vérifie la cohérence du parent et des accès.
4. Le système enregistre et journalise.

Parcours nominal suppression :

1. L’administrateur clique sur supprimer.
2. Il confirme.
3. Le système vérifie que l’action est possible.
4. Le dossier est supprimé ou refusé selon les contraintes de contenu.

États UX :

- Dossier avec documents ou enfants : demander une stratégie claire avant suppression définitive.
- Parent descendant sélectionné : bloquer pour éviter une boucle.

Succès : l’arborescence reste cohérente.

## UF-12 - Configurer les accès d’un dossier racine

Déclencheur : l’administrateur ouvre un dossier racine et utilise le panneau d’accès.

Parcours nominal :

1. L’administrateur sélectionne un utilisateur.
2. Il choisit un niveau : lecture, contribution ou gestion complète.
3. Il enregistre.
4. Le système vérifie que le dossier courant est un dossier racine.
5. Le système crée ou met à jour l’accès nominatif.
6. Le système journalise le changement.
7. L’accès s’applique au dossier racine et à ses descendants.

États UX :

- Aucun dossier sélectionné : “Sélectionnez un dossier avant de configurer ses accès.”
- Sous-dossier sélectionné : “Les accès directs se configurent au niveau du dossier racine.”
- Utilisateur absent : “Sélectionnez un utilisateur existant.”

Succès : l’utilisateur concerné accède au dossier avec le bon niveau d’action.

## UF-13 - Retirer un accès nominatif

Déclencheur : l’administrateur retire un accès utilisateur sur un dossier racine.

Parcours nominal :

1. L’administrateur clique sur retirer dans la liste des accès.
2. Le système vérifie le jeton CSRF.
3. Le système vérifie que l’accès appartient bien au dossier courant.
4. Le système journalise le retrait.
5. Le système supprime l’accès.

États UX :

- Accès introuvable : afficher une erreur claire.
- Utilisateur encore autorisé par rôle : indiquer que le retrait nominatif ne retire pas les droits liés aux rôles.

Succès : l’accès nominatif n’est plus actif.

## UF-14 - Consulter l’activité documentaire

Déclencheur : l’administrateur ouvre le tab `Activité`.

Parcours nominal :

1. Le système affiche les événements récents.
2. L’administrateur filtre par action ou par recherche texte.
3. Les résultats montrent l’acteur, l’action, la cible, le dossier et la date.
4. L’administrateur peut analyser les usages ou vérifier une action sensible.

États UX :

- Aucun log : afficher un état vide plutôt qu’un tableau nu.
- Recherche sans résultat : permettre de réinitialiser les filtres.

Succès : l’administrateur peut reconstituer les actions clés.

## UF-15 - Consulter les statistiques documentaires

Déclencheur : l’administrateur ouvre le tab `Statistiques`.

Parcours nominal :

1. Le système affiche les dossiers racine.
2. Il calcule le nombre de documents actifs par racine.
3. Il calcule le volume de stockage.
4. Il affiche l’activité par racine sur la période sélectionnée.
5. L’administrateur ajuste les dates `from` et `to` si besoin.

États UX :

- Période vide : afficher zéro sans erreur.
- Dossier sans activité : conserver la ligne pour comparaison.

Succès : l’administrateur identifie les zones actives et les volumes de stockage.

## UF-16 - Restaurer un document depuis la corbeille

Déclencheur : l’administrateur ouvre le tab `Corbeille`.

Parcours nominal :

1. Le système affiche les documents supprimés.
2. L’administrateur choisit restaurer.
3. Le système vérifie que le document est bien supprimé.
4. Le système vérifie que le dossier d’origine existe.
5. Le document est restauré.
6. Le système affiche un message de succès.

États UX :

- Document non supprimé : afficher une erreur.
- Dossier d’origine introuvable : bloquer la restauration.

Succès : le document redevient visible dans son dossier d’origine.

## UF-17 - Purger définitivement un document

Déclencheur : l’administrateur choisit la suppression définitive dans la corbeille.

Parcours nominal :

1. L’administrateur confirme la purge.
2. Le système vérifie le jeton CSRF.
3. Le fichier stocké est supprimé du disque si présent.
4. L’entité document est supprimée.
5. Le système affiche un message de succès.

États UX :

- Confirmation annulée : aucun changement.
- Fichier physique déjà absent : la purge doit rester possible.

Succès : le document ne peut plus être restauré.

## UF-18 - Configurer la sécurité du module Documents

Déclencheur : l’administrateur ouvre le dernier tab `Sécurité` du module Documents.

Parcours nominal rôles :

1. L’administrateur coche les rôles autorisés pour le module.
2. Il enregistre.
3. Le système met à jour l’accès global au module.

Parcours nominal utilisateurs :

1. L’administrateur sélectionne un utilisateur.
2. Il ajoute un accès nominatif.
3. Le système affiche l’utilisateur dans la liste.
4. L’administrateur peut retirer l’accès nominatif.

États UX :

- Aucun rôle disponible : afficher un état vide.
- Aucun accès nominatif : afficher un état vide.
- Important : l’accès module autorise l’entrée dans le module, mais les dossiers restent filtrés par leurs propres règles.

Succès : l’accès global au module Documents est maîtrisé sans remplacer la sécurité fine des dossiers.

## Ordre de navigation recommandé

1. Documents utilisateur : consulter les dossiers.
2. Documents utilisateur : rechercher et filtrer.
3. Documents utilisateur : ouvrir un fichier ou un lien.
4. Documents utilisateur : contribuer si autorisé.
5. Documents utilisateur : créer un sous-dossier ou supprimer si gestionnaire.
6. Admin Documents : Bibliothèque.
7. Admin Documents : créer/modifier dossiers.
8. Admin Documents : créer/modifier documents.
9. Admin Documents : accès du dossier racine.
10. Admin Documents : Activité.
11. Admin Documents : Statistiques.
12. Admin Documents : Corbeille.
13. Admin Documents : Sécurité module.

## Notes UX prioritaires

- Les droits doivent se traduire par des actions visibles ou invisibles, pas par des boutons désactivés sans explication.
- La recherche ne doit jamais révéler l’existence de documents non autorisés.
- Les accès nominaux de dossier racine et la sécurité globale du module doivent être explicitement distingués.
- Les actions destructives doivent rester confirmées et réversibles quand elles passent par la corbeille.
- Les messages d’erreur doivent être accentués et compréhensibles : “accès”, “sécurité”, “créé”, “supprimé”, “restauré”.
