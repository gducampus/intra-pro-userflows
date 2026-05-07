# Userflows UX - Module Baptême

Date: 2026-05-07

## Objectif UX

Le module baptême doit permettre au secrétariat, aux responsables pastoraux et aux administrateurs de créer, qualifier, affecter, valider, planifier, réaliser et intégrer les demandes de baptême sans perte d'information ni confusion de responsabilité.

Le parcours est pensé de la demande initiale vers le suivi pastoral complet : saisie, recherche, affectation, entretien, registre, validation, jour J, intégration dans la base membres, suivi post-baptême, statistiques et sécurité.

## Acteurs

- Secrétariat baptêmes : crée les demandes, suit le tableau de bord, filtre, prépare les transmissions, planifie les dates et pilote le jour J.
- Pasteur ou assistant pastoral : consulte les demandes affectées, réalise l'entretien, met à jour le statut et contribue à la validation.
- Responsable baptêmes : supervise les demandes, les affectations, les alertes, les statistiques et les exports.
- Administrateur : configure les droits, voit toutes les demandes, résout les cas sensibles, intègre les baptêmes dans la base membres et audite l'activité.

## UF-01 - Accéder au module baptême

Déclencheur : l'utilisateur ouvre le module Baptêmes depuis l'administration ou la navigation autorisée.

Parcours nominal :

1. Le système vérifie que l'utilisateur est authentifié.
2. Le système vérifie que l'utilisateur dispose d'un droit module baptême.
3. Le système détermine le périmètre visible selon le rôle, l'affectation et le secteur.
4. L'utilisateur arrive sur le tableau de bord baptêmes.
5. Le système affiche les demandes, les filtres et les actions autorisées.

États UX :

- Utilisateur non connecté : refuser l'accès.
- Droit module absent : afficher un refus clair.
- Aucune demande visible : afficher un état vide “Aucune demande de baptême disponible pour votre profil.”
- Périmètre restreint : ne jamais afficher les demandes hors périmètre, même grisées.

Succès : l'utilisateur voit uniquement les demandes et actions compatibles avec son rôle.

## UF-02 - Consulter le tableau de bord baptêmes

Déclencheur : l'utilisateur ouvre la page principale du module.

Parcours nominal :

1. Le système affiche les demandes triées par date de création ou priorité opérationnelle.
2. L'utilisateur voit le nom, le contact, le secteur, le statut, la date souhaitée, la date prévue et l'affectation.
3. Le système affiche les compteurs par statut.
4. L'utilisateur identifie les demandes en attente, affectées, en cours, validées, reportées, absentes ou réalisées.
5. Les actions disponibles sont affichées selon les droits et l'état de chaque demande.

États UX :

- Beaucoup de demandes : conserver les filtres visibles en haut de page.
- Demande sans affectation : signaler discrètement l'action attendue.
- Statut sensible : utiliser un libellé lisible plutôt que le code technique.

Succès : l'utilisateur comprend rapidement le volume, les priorités et les prochaines actions.

## UF-03 - Rechercher et filtrer une demande

Déclencheur : l'utilisateur saisit une recherche ou sélectionne un filtre.

Parcours nominal :

1. L'utilisateur recherche par nom, téléphone, email ou ville.
2. Il filtre par statut, pasteur, secteur ou période.
3. Le système applique les filtres côté serveur.
4. Le système applique ensuite les restrictions de droits et de périmètre.
5. Les résultats affichent les informations clés et les actions autorisées.

États UX :

- Aucun résultat : “Aucune demande de baptême ne correspond à votre recherche.”
- Recherche trop large : afficher les résultats paginés ou limités.
- Résultat hors périmètre : ne pas l'afficher.

Succès : l'utilisateur retrouve une demande sans fuite d'information sur les demandes non autorisées.

## UF-04 - Créer une demande de baptême

Déclencheur : le secrétariat ou l'administrateur clique sur “Nouvelle demande”.

Parcours nominal :

1. Le système affiche l'action uniquement si l'utilisateur peut créer une demande.
2. L'utilisateur renseigne l'identité, les coordonnées, la ville, le secteur, la date souhaitée et les notes utiles.
3. Il valide le formulaire.
4. Le système vérifie le jeton CSRF et les champs obligatoires.
5. Le système crée la demande au statut `waiting`.
6. Le système journalise la création.
7. Le système affiche un message de succès et retourne au tableau de bord.

États UX :

- Nom ou prénom absent : afficher une erreur inline.
- Contact absent : demander au moins un moyen de contact si la règle métier l'exige.
- Secteur non renseigné : autoriser ou bloquer selon la configuration du module.
- Utilisateur pastoral non autorisé : masquer l'action de création.

Succès : la demande apparaît dans le tableau de bord au statut en attente.

## UF-05 - Modifier une demande initiale

Déclencheur : un utilisateur autorisé ouvre une demande existante en édition.

Parcours nominal :

1. Le système vérifie les droits sur la demande.
2. L'utilisateur ajuste les informations de contact, secteur, date souhaitée ou notes.
3. Le système verrouille les champs sensibles si la demande est déjà affectée ou validée.
4. L'utilisateur valide.
5. Le système enregistre les modifications.
6. Le système journalise la modification.

États UX :

- Demande affectée : distinguer les champs modifiables des champs verrouillés.
- Droits insuffisants : accès refusé.
- Modification concurrente : préserver la cohérence et inviter à recharger si nécessaire.

Succès : la demande est à jour sans casser le workflow pastoral.

## UF-06 - Préparer la transmission pastorale

Déclencheur : le secrétariat prépare les demandes à traiter par l'équipe pastorale.

Parcours nominal :

1. L'utilisateur filtre les demandes en attente.
2. Il sélectionne les demandes à transmettre ou à mettre à l'ordre du jour.
3. Le système génère une liste exploitable avec les informations utiles à l'entretien.
4. L'utilisateur confirme la transmission.
5. Le système journalise l'action.

États UX :

- Aucune demande sélectionnée : afficher “Sélectionnez au moins une demande.”
- Demande incomplète : signaler les informations manquantes avant transmission.
- Export ou ordre du jour vide : bloquer la génération.

Succès : l'équipe pastorale dispose d'une liste claire des demandes à traiter.

## UF-07 - Affecter une demande

Déclencheur : un responsable autorisé affecte une demande à un pasteur, assistant pastoral ou à lui-même.

Parcours nominal :

1. Le système affiche l'action d'affectation uniquement si l'utilisateur y est autorisé.
2. L'utilisateur choisit la personne affectée.
3. Il ajoute un commentaire optionnel si nécessaire.
4. Le système vérifie le jeton CSRF et la cohérence du périmètre.
5. Le système enregistre l'affectation.
6. Si la demande était en attente, le statut passe à `assigned`.
7. Le système journalise l'affectation et le changement de statut.

États UX :

- Personne affectée invalide : afficher une erreur.
- Demande déjà affectée : proposer une réaffectation explicite.
- Droits insuffisants : masquer ou refuser l'action.

Succès : la demande apparaît dans le périmètre de la personne affectée.

## UF-08 - Consulter ses demandes affectées

Déclencheur : un pasteur ou assistant pastoral ouvre son tableau de bord baptêmes.

Parcours nominal :

1. Le système charge uniquement les demandes affectées à l'utilisateur ou à son périmètre autorisé.
2. L'utilisateur voit les demandes à traiter, en cours ou à valider.
3. Il ouvre une demande pour consulter les informations utiles à l'entretien.
4. Il accède à l'historique et au registre selon l'état de la demande.

États UX :

- Aucune demande affectée : afficher “Aucune demande de baptême ne vous est affectée.”
- Demande hors secteur : ne pas afficher.
- Responsable principal ou administrateur : afficher l'ensemble selon le droit de supervision.

Succès : l'acteur pastoral traite uniquement les demandes qui lui reviennent.

## UF-09 - Suivre l'entretien pastoral

Déclencheur : la personne affectée démarre ou met à jour le suivi d'entretien.

Parcours nominal :

1. L'utilisateur ouvre la demande affectée.
2. Il consulte les informations initiales.
3. Il ajoute les notes utiles à l'entretien.
4. Il met le statut à `in_progress` si le traitement démarre.
5. Après entretien, il valide ou reporte selon le résultat.
6. Le système journalise chaque transition.

États UX :

- Transition invalide : bloquer et expliquer l'état attendu.
- Notes sensibles : rappeler la sobriété de saisie si une charte existe.
- Entretien non terminé : conserver la demande en cours.

Succès : le statut reflète fidèlement l'avancement pastoral.

## UF-10 - Valider l'entretien

Déclencheur : l'entretien pastoral est terminé et favorable.

Parcours nominal :

1. L'utilisateur ouvre la demande.
2. Il vérifie les informations de contact, secteur et notes.
3. Il choisit l'action “Valider l'entretien”.
4. Le système vérifie que la demande est dans un état compatible.
5. Le statut passe à `interview_validated`.
6. Le système journalise la validation.
7. Le registre devient accessible pour complétion.

États UX :

- Demande non affectée : bloquer la validation.
- Informations insuffisantes : afficher les champs à compléter.
- Droits insuffisants : accès refusé.

Succès : la demande peut entrer dans la phase registre et consentements.

## UF-11 - Compléter le registre de baptême

Déclencheur : une demande avec entretien validé ouvre le formulaire registre.

Parcours nominal :

1. Le système affiche le registre lié à la demande.
2. L'utilisateur vérifie les données consolidées de la demande initiale.
3. Il renseigne les informations complémentaires.
4. Il collecte la signature RGPD.
5. Il renseigne le consentement image.
6. Il enregistre le registre.
7. Le système journalise la complétion.

États UX :

- Entretien non validé : bloquer l'accès au registre.
- Signature RGPD absente : empêcher la validation finale.
- Consentement image non renseigné : empêcher la validation si requis.

Succès : le registre est complet ou clairement marqué comme incomplet.

## UF-12 - Valider définitivement la demande

Déclencheur : le registre est complet et l'équipe baptêmes confirme la validation finale.

Parcours nominal :

1. L'utilisateur ouvre la demande et son registre.
2. Le système vérifie la complétude du registre.
3. L'utilisateur confirme la validation finale.
4. Le système passe la demande au statut `validated`.
5. Le système journalise la validation.
6. La demande devient planifiable pour une date de baptême.

États UX :

- Registre incomplet : afficher les éléments manquants.
- Consentement manquant : bloquer avec un message explicite.
- Demande déjà validée : éviter les doubles validations.

Succès : la demande est prête pour la planification opérationnelle.

## UF-13 - Planifier le baptême

Déclencheur : le secrétariat planifie une demande validée.

Parcours nominal :

1. L'utilisateur ouvre une demande validée.
2. Il renseigne la date prévue de baptême.
3. Il ajuste éventuellement l'ordre de passage.
4. Il enregistre.
5. Le système met à jour le tableau opérationnel.
6. Le système journalise la planification.

États UX :

- Date prévue absente : conserver la demande validée mais non planifiée.
- Date passée incohérente : demander confirmation ou bloquer selon règle métier.
- Ordre de passage en doublon : signaler le conflit.

Succès : la demande apparaît dans le planning ou tableau du jour J.

## UF-14 - Gérer le jour J

Déclencheur : le secrétariat ouvre les demandes planifiées pour une date de baptême.

Parcours nominal :

1. Le système affiche les participants prévus pour la date.
2. L'utilisateur vérifie l'ordre de passage.
3. Il renseigne la présence.
4. Il met à jour le statut des prophéties si applicable.
5. Il marque les demandes réalisées, absentes ou reportées.
6. Le système journalise les changements.

États UX :

- Participant absent : proposer le statut `absent` ou `postponed` selon le contexte.
- Ordre modifié : conserver une trace de la modification.
- Double clic ou renvoi formulaire : éviter les transitions dupliquées.

Succès : chaque demande planifiée a un état opérationnel clair après le jour J.

## UF-15 - Consulter l'historique d'une demande

Déclencheur : l'utilisateur ouvre l'action “Historique”.

Parcours nominal :

1. Le système vérifie le droit de consultation de la demande.
2. Il affiche les changements de statut du plus récent au plus ancien.
3. Chaque ligne indique l'ancien statut, le nouveau statut, l'acteur, le commentaire et la date.
4. L'utilisateur peut reconstituer le parcours de la demande.

États UX :

- Aucun historique : afficher un état vide plutôt qu'un tableau nu.
- Acteur supprimé ou absent : afficher “Utilisateur inconnu” sans erreur.
- Accès hors périmètre : refuser l'accès.

Succès : les décisions clés sont auditables.

## UF-16 - Intégrer un baptême réalisé dans la base membres

Déclencheur : l'administrateur ou responsable autorisé intègre une demande réalisée.

Parcours nominal :

1. Le système affiche l'action uniquement si la demande est réalisée et validée.
2. L'utilisateur clique sur “Intégrer”.
3. Le système vérifie le jeton CSRF.
4. Le système recherche les doublons potentiels dans la base membres.
5. Si aucun doublon bloquant n'existe, le système crée ou met à jour la fiche membre.
6. Le système renseigne `Member::baptismAt` avec la date prévue ou réalisée.
7. Le système génère les suivis post-baptême M+1, M+4 et M+8.
8. Le système journalise l'intégration.

États UX :

- Demande reportée ou absente : masquer ou bloquer l'intégration.
- Registre non validé : bloquer l'intégration.
- Doublon potentiel : afficher une résolution avant création automatique.
- Fiche membre existante : expliciter si l'action met à jour ou crée une fiche.

Succès : le baptême réalisé est synchronisé dans les membres sans écrasement silencieux.

## UF-17 - Résoudre les doublons avant intégration

Déclencheur : le système détecte un ou plusieurs doublons potentiels.

Parcours nominal :

1. Le système affiche les fiches membres candidates.
2. L'utilisateur compare nom, prénom, téléphone, email et autres données disponibles.
3. Il choisit une fiche existante ou décide de créer une nouvelle fiche si autorisé.
4. Si un forçage est nécessaire, il saisit une justification.
5. Le système enregistre la décision.
6. Le système reprend l'intégration.

États UX :

- Doublon bloquant sans décision : empêcher l'intégration.
- Justification absente : bloquer le forçage.
- Utilisateur non autorisé : afficher seulement la consultation ou refuser l'action.

Succès : l'intégration est fiable et traçable.

## UF-18 - Suivre les jalons post-baptême

Déclencheur : un baptême intégré a généré les suivis M+1, M+4 et M+8.

Parcours nominal :

1. Le système affiche les suivis à venir ou en retard.
2. L'utilisateur filtre par jalon, statut, secteur ou période.
3. Il ouvre un suivi et ajoute des notes.
4. Il marque le suivi comme fait, à relancer ou annulé selon les règles retenues.
5. Le système conserve l'historique du suivi.

États UX :

- Aucun suivi : afficher un état vide.
- Suivi en retard : le rendre visible sans alarmer inutilement.
- Demande ou membre hors périmètre : ne pas afficher.

Succès : l'accompagnement post-baptême reste visible et pilotable.

## UF-19 - Exporter les demandes

Déclencheur : l'utilisateur clique sur l'export depuis le tableau de bord.

Parcours nominal :

1. Le système applique les filtres courants.
2. Le système applique les restrictions de droits et de périmètre.
3. Le système génère un fichier CSV compatible tableur.
4. L'utilisateur récupère l'export.

États UX :

- Aucun résultat exportable : afficher un message clair.
- Export volumineux : prévenir si un traitement différé est nécessaire.
- Données sensibles : limiter les colonnes selon le rôle.

Succès : l'utilisateur obtient un export exploitable sans contourner les droits.

## UF-20 - Consulter les statistiques baptêmes

Déclencheur : un responsable ou administrateur ouvre la vue statistiques.

Parcours nominal :

1. Le système affiche les volumes de demandes par période.
2. Il affiche les répartitions par statut, secteur, ville et affectation.
3. Il calcule le taux de complétion du workflow.
4. L'utilisateur ajuste la période et les filtres.
5. Les statistiques se recalculent selon le périmètre autorisé.

États UX :

- Période vide : afficher zéro sans erreur.
- Statut reporté ou absent : conserver ces catégories pour ne pas fausser la lecture.
- Données hors périmètre : ne pas les inclure dans les totaux visibles.

Succès : le responsable peut piloter l'activité sans dépendre uniquement de la date de baptême membre.

## UF-21 - Gérer les alertes et notifications

Déclencheur : une nouvelle demande, une affectation, une validation, une absence, un doublon ou une incohérence survient.

Parcours nominal :

1. Le système crée une notification interne selon la règle configurée.
2. Le destinataire voit l'alerte dans son périmètre.
3. Il ouvre la demande concernée.
4. Il traite l'action attendue.
5. Le système historise l'envoi ou le traitement.

États UX :

- Notification non configurée : ne pas bloquer le workflow.
- Destinataire hors périmètre : ne pas envoyer d'information sensible.
- Alerte déjà traitée : éviter les relances inutiles.

Succès : les actions sensibles ou urgentes ne restent pas invisibles.

## UF-22 - Configurer les droits du module baptême

Déclencheur : l'administrateur ouvre la configuration de sécurité du module baptême.

Parcours nominal :

1. L'administrateur configure les rôles autorisés : secrétariat, pasteurs, assistants, responsables et administrateurs.
2. Il définit les droits de création, lecture, affectation, validation, intégration, export et statistiques.
3. Il enregistre.
4. Le système applique ces droits côté serveur et côté interface.
5. Les tableaux de bord reflètent immédiatement les actions disponibles.

États UX :

- Aucun rôle disponible : afficher un état vide.
- Configuration incohérente : prévenir avant enregistrement.
- Accès module accordé sans périmètre : expliquer que les demandes restent filtrées par rôle, secteur ou affectation.

Succès : le module est accessible aux bons acteurs sans exposer les demandes sensibles.

## Ordre de navigation recommandé

1. Baptêmes : consulter le tableau de bord.
2. Baptêmes : rechercher et filtrer.
3. Baptêmes : créer une demande.
4. Baptêmes : préparer la transmission pastorale.
5. Baptêmes : affecter une demande.
6. Baptêmes : suivre ses demandes affectées.
7. Baptêmes : réaliser et valider l'entretien.
8. Baptêmes : compléter le registre.
9. Baptêmes : valider définitivement la demande.
10. Baptêmes : planifier le baptême.
11. Baptêmes : gérer le jour J.
12. Baptêmes : consulter l'historique.
13. Baptêmes : intégrer dans la base membres.
14. Baptêmes : résoudre les doublons.
15. Baptêmes : suivre les jalons post-baptême.
16. Baptêmes : exporter les demandes.
17. Baptêmes : consulter les statistiques.
18. Baptêmes : gérer alertes et notifications.
19. Baptêmes : configurer les droits module.

## Notes UX prioritaires

- Les statuts doivent être lisibles par métier : en attente, affecté, en cours, entretien validé, validé, reporté, absent, réalisé.
- La fiche membre ne doit être synchronisée qu'après baptême réalisé, validé et intégré.
- Les demandes hors périmètre ne doivent jamais être visibles dans les tableaux, recherches, exports ou statistiques.
- Les transitions de statut doivent être explicites, historisées et protégées contre les doubles soumissions.
- Le registre et les consentements doivent bloquer la validation finale tant qu'ils sont incomplets.
- Les doublons doivent être résolus avant intégration, avec justification obligatoire en cas de forçage.
- Les actions visibles doivent refléter les droits réels : créer, affecter, valider, planifier, intégrer, exporter, consulter les statistiques.
- Les messages d'erreur doivent être accentués et compréhensibles : “baptême”, “affecté”, “validé”, “reporté”, “réalisé”, “intégré”.
