# Endoxa — cadrage produit

> *ἔνδοξα* : chez Aristote, les opinions communément admises — celles que
> partagent le plus grand nombre, ou les sages.

> Statut : brouillon issu d'une session de brainstorm. Aucune ligne de code n'a
> encore été écrite. Tout ce qui suit est révisable ; les points marqués
> **[ouvert]** attendent encore une décision.

---

## 1. Le concept en une phrase

Une application quotidienne où l'on juge des propositions issues de l'actualité
au **jugement majoritaire**, et où le vrai produit n'est pas le résultat du vote
mais **le miroir** : au fil des jours, chacun découvre son propre système de
valeurs, et les membres de son groupe découvrent le sien.

## 2. La promesse

**Pour l'utilisateur :** « Au bout de trois mois, je sais nommer ce qui compte
vraiment pour moi, je repère mes arbitrages et je comprends mes propres
contradictions. »

**Pour le groupe :** « Je ne sais pas comment mon oncle a voté, mais je sais
qu'il place la sécurité et la tradition très haut. Ça ne m'énerve plus, ça
s'explique. »

C'est un produit d'**introspection** qui se pratique **à plusieurs**.
L'actualité est le prétexte, pas le sujet.

## 3. Décisions actées

| Sujet | Décision |
|---|---|
| Objectif principal | Se connaître soi-même (miroir), pas convaincre ni s'engager |
| Utilisateur cible | Un groupe qui existe déjà : famille, classe, asso, équipe |
| Modèle de valeurs | Théorie des valeurs de **Schwartz** (10 valeurs, cercle) |
| Scoring | Généré par l'IA, en coulisse, attaché aux propositions et justifications |
| Vote | **Jugement majoritaire**, 5 mentions × 4 propositions |
| Sans avis | Mention « sans avis » distincte, comptée à part du JM |
| Changer d'avis | Possible, et **valorisé** dans le produit |
| Question du jour | **Une seule pour tout le monde**, chaque jour |
| Sujets | Actualité chaude, France + international |
| Source | Agrégation de **flux RSS de presse** de sensibilités variées |
| Dossier factuel | Chiffres sourcés + l'argument principal de chaque camp |
| Justification | L'utilisateur **choisit** parmi des justifications proposées (pas de texte libre) |
| Le « pourquoi » | Question **séparée** après le vote (fait / émotion / vécu / principe) |
| Anonymat | **Votes anonymes**, mais **profil de valeurs visible** dans le groupe |
| Profil affiché | Radar complet des 10 valeurs de Schwartz |
| Groupes | Privés, sur invitation, appartenance multiple possible |
| Onboarding | 10 dilemmes d'amorçage à l'inscription |
| Rétention | Le profil qui se complète + la vie du groupe |
| Données perso | Pseudo + tranche d'âge + région (le reste optionnel) |
| Gouvernance | Publication automatique + **contrôle de neutralité par les utilisateurs** |
| Pédagogie du JM | Présente mais discrète, jamais militante |
| Ton | Calme, élégant, respirant |
| Plateforme | Web mobile (PWA) d'abord, natif ensuite si ça prend |
| Session type | ~3 minutes |
| Périmètre v1 | Expérience **solo complète**, sans groupes |
| Archives | Questions passées **rejouables à volonté** |
| Rendez-vous hebdo | **Le bilan du dimanche** |
| Nom | **Endoxa** |
| Nature du projet | **[ouvert]** — à trancher plus tard |

## 4. La boucle quotidienne

Huit temps, dont deux optionnels, pour environ trois minutes.

1. **Le sujet.** Titre et trois lignes de contexte factuel. Deux portes :
   « voir les faits » ou « je vote ».
2. **Le dossier** *(optionnel)*. Trois chiffres sourcés et cliquables, puis
   l'argument principal de chaque camp en une phrase. C'est le matériau qui
   permettra plus tard à l'utilisateur de distinguer ce qu'il sait de ce qu'il
   ressent.
3. **Le vote.** Quatre propositions, chacune notée sur cinq mentions :
   *À rejeter · Insuffisant · Passable · Bien · Très bien*, plus une case
   *Sans avis*. On note les quatre, pas une seule — c'est ce qui distingue le
   jugement majoritaire d'un sondage ordinaire.
4. **La justification.** « Qu'est-ce qui motive ton jugement ? » Quatre
   formulations proposées par l'IA ; on en choisit une. Aucune saisie de texte,
   donc aucune modération, et un signal de valeurs bien plus précis que le vote
   seul.
5. **Le pourquoi.** « D'où vient cette conviction ? » — *Des données · Un
   ressenti · Mon expérience vécue · Un principe moral · La confiance en
   quelqu'un*. Une seule réponse, un seul geste.
6. **Les résultats.** La proposition gagnante au jugement majoritaire, la
   distribution des mentions, ma position par rapport à l'ensemble.
7. **L'argument d'en face.** La justification la plus choisie par ceux qui ont
   jugé à l'opposé de moi, présentée avec la valeur qu'elle défend. Pas une
   contradiction : une traduction.
8. **Mon profil.** Ce que ce vote a déplacé sur mon radar, et éventuellement une
   discrète invitation à noter l'équité de la question du jour.

## 5. Le modèle de valeurs

### Les dix valeurs de Schwartz

| Valeur | Ce qu'elle recouvre |
|---|---|
| Autonomie | Indépendance de pensée et d'action, liberté, créativité |
| Stimulation | Nouveauté, audace, goût du changement |
| Hédonisme | Plaisir, jouissance de la vie |
| Réussite | Succès personnel selon les critères sociaux, mérite, compétence |
| Pouvoir | Statut, contrôle des ressources, autorité |
| Sécurité | Sûreté, stabilité, ordre, harmonie sociale |
| Conformité | Retenue, respect des règles et des attentes |
| Tradition | Respect des coutumes, de l'héritage, des identités |
| Bienveillance | Souci du bien-être des proches, loyauté, entraide |
| Universalisme | Justice sociale, égalité, tolérance, protection de la nature |

Elles s'organisent en cercle, avec deux axes d'opposition qui donnent
gratuitement les arbitrages politiques les plus courants :

- **Ouverture au changement** (autonomie, stimulation) ↔ **Continuité**
  (sécurité, conformité, tradition)
- **Dépassement de soi** (universalisme, bienveillance) ↔ **Affirmation de soi**
  (pouvoir, réussite)

### Comment on calcule le profil

1. Chaque **proposition** porte un vecteur de poids sur les dix valeurs, généré
   par l'IA : par exemple *{sécurité : +0,8 ; autonomie : −0,5}*.
2. Chaque **justification** porte son propre vecteur, plus précis encore
   puisqu'elle dit *pourquoi* on juge ainsi.
3. Les mentions se traduisent en scores (`À rejeter` = −2 … `Très bien` = +2 ;
   `Sans avis` = ignoré) et pondèrent les vecteurs.
4. Les contributions s'accumulent, puis sont **centrées par utilisateur** : on
   soustrait la moyenne personnelle avant d'afficher le radar. Sans cette
   étape, on mesure surtout le fait que certains notent généreusement et
   d'autres sévèrement — pas leurs valeurs.
5. Le radar reste flou tant que le nombre de votes est faible sur une
   dimension : on affiche l'incertitude au lieu de la masquer.

### Réserve honnête sur Schwartz

Schwartz mesure des valeurs **personnelles**, pas des positions politiques.
Deux valeurs se prêtent mal à des questions d'actualité — **hédonisme** et
**pouvoir** — parce que presque personne ne revendique une proposition au nom du
pouvoir. En pratique, sept ou huit dimensions porteront l'essentiel du signal.

Le lien avec le vocabulaire politique français fonctionne globalement bien :
liberté → autonomie, égalité et écologie → universalisme, mérite → réussite,
souveraineté → tradition et sécurité, solidarité → bienveillance. La laïcité est
le cas le plus ambigu : elle peut relever de l'autonomie comme de la tradition
selon celui qui la défend — ce qui est précisément le genre de chose que
l'application devrait savoir montrer plutôt que trancher.

## 6. Le jugement majoritaire

**Mécanique.** Chaque électeur attribue une mention à *chaque* proposition. La
**mention majoritaire** d'une proposition est la médiane des mentions reçues. Le
gagnant est la proposition dont la mention majoritaire est la plus élevée.

**Égalités.** Quand deux propositions ont la même médiane, on compare la part
d'électeurs qui ont noté *strictement au-dessus* de cette médiane et la part
qui a noté *strictement en dessous*. Celle qui a le plus de soutiens
au-dessus l'emporte ; à défaut, celle qui a le moins d'opposants en dessous.

**Pédagogie discrète.** Quand le résultat au jugement majoritaire diffère de ce
qu'aurait donné un scrutin classique, l'application le signale en une ligne, sans
en faire un cours. La démonstration se fait par l'exemple, pas par le discours.

## 7. Le profil épistémique

C'est la partie la plus originale du produit, et probablement celle qui n'existe
nulle part ailleurs.

À force de répondre « d'où vient cette conviction ? », l'application peut dire :
« tes positions sur l'écologie s'appuient surtout sur des données, celles sur la
sécurité sur ton expérience vécue, celles sur l'école sur un principe moral. »

Ce n'est pas un jugement — aucune source n'est présentée comme supérieure à une
autre. C'est une **carte de soi** en deux couches : *ce à quoi je tiens* et
*sur quoi je m'appuie*. Le produit doit rester rigoureusement neutre sur ce
point : une opinion fondée sur le vécu n'est pas moins légitime qu'une opinion
fondée sur des chiffres.

## 8. Groupes et vie sociale

- Un groupe se crée en trente secondes et s'ouvre par lien d'invitation.
- On peut appartenir à plusieurs groupes (famille, travail, amis).
- **Les votes individuels ne sont jamais visibles.** Seuls les agrégats du
  groupe le sont.
- **Les radars de valeurs, eux, sont visibles** entre membres. C'est le pari
  central : partager qui l'on est sans exposer ce que l'on a voté.
- Le groupe se compare à la moyenne nationale : « votre groupe est plus divisé
  que la moyenne sur ce sujet », « vous êtes tous très haut en bienveillance ».
- Notifications sobres : quand tout le monde a voté, quand le résultat du groupe
  est surprenant. Jamais de relance culpabilisante.

## 9. Génération de contenu et garde-fous

**Le pipeline quotidien.**

1. Agrégation de flux RSS de plusieurs titres de presse aux sensibilités
   assumées différentes.
2. Détection des sujets réellement saillants (présents dans plusieurs sources).
3. Génération par l'IA : contexte, quatre propositions équilibrées, trois
   chiffres avec leurs sources, un argument par camp, quatre justifications, et
   les vecteurs de valeurs associés.
4. Passage de contrôle par une seconde IA : équilibre des propositions,
   formulation non orientée, vérification que les chiffres proviennent bien des
   sources citées.
5. Publication, avec une réserve de questions intemporelles en secours si le
   pipeline échoue — il ne doit jamais y avoir un jour sans question.

**Le risque principal du produit.** L'IA qui rédige la question et les
propositions détient l'essentiel du pouvoir sur le résultat. Une formulation
légèrement orientée suffit à faire basculer un vote. Les parades retenues :

- La **diversité des sources** en amont est un garde-fou structurel, pas un
  détail de mise en œuvre.
- Les utilisateurs peuvent **noter l'équité** de la question du jour et la
  signaler ; au-delà d'un seuil, elle est retirée et le fait est rendu public.
- Les vecteurs de valeurs attachés aux propositions doivent être
  **consultables** par qui le souhaite. Un scoring caché serait la meilleure
  façon de perdre la confiance des utilisateurs sur un sujet pareil.
- Les prompts de génération devraient être **publics**. C'est la version
  moderne de « qui écrit les questions du sondage ».

## 10. Esquisse du modèle de données

```
User          pseudo, tranche_âge, région, créé_le
Group         nom, code_invitation
GroupMember   user, group, rôle

Question      date, titre, contexte, statut, sources[], score_équité
Proposition   question, texte, vecteur_valeurs{}
Justification question, proposition, texte, vecteur_valeurs{}, type_source
Fait          question, énoncé, chiffre, url_source

Vote              user, proposition, mention(−2..+2 | sans_avis), version, daté_le
ChoixJustification user, question, justification
RéponseÉpistémique user, question, type_source
SignalementÉquité  user, question, note, motif

ProfilValeurs  user, 10 scores, incertitude par dimension, nb_votes
```

`Vote` est **versionné** plutôt qu'écrasé : c'est ce qui permet de célébrer les
changements d'avis et de montrer à chacun le chemin parcouru.

## 11. Périmètre

**v1 — l'expérience solo complète.** Onboarding en dix dilemmes, question du
jour, dossier factuel, vote au jugement majoritaire, justification, question
épistémique, résultats, argument d'en face, radar de valeurs, historique,
changement d'avis. Pas de groupes. C'est testable seul dès le premier jour, et
c'est déjà le cœur du produit.

**v2 — le groupe.** Création par lien, radars des membres, agrégats et
comparaisons, notifications. C'est la seule façon de vérifier la vraie promesse
auprès de vrais proches.

**Plus tard.** Application native, comparaisons nationales par âge et région,
rejeu annuel, usage scolaire, éventuelle ouverture publique.

## 12. Risques identifiés

| Risque | Gravité | Parade envisagée |
|---|---|---|
| Biais de formulation de l'IA | **Élevée** | Sources plurielles, seconde IA de contrôle, notation par les utilisateurs, prompts publics |
| Opinions politiques = données sensibles (RGPD art. 9) | **Élevée** | Consentement explicite, minimisation, hébergement européen, pseudonymat, export et suppression natifs |
| Un radar qui enferme au lieu d'ouvrir | Moyenne | Profil mouvant et incertain par construction, changement d'avis valorisé, pas d'étiquette globale |
| Coût de l'IA à l'échelle | Moyenne | Une seule génération par jour pour tout le monde, mise en cache totale |
| Panne du flux d'actualité | Faible | Réserve de questions intemporelles pré-générées |
| Validation par les stores sur un sujet politique | Faible en v1 | Le web contourne la question jusqu'au passage au natif |
| Groupe trop petit pour des agrégats sensés | Moyenne | Seuil minimal avant affichage des résultats de groupe |

## 13. Les archives

Toutes les questions passées restent **rejouables à volonté**, dans une
bibliothèque consultable. Trois raisons :

- Un nouvel arrivant peut enrichir son radar sans attendre trois semaines.
- Un utilisateur qui saute des jours n'est pas puni.
- C'est le support naturel du changement d'avis : rejouer une vieille question
  et découvrir qu'on ne la juge plus pareil.

Conséquence sur les données : un vote d'archive doit être **distingué** du vote
du jour dans les statistiques publiques, sans quoi on mélangerait des jugements
formés dans des contextes différents. Il compte pleinement pour le profil
personnel, mais pas dans le résultat officiel de la journée.

## 14. Le bilan du dimanche

Un rendez-vous hebdomadaire, plus lent que le quotidien, qui sert directement la
promesse d'introspection :

- Ce qui a bougé sur ton radar cette semaine, et grâce à quelle question.
- Ta position la plus atypique par rapport à l'ensemble.
- Le sujet sur lequel ton groupe s'est le plus divisé.
- La valeur que tu as le plus mobilisée sans t'en rendre compte.
- Éventuellement : une question ancienne à rejouer.

C'est aussi le format qui se partage le mieux, et le seul moment où l'on peut
raisonnablement se permettre une notification.

## 15. Le nom

**Endoxa**, retenu après vérification de disponibilité.

*ἔνδοξα* désigne chez Aristote les opinions communément admises — celles que
partagent le plus grand nombre, ou les sages. C'est littéralement l'objet de
l'application.

Vérifications effectuées : `endoxa.fr` et `endoxa.app` ne résolvent pas en DNS,
ce qui suggère qu'ils sont libres — **à confirmer chez un registrar**, l'absence
de DNS ne prouvant pas la disponibilité. `endoxa.com` appartient à un éditeur
B2B sud-africain sans visibilité en France.

*Doxa* seul a été écarté : marque horlogère suisse établie, domaines pris, et
surtout confusion avec **Odoxa**, institut de sondage français — rédhibitoire
pour une application de vote.

## 16. Points encore ouverts

- La nature du projet : personnel, civique, ou produit à monter.
- L'identité visuelle et le traitement graphique du radar.
- Le seuil de membres à partir duquel les résultats d'un groupe s'affichent.
- Le mécanisme exact de « célébration » d'un changement d'avis.
- La formulation précise des cinq mentions et des cinq sources épistémiques.
- Le sort des votes d'archive dans les statistiques nationales.

---

## 17. Décisions révisées après les dossiers de questions

Sept dossiers de spécialistes (voir `docs/questions/`) ont produit environ 280
questions et relevé plusieurs contradictions dans le cadrage initial. Les
décisions ci-dessous **remplacent** celles des sections précédentes.

| Sujet | Décision initiale | Décision révisée | Motif |
|---|---|---|---|
| Nature du projet | ouverte | **Bien commun civique** | Seul cadre où « profil de valeurs » et « contrôle de neutralité par les utilisateurs » ne sont pas des promesses intenables |
| Anonymat / profils | votes anonymes, profils visibles | **Vote secret par défaut, révélation volontaire après résultat ; profils visibles** | Le recoupement profil × résultat par question trahissait le vote dans tout petit groupe |
| Durée de session | 3 minutes | **5 minutes, parcours complet** | Le parcours décrit fait 4 à 6 minutes chronométrées ; la promesse initiale était fausse |
| Échelle de mentions | 5 mentions | **6 mentions** (À rejeter · Insuffisant · Passable · Assez bien · Bien · Très bien) | Échelle d'Orsay 2007 de Balinski et Laraki ; pas de milieu neutre, donc pas de refuge, et moins d'égalités sur la médiane |
| Chiffres du dossier | tirés des flux de presse | **Sources publiques uniquement** (INSEE, Eurostat, ministères, data.gouv), extrait consultable | Un flux RSS ne contient pas de chiffres : le modèle les aurait fabriqués en leur attribuant une source |
| Archives | rejouables à volonté, tout compte | **Un seul vote par question ; le rejeu écrase et recalcule** | La rejouabilité illimitée rendait le profil fabricable |
| Régime éditorial | publication automatique | **Post-modération** : publication automatique, retrait en un clic, signalement utilisateur | Seul régime tenable sans équipe ; le risque factuel est fortement réduit par le passage aux sources publiques |
| Partage | non tranché | **Export image de son profil personnel**, partageable hors de l'app | Partage choisi, jamais le résultat d'un vote |

### Ce que ces décisions impliquent

- **Les flux de presse ne servent plus qu'à repérer le sujet.** Les faits viennent
  d'ailleurs, et un sujet non documenté par une statistique publique devient
  inéligible. C'est une restriction réelle du champ couvert.
- **La révélation volontaire crée une pression sociale** : dans une famille, ne pas
  révéler son vote est un signal en soi. Il faudra concevoir contre cet effet —
  la révélation doit rester rare et jamais comptabilisée.
- **Six mentions × quatre propositions sur mobile** contraignent fortement la
  disposition de l'écran de vote, probablement vers une lecture verticale.
- **Le rejeu qui écrase** impose de conserver l'historique des versions d'un vote
  et de recalculer le profil, ce que le modèle de données doit prévoir dès le
  premier enregistrement.
- **Le statut de bien commun** rend la transparence de l'algorithme de profil et
  de la ligne éditoriale opposable au public, donc obligatoire et non plus
  optionnelle.
