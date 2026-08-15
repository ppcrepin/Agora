# Méthode de travail

> Ce document est contraignant. Il décrit comment le projet Endoxa est conduit,
> et il prime sur toute habitude de travail.

## 1. Principe

**Rien n'est livré du premier coup.** Toute proposition — une maquette, un
libellé, un algorithme, un schéma de données, une palette — est produite par un
spécialiste, contestée par au moins un autre, vérifiée par une preuve, et refaite
autant de fois que nécessaire avant d'être montrée.

Une proposition qui n'a pas été contestée n'est pas une proposition : c'est un
premier jet.

## 2. L'équipe

Neuf rôles. Chacun est tenu à une exigence extrême dans son domaine et a le
devoir de contredire les autres.

| Rôle | Domaine | Droit de veto sur |
|---|---|---|
| **UX & parcours** | Flux, états, frictions, accessibilité d'usage | Tout écran, tout parcours |
| **Direction artistique** | Identité, palette, typographie, mouvement | Toute décision visuelle |
| **Rédaction produit** | Chaque mot affiché, en français | Tout libellé, tout ton |
| **Psychométrie** | Modèle de Schwartz, scoring, validité de la mesure | Toute affirmation sur les valeurs |
| **Théorie du vote** | Jugement majoritaire, agrégation, cas limites | Tout calcul de résultat |
| **Éditorial & neutralité** | Génération IA, sources, équilibre, déontologie | Toute question publiée |
| **Protection des données** | RGPD, données sensibles, modèle de menace | Tout traitement de données |
| **Architecture technique** | Stack, données, tests, performance, coûts | Toute décision d'implémentation |
| **Qualité** | Contradiction systématique, vérification des preuves | Toute livraison |

## 3. Le cycle obligatoire

Aucune étape n'est facultative, et l'ordre ne change pas.

1. **Questionner.** Le spécialiste concerné produit les questions ouvertes avant
   toute proposition, classées en *bloquant*, *structurant*, *détail*.
2. **Trancher.** Les questions bloquantes sont soumises au porteur du projet, en
   choix multiples, avec pour chaque option sa conséquence réelle.
3. **Proposer.** Le spécialiste produit, en tenant les décisions prises.
4. **Contester.** Au moins un autre rôle attaque la proposition et cherche
   activement à la faire tomber. Le rôle Qualité cherche systématiquement ce qui
   a été oublié plutôt que ce qui est faux.
5. **Prouver.** La proposition est vérifiée par un moyen extérieur à l'opinion :
   rendu et capture d'écran pour un visuel, validateur pour une palette, test
   numérique pour un calcul, source citée pour un fait, mesure pour une
   performance.
6. **Refaire.** S'il reste une objection non levée, retour à l'étape 3. Deux
   tours au minimum sur tout ce qui est structurant.
7. **Montrer.** Seulement alors, et en indiquant explicitement ce qui a été
   contesté, ce qui a été corrigé, et ce qui reste incertain.

## 4. Règles de preuve

- **Un visuel** n'existe pas tant qu'il n'a pas été rendu et regardé, en clair et
  en sombre, sur une largeur de téléphone et sur une largeur d'écran.
- **Une palette** n'est jamais jugée à l'œil : elle passe un validateur
  (bandes de luminosité, plancher de chroma, séparation pour les daltonismes,
  contraste sur le fond réel).
- **Un calcul** n'est acquis qu'accompagné de ses cas limites sous forme de tests.
- **Un fait** n'est affirmé qu'avec sa source, vérifiée et datée.
- **Une affirmation sur les valeurs ou sur la mesure** doit distinguer ce que la
  littérature établit de ce qui relève de notre interprétation.
- **Un chiffre de coût ou de performance** est mesuré ou explicitement annoncé
  comme une estimation, avec sa méthode.

## 5. Ce qui est interdit

- Présenter un premier jet comme une recommandation.
- Livrer un visuel sans l'avoir regardé.
- Affirmer qu'une chose fonctionne sans l'avoir exécutée.
- Masquer une incertitude pour rendre une proposition plus vendable.
- Passer à l'étape suivante avec une question bloquante non tranchée.
- Élargir le périmètre demandé sans le dire.

## 6. Rythme

Le questionnement précède toujours la production, et il n'est pas limité en
volume. Tant que des questions structurantes restent ouvertes, on questionne.
