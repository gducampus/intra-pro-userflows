# Userflows UX - Génération enfant

Date: 2026-05-07

## Objectif UX

Le module Génération enfant doit permettre à l'équipe enfance de structurer les clubs, gérer les fiches enfants, sécuriser les consentements, suivre les présences, communiquer avec les familles et organiser des événements gratuits sans exposer inutilement les données sensibles des mineurs.

Le parcours est pensé du pilotage vers l'opérationnel terrain : clubs, fiches enfants, consentements, présences, notifications, événements, vue d'ensemble, confidentialité et passerelle pastorale minimale.

## Acteurs

- Administrateur : configure le module, voit l'ensemble des clubs, fiches, présences, événements, notifications et statistiques.
- Responsable de pôle enfance : supervise les clubs de son périmètre, suit les effectifs, les présences, les communications et les points sensibles.
- Responsable de club : gère son club, consulte les fiches utiles, suit les présences, prépare les événements et communique avec les familles concernées.
- Équipier enfance : consulte uniquement les informations nécessaires à l'encadrement et saisit les présences ou remarques autorisées.
- Parent ou responsable légal : fournit les informations de l'enfant, les coordonnées de contact et les consentements.
- Système : applique les règles d'âge, de rattachement, de consentement, de visibilité, de traçabilité et de minimisation des données.

## UF-01 - Accéder au module Génération enfant

Déclencheur : l'utilisateur ouvre le module Génération enfant depuis l'administration ou la navigation autorisée.

Parcours nominal :

1. Le système vérifie que l'utilisateur est authentifié.
2. Le système vérifie que l'utilisateur dispose d'un accès au module.
3. Le système détermine son périmètre : tous les clubs, pôle, club rattaché ou simple consultation.
4. L'utilisateur arrive sur l'onglet autorisé par défaut.
5. Le système affiche uniquement les données et actions compatibles avec ses droits.

États UX :

- Utilisateur non connecté : refuser l'accès.
- Droit module absent : afficher un refus clair.
- Aucun club visible : afficher “Aucun club disponible pour votre profil.”
- Périmètre restreint : ne jamais afficher les clubs ou enfants hors périmètre, même grisés.

Succès : l'utilisateur voit uniquement les clubs, enfants et actions utiles à son rôle.

## UF-02 - Consulter la vue d'ensemble

Déclencheur : l'utilisateur ouvre l'onglet `Vue d'ensemble`.

Parcours nominal :

1. Le système affiche les compteurs clés : clubs actifs, enfants actifs, présences récentes et inscriptions.
2. L'utilisateur identifie les volumes importants et les zones à vérifier.
3. Le système met en avant les informations opérationnelles sans afficher de données sensibles inutiles.
4. L'utilisateur rejoint le bon onglet pour agir : clubs, fiches, présences, événements ou notifications.

États UX :

- Aucune donnée : afficher des compteurs à zéro sans erreur.
- Données incomplètes : signaler discrètement les fiches ou consentements à compléter.
- Accès restreint : calculer les statistiques uniquement sur le périmètre autorisé.

Succès : le responsable comprend rapidement l'activité du module et sait où agir.

## UF-03 - Consulter les clubs

Déclencheur : l'utilisateur ouvre l'onglet `Clubs`.

Parcours nominal :

1. Le système affiche les clubs visibles.
2. Chaque club présente son nom, son pôle, sa tranche d'âge, son horaire, sa capacité et son statut.
3. L'utilisateur identifie les clubs actifs et leur organisation.
4. Les actions de création ou modification sont visibles selon les droits.

États UX :

- Aucun club : afficher un état vide invitant à créer le premier club si l'utilisateur est autorisé.
- Club inactif : le rendre identifiable sans le mélanger aux clubs opérationnels.
- Club hors périmètre : ne pas l'afficher.

Succès : l'organisation des groupes d'enfants est claire et exploitable.

## UF-04 - Créer ou modifier un club

Déclencheur : un administrateur ou responsable autorisé crée ou édite un club.

Parcours nominal :

1. L'utilisateur renseigne le nom du club.
2. Il choisit le pôle, la tranche d'âge, les horaires et la capacité.
3. Le système vérifie la cohérence des âges et du rattachement.
4. Le club est enregistré.
5. Le système affiche un message de succès et revient à la liste des clubs.

États UX :

- Nom absent : afficher “Le nom du club est obligatoire.”
- Tranche d'âge incohérente : bloquer si l'âge minimum dépasse l'âge maximum.
- Pôle absent : proposer `Enfance` par défaut si la règle métier le permet.
- Capacité non renseignée : autoriser si la capacité est optionnelle.

Succès : le club apparaît au bon niveau avec des paramètres compréhensibles.

## UF-05 - Consulter les fiches enfants

Déclencheur : l'utilisateur ouvre l'onglet `Fiches enfants`.

Parcours nominal :

1. Le système affiche les enfants visibles dans le périmètre de l'utilisateur.
2. Chaque ligne présente l'identité, le club, le responsable légal, les informations critiques et le statut.
3. Les allergies ou alertes importantes sont mises en évidence.
4. L'utilisateur ouvre ou crée une fiche selon ses droits.

États UX :

- Aucun enfant : afficher “Aucune fiche enfant enregistrée.”
- Fiche inactive ou archivée : ne pas la mélanger aux fiches actives.
- Donnée sensible : ne l'afficher que si elle est utile au rôle courant.
- Club absent : signaler que l'enfant doit être rattaché.

Succès : l'équipe retrouve rapidement une fiche et les informations nécessaires à l'encadrement.

## UF-06 - Créer une fiche enfant

Déclencheur : un administrateur, responsable ou parent via formulaire autorisé crée une fiche enfant.

Parcours nominal :

1. L'utilisateur saisit le prénom, le nom et la date de naissance de l'enfant.
2. Il renseigne le responsable légal principal et ses coordonnées.
3. Il saisit les allergies ou informations de sécurité.
4. Il sélectionne ou confirme le club de rattachement.
5. Il renseigne les consentements requis.
6. Le système vérifie les champs obligatoires.
7. La fiche est créée et associée au club.

États UX :

- Identité enfant incomplète : afficher les erreurs inline.
- Responsable légal absent : bloquer la création si le contact est obligatoire.
- Date de naissance absente : permettre un rattachement manuel ou demander la date selon la règle retenue.
- Allergie renseignée : l'information doit être visible dans les écrans terrain autorisés.

Succès : l'enfant est inscrit avec les informations utiles et sécurisées.

## UF-07 - Gérer les consentements

Déclencheur : l'utilisateur crée ou met à jour une fiche enfant.

Parcours nominal :

1. Le système affiche les consentements requis : RGPD, droit à l'image et participation aux activités spécifiques si applicable.
2. Le parent ou l'utilisateur autorisé renseigne les choix.
3. Le système enregistre la date et la version du consentement si disponible.
4. Toute modification est historisée.
5. Les fonctionnalités dépendantes du consentement deviennent disponibles ou restent bloquées.

États UX :

- Consentement RGPD absent : empêcher l'activation complète de la fiche.
- Droit à l'image refusé : ne jamais inclure l'enfant dans des usages image.
- Consentement expiré ou à renouveler : afficher une alerte claire.
- Modification sensible : confirmer et tracer l'acteur.

Succès : l'équipe sait exactement ce qui est autorisé pour chaque enfant.

## UF-08 - Affecter ou corriger le club d'un enfant

Déclencheur : une fiche enfant est créée ou son âge change.

Parcours nominal :

1. Le système propose un club selon l'âge ou la date de naissance.
2. L'utilisateur vérifie le rattachement.
3. Il confirme ou choisit un autre club si un cas particulier le justifie.
4. Le système enregistre le club courant.
5. Le changement est historisé.

États UX :

- Aucun club compatible : demander une affectation manuelle.
- Changement manuel : demander une justification si nécessaire.
- Club hors périmètre : ne pas le proposer.

Succès : chaque enfant est rattaché à un club cohérent et traçable.

## UF-09 - Saisir les présences

Déclencheur : un équipier ou responsable ouvre l'onglet `Présences` pour un créneau.

Parcours nominal :

1. L'utilisateur choisit le club, la date et le créneau.
2. Le système affiche les enfants attendus dans ce club.
3. L'utilisateur marque les enfants présents.
4. Il ajoute si besoin une remarque logistique, comportementale ou un incident.
5. Le système enregistre la session et les présences.
6. Les statistiques sont mises à jour.

États UX :

- Aucun enfant dans le club : afficher un état vide utile.
- Enfant de passage : permettre l'ajout si la règle métier l'autorise.
- Information critique : afficher allergies et alertes avant validation.
- Double saisie : éviter les présences dupliquées sur un même créneau.

Succès : la présence est enregistrée rapidement avec les informations de sécurité utiles.

## UF-10 - Consulter l'historique des présences

Déclencheur : un responsable consulte les présences d'un club ou d'un enfant.

Parcours nominal :

1. Le système affiche les sessions récentes.
2. L'utilisateur filtre par club, enfant, date ou créneau.
3. Les présences, absences, remarques et incidents autorisés sont visibles.
4. L'utilisateur peut analyser la régularité ou préparer un suivi.

États UX :

- Aucun historique : afficher “Aucune présence enregistrée.”
- Remarque sensible : limiter l'affichage aux rôles autorisés.
- Période sans activité : afficher zéro sans erreur.

Succès : le responsable suit la participation sans exposer plus d'informations que nécessaire.

## UF-11 - Créer un événement gratuit

Déclencheur : un responsable ou administrateur crée un événement depuis l'onglet `Événements`.

Parcours nominal :

1. L'utilisateur choisit le club concerné.
2. Il renseigne le titre, la date, la description et le statut de publication.
3. Le système vérifie que l'événement est gratuit.
4. L'événement est enregistré.
5. S'il est publié, il devient visible pour le public autorisé.

États UX :

- Titre absent : afficher “Le titre de l'événement est obligatoire.”
- Date absente : autoriser le brouillon ou bloquer selon la règle retenue.
- Paiement : aucun champ de paiement ne doit être proposé.
- Club hors périmètre : ne pas proposer le club.

Succès : l'événement gratuit est prêt à être communiqué aux familles concernées.

## UF-12 - Gérer les inscriptions à un événement

Déclencheur : un événement publié accepte des inscriptions.

Parcours nominal :

1. Le parent ou responsable légal ouvre le formulaire d'inscription autorisé.
2. Le système affiche uniquement les événements pertinents pour le club de l'enfant.
3. L'inscription est saisie et validée.
4. Le système enregistre le participant.
5. Le responsable consulte la liste des inscrits.

États UX :

- Événement non publié : ne pas afficher le formulaire.
- Enfant hors club concerné : refuser l'inscription.
- Inscription déjà existante : éviter les doublons.
- Liste exportée : appliquer les droits et limiter les données sensibles.

Succès : les participations sont structurées et exploitables pour l'organisation.

## UF-13 - Créer une notification

Déclencheur : un responsable ou administrateur crée une communication depuis l'onglet `Notifications`.

Parcours nominal :

1. L'utilisateur choisit le club, le type de notification et la cible.
2. Il renseigne le titre et le message.
3. Le système vérifie le périmètre et les destinataires.
4. La notification est enregistrée.
5. Le système conserve l'historique de communication.

États UX :

- Titre ou message absent : afficher une erreur inline.
- Cible trop large : prévenir si la notification dépasse le club courant.
- Aucun destinataire : afficher “Aucun destinataire disponible.”
- Donnée sensible dans le message : inviter à reformuler si une modération existe.

Succès : la communication est ciblée, traçable et compréhensible.

## UF-14 - Consulter l'historique des notifications

Déclencheur : l'utilisateur ouvre l'onglet `Notifications`.

Parcours nominal :

1. Le système affiche les notifications récentes.
2. L'utilisateur voit le club, le type, la cible, le titre et la date.
3. Il peut vérifier les communications déjà envoyées ou préparées.
4. Le système limite la liste au périmètre autorisé.

États UX :

- Aucune notification : afficher un état vide.
- Notification ancienne : conserver l'historique sans surcharger l'écran.
- Notification hors périmètre : ne pas l'afficher.

Succès : les communications restent auditables sans confusion.

## UF-15 - Partager le strict nécessaire avec le suivi pastoral

Déclencheur : un enfant de l'âge concerné est éligible à une continuité pastorale.

Parcours nominal :

1. Le système vérifie l'âge, le statut actif et les consentements applicables.
2. Le système prépare une projection minimale des données utiles.
3. L'utilisateur autorisé confirme le partage si une action humaine est requise.
4. Le système transmet uniquement les données nécessaires.
5. Le partage est journalisé.

États UX :

- Consentement absent ou refusé : bloquer le partage.
- Enfant non éligible : ne pas proposer l'action.
- Données sensibles non nécessaires : les exclure de la projection.
- Droits insuffisants : masquer l'action.

Succès : la passerelle pastorale respecte la confidentialité et la minimisation des données.

## UF-16 - Archiver ou désactiver une fiche enfant

Déclencheur : un enfant quitte le club, les consentements ne sont pas renouvelés ou une demande RGPD est traitée.

Parcours nominal :

1. L'utilisateur autorisé ouvre la fiche enfant.
2. Il choisit l'archivage, la désactivation ou le traitement RGPD approprié.
3. Le système explique les conséquences.
4. L'utilisateur confirme.
5. La fiche est retirée des listes actives.
6. L'action est historisée.

États UX :

- Action destructive : demander confirmation.
- Données liées : expliquer ce qui est conservé, supprimé ou anonymisé.
- Fiche désactivée : empêcher son utilisation opérationnelle.
- Droits insuffisants : masquer l'action.

Succès : les données restent conformes sans polluer les listes actives.

## UF-17 - Configurer les droits du module

Déclencheur : l'administrateur ouvre la sécurité du module Génération enfant.

Parcours nominal :

1. L'administrateur choisit les rôles autorisés.
2. Il définit les accès aux clubs, fiches enfants, présences, événements, notifications, exports et statistiques.
3. Il enregistre la configuration.
4. Le système applique les droits côté serveur et côté interface.
5. Les actions visibles reflètent immédiatement les droits réels.

États UX :

- Aucun rôle disponible : afficher un état vide.
- Accès trop large aux données enfants : prévenir avant validation.
- Utilisateur rattaché à aucun club : afficher un accès vide plutôt qu'un accès global implicite.

Succès : les mineurs et leurs données sont protégés par un périmètre clair.

## Ordre de navigation recommandé

1. Génération enfant : consulter la vue d'ensemble.
2. Génération enfant : créer ou vérifier les clubs.
3. Génération enfant : créer les fiches enfants.
4. Génération enfant : renseigner les consentements.
5. Génération enfant : vérifier les rattachements aux clubs.
6. Génération enfant : saisir les présences.
7. Génération enfant : consulter l'historique de présence.
8. Génération enfant : créer les événements gratuits.
9. Génération enfant : suivre les inscriptions.
10. Génération enfant : créer les notifications.
11. Génération enfant : consulter l'historique des notifications.
12. Génération enfant : vérifier les données partageables avec le suivi pastoral.
13. Génération enfant : archiver ou désactiver les fiches non actives.
14. Génération enfant : configurer les droits module.

## Notes UX prioritaires

- Les informations liées aux mineurs doivent être minimisées par défaut : afficher seulement ce qui sert l'action en cours.
- Les allergies, incidents et informations de sécurité doivent être visibles aux équipiers autorisés au moment utile, sans être exposées dans les vues globales inutiles.
- Les consentements RGPD, image et activités spécifiques doivent être explicites, datés et bloquants quand ils sont requis.
- Les clubs hors périmètre et les enfants hors périmètre ne doivent jamais apparaître dans les listes, recherches, statistiques, exports ou notifications.
- Les présences doivent être rapides à saisir sur le terrain, avec des écrans denses mais lisibles.
- Les événements du module restent gratuits : aucun paiement, tarif ou logique de transaction ne doit apparaître.
- Les notifications doivent être ciblées par club, pôle ou population autorisée, avec historique consultable.
- La passerelle vers le suivi pastoral doit transmettre uniquement le strict nécessaire, après vérification des droits et consentements.
- Les actions visibles doivent refléter les droits réels : créer un club, créer une fiche, saisir une présence, publier un événement, notifier, exporter, archiver.
- Les messages d'erreur doivent être accentués et compréhensibles : “enfant”, “présence”, “événement”, “consentement”, “sécurité”, “archivé”, “désactivé”.
