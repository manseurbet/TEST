---
name: situations-dgd-regles
description: "Règles métier et procédures de travail pour l'application Situations_DGD (situation-dgd.com) : cheminement des documents (décompte provisoire, avenants, avenant de clôture, DGD), DGD de simulation, verrouillage et export, mode Entrepreneur, et règles de collaboration. À utiliser dès qu'il est question de situation-dgd.com, de Situations_DGD, de décompte, d'avenant, de DGD, de l'export Excel, du mode Entrepreneur, du lien OK, ou des fichiers views.js, core.js, import-backup.js, index.html."
---

# Situations_DGD : règles et procédures

Application web (PWA) pour bureaux d'études et entreprises BTP en Algérie. Hébergée sur GitHub Pages, domaine situation-dgd.com. L'utilisateur est architecte, pas développeur. Il connaît le terrain et le vocabulaire des chantiers : il faut suivre ses règles, pas les réinventer.

## 1. Cheminement des documents

Pendant le chantier :

- **Décompte provisoire** : établi à partir des attachements, par période. Il n'a **pas** de colonnes plus-value ni moins-value. Il est accessible dès le premier attachement validé.
- **Question « Voulez-vous un avenant ? »** : posée à tout moment pendant le chantier.
  - **Oui** : ouvre l'avenant suivant (n°1, n°2, …).
  - **Non** : retour aux attachements et au décompte provisoire.
- **Avenants** : une liste numérotée, comme les situations du décompte. Chaque avenant peut ajouter des travaux supplémentaires et de nouvelles quantités.
- **Avenant de clôture** : un avenant comme les autres, marqué « de clôture ». Il peut être le n°1, le n°2, ou le seul, et son intitulé peut combiner les deux (« avenant n°1 et de clôture »). Ce n'est pas forcément le dernier numéro.

Fin de chantier :

- Après l'avenant de clôture, **plus rien ne s'ajoute** : ni avenant, ni décompte provisoire.
- **Fin des travaux**, puis **DGD** : décompte général définitif. Il contient la plus-value, la moins-value et les nouvelles quantités. Il est **toujours** intitulé « définitif », même si les travaux ne sont pas tous achevés.
- Le DGD se fait **après** l'avenant de clôture.
- La case « travaux finis » ne sert qu'à masquer les articles encore en cours. Elle sera revue plus tard.

Pages séparées : le décompte provisoire et le DGD sont deux pages distinctes. Ne jamais mélanger leurs colonnes.

## 2. DGD de simulation

- Demandé parfois par le maître d'ouvrage pour contrôler les dépenses.
- Calcule le DGD comme si les travaux étaient terminés.
- Destiné **au seul maître d'ouvrage**. L'entrepreneur ne le reçoit pas.
- **Sans valeur officielle** : il ne clôt ni ne verrouille rien. Les avenants et attachements restent possibles.
- Porte la mention « Simulation, sans valeur officielle » à l'impression, pour ne jamais être confondu avec le DGD.
- Ne figure pas dans le schéma principal du cheminement.

## 3. Verrouillage et export

- Après le DGD, le **projet est bloqué dans l'application** : plus aucune modification.
- L'**export reste libre**, mais il n'y a **qu'un seul export** après le DGD.
- Le titre des documents suit la demande du maître d'ouvrage (décompte provisoire, avenant, DGD, simulation).
- Le fichier Excel exporté reste **modifiable** (pas de lecture seule). Il contient les valeurs, pas les formules, volontairement.
- Feuille cachée `_archigest` : elle contient les données complètes du projet pour la restauration. Elle reste dans le fichier de l'entrepreneur ; le fichier envoyé à l'architecte doit la retirer manuellement avant envoi. L'utilisateur fait cela à la main, pas l'application.

## 4. Mode Entrepreneur

- Valable pour tous les entrepreneurs. Le mode Bureau reste inchangé.
- Pas d'interrupteur entre les deux modes.
- Parcours voulu : l'entrepreneur remplit l'attachement, appuie sur **Envoyer**, reçoit un message (WhatsApp), répond **OK** ; l'application ouvre alors le dossier Excel à télécharger.
- Le message d'envoi va vers le numéro de l'utilisateur (« Dossier à préparer »). L'architecte corrige sur la **page imprimée**, pas dans l'application.
- Imprimer n'est disponible que sur l'attachement, à faire signer avec le maître d'œuvre.
- Après téléchargement du dossier, le projet reste **caché** dans l'appareil. L'accueil propose « Attachement suivant » (reprend avec le cumul), « Nouveau devis » (demande confirmation « Confirmer nouveau projet »), et « Pas reçu ? Télécharger encore ».
- Accueil sans projet : bouton « Importer le devis Excel ».
- Bouton « Envoyer » : visible seulement après la validation de l'attachement en cours. Au 2e attachement, il disparaît jusqu'à validation.
- Le lien OK doit être ouvert **dans le même navigateur et sur le même téléphone** que le projet. Sinon : « Projet introuvable sur cet appareil ».
- Le générateur (generateur-dossiers.html) signe le lien avec le **seul numéro de projet**, comme `dosCheck` dans core.js. Le lien a la forme `…/#dossier=PROJET~CODE`.
- Pas de demande de code au retour du OK (paiement avant l'envoi du OK). Le code de licence reste demandé pour l'export Excel en mode Bureau.
- Dossier entrepreneur : un export (un seul fichier). Pas de deuxième copie.
- Phrase « dossier sous 24 h » : à ne garder que si le délai peut être tenu.

## 5. Règles d'interface

- Textes très courts : un mot explicite ou un bouton, pas une phrase (« les Algériens ne lisent pas, ils veulent un mot explicite, un bouton »).
- Libellés précis plutôt que génériques (« Importer le devis Excel », « Confirmer nouveau projet »).

## 6. Règles de collaboration

- **Ne rien coder sans accord explicite.** Discuter d'abord les règles, puis demander le code.
- **Avant de modifier un fichier, demander la version en ligne** (celle qui est réellement déployée). Ne pas travailler sur une copie de travail.
- **N'envoyer que les fichiers modifiés**, pas le zip complet.
- **Ne jamais publier** generateur-dossiers.html ni generateur-codes.html sur GitHub (ils contiennent des secrets de signature).
- Tester dans un navigateur avant tout déploiement.
- Déployer ensemble les fichiers qui dépendent l'un de l'autre (ex. views.js et import-backup.js).
- Signaler les incohérences avant de coder.
- Ne pas déployer une version corrigée tant que TEST ne valide pas le parcours complet. Les testeurs gardent l'ancienne version jusque-là.

## 7. Points encore ouverts

- Si les travaux finissent **sans** avenant de clôture, peut-on établir le DGD ? (Le schéma actuel ne le prévoit pas.)
- Le verrouillage exact : que se passe-t-il si un export a déjà été fait avant le DGD ?
- Le rôle de la case « travaux finis » après sa suppression éventuelle.
- Sauvegarde serveur (décision du 10 novembre : attendre le retour des utilisateurs).

## 8. Comptage (GoatCounter et Google Analytics)

- Les visites et événements de la version de test (dossier `/TEST`) ne doivent pas être comptés. `index.html` et `views.js` le font déjà ; une fois TEST supprimé, ces conditions n'ont plus d'effet et peuvent rester.
- GoatCounter : manseurtahar.goatcounter.com.
