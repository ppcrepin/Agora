# Questions — Théorie du vote — jugement majoritaire

> Produit par le rôle correspondant de l'équipe (voir `docs/methode.md`).
> Questions, pas solutions. Niveaux : **BLOQUANT**, **STRUCTURANT**, **DÉTAIL**.

---

Recherche terminée. Voici le document.

---

# Endoxa — Jugement majoritaire : questions à trancher avant implémentation

**Note méthodologique.** L'accès direct aux pages (WebFetch, curl) est bloqué par le proxy sortant de cet environnement ; j'ai donc travaillé à partir d'extraits de résultats de recherche, que je cite en fin de document. Je signale explicitement les trois points où une re-vérification sur source primaire (Balinski & Laraki, *Majority Judgment*, MIT Press, ch. 13–14) est indispensable avant de coder : ils sont marqués **[À RE-VÉRIFIER SUR SOURCE PRIMAIRE]**.

---

## Rappel formel minimal (pour fixer les notations)

Soit une échelle ordonnée de mentions $g_1 > g_2 > \dots > g_k$ et $n$ votants ayant exprimé une mention sur une proposition.

- **Mention majoritaire** $\alpha$ : la mention telle qu'au moins la moitié des votants attribuent *au plus* $\alpha$ **et** au moins la moitié attribuent *au moins* $\alpha$. Quand $n$ est pair, deux mentions vérifient cette condition ; Balinski et Laraki retiennent **la plus basse** (médiane basse). Ils insistent sur le terme « mention majoritaire » plutôt que « mention médiane », précisément parce que la médiane n'est pas définie de façon unique en effectif pair.
- $p$ = part des votants attribuant **strictement plus** que $\alpha$ (« partisans » / *proponents*).
- $q$ = part des votants attribuant **strictement moins** que $\alpha$ (« opposants »).
- $(p, \alpha, q)$ est la **jauge majoritaire** (*majority gauge*).

**Deux formulations de la règle de départage circulent, et elles ne sont pas interchangeables :**

1. **Règle récursive (formulation canonique de Balinski & Laraki).** Tant que des propositions sont à égalité sur $\alpha$, on retire **une** occurrence de $\alpha$ du multiensemble de chacune, on recalcule la mention majoritaire (« mention majoritaire de rang 1 »), et l'on recommence jusqu'à ce que les mentions diffèrent.
2. **Règle de la jauge (raccourci usuel).** Parmi les propositions à égalité sur $\alpha$, on considère les valeurs $p$ et $q$ de chacune ; **la plus grande des valeurs** décide : si c'est un $p$, la proposition correspondante l'emporte ; si c'est un $q$, elle perd. Équivalent au tri par $\alpha$ décroissant, puis par $\mathrm{sign}(p-q)$, puis par $\max(p,q)$.

Ces deux règles coïncident dans le cas *générique* (jauge décisive, grands effectifs), qui est le cas quasi certain sur des scrutins réels. **Sur les petits effectifs — exactement le cas des groupes privés d'Endoxa — elles peuvent diverger et la seconde peut être indéterminée.** C'est le point technique central de ce document.

Il existe des alternatives publiées à ce départage (Fabre, *Social Choice and Welfare*, 2021) : **jugement typique** ($p-q$), **jugement usuel** ($(p-q)$ divisé par la part des votants à la mention majoritaire), **jugement central** (comparaison des parts relatives de partisans et d'opposants). Le jugement usuel est continu par rapport à ces parts ; le départage de Balinski & Laraki, qui prend la plus grande des parts, **perd la continuité**.

---

# BLOQUANT

## Q1. Quelle convention de médiane retenez-vous quand le nombre de votants exprimés est pair : médiane basse ou médiane haute ?

**Enjeu.** Si ce n'est pas fixé, la mention affichée sur un vote de groupe à 4 personnes dépend de l'implémentation du tri et peut changer d'une version à l'autre du code.

**Réponses possibles.**
- *Médiane basse (mention la plus faible des deux centrales).* Conforme à Balinski & Laraki ; conservateur ; une proposition doit convaincre strictement plus de la moitié pour monter d'un cran. Effet : les mentions affichées sont systématiquement un peu plus sévères sur les effectifs pairs.
- *Médiane haute.* Plus flatteur, mais rompt avec la littérature et rend vos résultats incomparables à tout autre outil de JM.
- *Moyenne des deux mentions centrales.* Interdit : les mentions sont un ordinal, pas un nombre ; c'est exactement l'erreur que le JM combat.
- *Alterner selon le contexte (global vs groupe).* Incohérence garantie, deux vérités affichées côte à côte.

**Recommandation.** Médiane basse, partout, sans exception — c'est la convention de référence et elle rend vos résultats auditables contre n'importe quel calculateur tiers.

---

## Q2. Quelle règle de départage exacte implémentez-vous : la règle récursive ou la règle de la jauge ?

**Enjeu.** Avec 4 propositions et seulement 5 mentions, l'égalité sur la mention majoritaire n'est pas un cas limite, c'est le **régime nominal** : le départage *est* votre algorithme de décision, pas un correctif.

**Réponses possibles.**
- *Règle récursive seule.* Toujours définie, jamais ambiguë, se dégrade proprement sur 3 votants. Coût : plus difficile à expliquer en une phrase à l'utilisateur.
- *Règle de la jauge seule.* Facile à afficher (« 45 % au-dessus, 10 % en dessous »), mais **indéterminée quand le maximum des quatre valeurs est atteint par plusieurs d'entre elles**, ce qui arrive constamment à 3–8 votants.
- *Jauge, avec repli sur la récursive en cas d'indétermination.* Meilleur des deux mondes en pratique, mais il faut prouver que les deux coïncident quand la jauge est décisive, sinon vous avez deux algorithmes qui divergent silencieusement.
- *Une alternative publiée (jugement usuel).* Continue, mieux comportée sur petits effectifs, défendable académiquement — mais ce n'est plus « le jugement majoritaire », et l'app le promeut.

**Recommandation.** Implémenter la **règle récursive comme définition normative**, et n'utiliser la jauge que comme *affichage* — la récursive est la seule totale et la seule qui reste correcte à 3 votants. **[À RE-VÉRIFIER SUR SOURCE PRIMAIRE : la formulation exacte de la récursion et son domaine d'équivalence avec la jauge.]**

---

## Q3. Que faites-vous quand la règle de départage s'épuise et que deux propositions sont strictement identiques (distributions égales) ?

**Enjeu.** Un tirage au sort silencieux transforme un résultat public en loterie non annoncée, et détruit la reproductibilité de l'archive.

**Réponses possibles.**
- *Afficher « ex æquo » sans gagnant unique.* Honnête, conforme à la méthode ; oblige l'UI à savoir afficher deux gagnants.
- *Tirage au sort déterministe seedé sur l'ID de la question.* Reproductible mais arbitraire, et l'utilisateur ne saura jamais que c'était arbitraire.
- *Départage par ordre alphabétique / ordre de saisie.* Biais systématique, exploitable par le rédacteur des propositions.
- *Départage par nombre de « sans avis » (moins = mieux).* Introduit un critère hors JM, non justifié théoriquement.

**Recommandation.** Afficher l'ex æquo explicitement — c'est un résultat, pas un échec, et le masquer serait la seule vraie trahison de la méthode.

---

## Q4. Votre échelle à 5 niveaux a un point milieu exact. Assumez-vous que la mention majoritaire tombe très souvent sur ce point milieu ?

**Enjeu.** Si les quatre propositions convergent toutes vers la mention centrale, l'app affiche quatre fois la même mention et tout se joue sur le départage — l'utilisateur ne voit plus la méthode fonctionner.

**Réponses possibles.**
- *Garder 5 mentions avec milieu neutre.* Familier (échelle de Likert standard), mais expose à l'**effet de centralité** : les répondants sans opinion tranchée se réfugient au milieu, et la médiane s'y installe.
- *Passer à 6 mentions (recommandation explicite de Balinski & Laraki, échelle « À rejeter / Insuffisant / Passable / Assez bien / Bien / Très bien », testée à Orsay en 2007).* Pas de milieu exact, donc pas de refuge neutre ; l'électeur est forcé de pencher. Coût : une colonne de plus sur mobile.
- *Garder 5 mais décaler le milieu (2 mentions positives, 1 pivot, 2 négatives avec un pivot non neutre type « Passable »).* Le pivot n'est plus « sans opinion » mais « médiocre mais acceptable ». Réduit le refuge sans changer le nombre.
- *4 mentions.* Trop grossier ; multiplie encore les égalités.

**Recommandation.** Passer à 6 mentions si l'UI le supporte ; sinon, 5 mentions dont le pivot est explicitement **évaluatif et non neutre** — le vrai « je ne sais pas » doit être la case « sans avis », pas le milieu de l'échelle.

---

## Q5. Les libellés des 5 mentions sont-ils perçus comme régulièrement espacés, et l'avez-vous testé ?

**Enjeu.** Le JM suppose un **langage commun** : si « Assez bien » et « Bien » sont perçus comme quasi synonymes alors que « Insuffisant » et « À rejeter » sont perçus comme très distants, la médiane est biaisée par le lexique, pas par les opinions.

**Réponses possibles.**
- *Reprendre le lexique scolaire français (Très bien / Bien / Assez bien / Passable / Insuffisant / À rejeter).* Immédiatement compris par tout francophone scolarisé — c'est l'argument central de Balinski & Laraki sur le langage commun. Mais il est calibré pour juger des *personnes*, pas des *propositions d'action*.
- *Lexique d'adhésion (Totalement d'accord → Totalement opposé).* Adapté à des propositions, mais glisse vers l'échelle de Likert d'accord, qui est une échelle bipolaire d'opinion, pas une échelle de mérite — et les biais d'acquiescement y sont documentés.
- *Lexique de désirabilité (Excellente idée / Bonne / Acceptable / Mauvaise / À rejeter).* Compromis lisible, adapté à l'objet « proposition ».
- *Lexique inventé maison.* À proscrire : le langage commun n'est commun que s'il préexiste au vote.

**Recommandation.** Un lexique de **désirabilité de la mesure**, pré-testé sur 50 personnes par une tâche de placement sur une règle graduée, pour vérifier l'ordre et la régularité perçue avant de figer.

---

## Q6. Les « sans avis » sont-ils exclus du calcul, et affichez-vous leur volume à côté du résultat ?

**Enjeu.** Exclure sans afficher permet à une proposition jugée par 8 % des votants d'être présentée avec la même autorité qu'une proposition jugée par 95 %.

**Réponses possibles.**
- *Exclusion pure, non affichée.* Simple, mais trompeur : c'est l'erreur d'implémentation la plus courante des outils de JM en ligne.
- *Exclusion du calcul + affichage systématique du taux d'expression.* Le calcul reste orthodoxe, l'utilisateur voit la base. Coût : une ligne d'UI par proposition.
- *Comptage des « sans avis » comme mention la plus basse.* C'est la convention de Balinski & Laraki **pour les élections politiques** (ne pas cocher = rejeter), justifiée parce qu'un électeur est censé connaître les candidats. Ici, elle serait abusive : un utilisateur peut légitimement ignorer un sujet géopolitique.
- *Exclusion + seuil de quorum en dessous duquel on n'affiche pas de mention.* Le plus rigoureux ; voir Q7.

**Recommandation.** Exclusion du calcul, **affichage obligatoire du taux d'expression au même niveau visuel que la mention** — la mention sans sa base est un chiffre sans unité.

---

## Q7. Que se passe-t-il si une proposition reçoit une majorité de « sans avis » ?

**Enjeu.** Sans règle, une proposition peut « gagner » sur la base d'une poignée de jugements polarisés d'un public auto-sélectionné.

**Réponses possibles.**
- *Rien de spécial, on calcule sur les exprimés.* Exemple : 100 votants, 60 « sans avis », 40 exprimés répartis 20 « Très bien » / 20 « À rejeter » → la médiane basse donne « À rejeter » à partir d'un échantillon de 40 % coupé en deux. Résultat affiché : absurde et sûr de lui.
- *Quorum d'expression (ex. ≥ 50 % des votants du jour) en dessous duquel la proposition est affichée « non concluante » et exclue du classement.* Rigoureux ; risque de retirer une proposition du podium et de dérouter.
- *Quorum d'affichage seulement (on calcule, on classe, mais on marque la proposition d'un avertissement).* Compromis lisible.
- *Signaler la proposition comme mal rédigée et déclencher une révision du prompt de l'IA.* La bonne réponse produit, en plus de la réponse calcul.

**Recommandation.** Quorum d'expression à 50 %, sous lequel on affiche « trop peu de jugements » au lieu d'une mention — et on traite le taux de « sans avis » comme un **indicateur qualité de la rédaction IA**, pas comme du bruit.

---

## Q8. À partir de combien de votants exprimés affichez-vous une mention majoritaire ?

**Enjeu.** À 3, 4 ou 5 votants, la mention majoritaire est un ordinal calculé sur un échantillon minuscule : elle est *bien définie* mathématiquement mais *sans contenu informationnel*, et la présenter comme un résultat est une faute.

**Réponses possibles.**
- *Aucun seuil : on affiche dès 1 votant.* Un groupe familial de 3 verra « À rejeter » ou « Très bien » basculer au gré d'un seul vote. Sur 3 votants, le déplacement d'une seule mention change la médiane dans la quasi-totalité des configurations.
- *Seuil à 5 exprimés pour afficher la mention, distribution visible dès 1.* La distribution brute (3 barres) est honnête à tout effectif ; la *mention* ne l'est pas.
- *Seuil à 10–15.* Statistiquement plus confortable, mais tue l'usage « famille » et « classe » qui est un pilier du produit.
- *Pas de seuil, mais un libellé différent : « tendance du groupe » au lieu de « mention majoritaire ».* Requalifie honnêtement l'objet sans l'interdire.

**Recommandation.** Sous 5 exprimés, afficher **la distribution seule, sans mention ni classement**, et nommer l'objet « tendance » — la médiane d'un groupe de 3 n'est pas un résultat de vote, c'est une anecdote.

---

## Q9. Peut-on fusionner les votes du jour et les votes d'archive dans un même calcul ?

**Enjeu.** Le jugement majoritaire **viole le critère de renforcement** (*join-consistency*) : une proposition peut gagner dans deux sous-populations et perdre dans leur réunion.

Contre-exemple vérifié à la main (règle de la jauge, échelle à 5 mentions, mention majoritaire = « Assez bien » = $g_3$ partout) :

| | $p$ (au-dessus) | à $g_3$ | $q$ (en dessous) | |
|---|---|---|---|---|
| **Jour J (n=100)** | | | | |
| X | 49 | 6 | 45 | $\max\{49,45,40,40\}=49=p_X$ → **X gagne** |
| Y | 40 | 20 | 40 | |
| **Archive (n=100)** | | | | |
| X | 20 | 40 | 40 | $\max\{20,40,15,44\}=44=q_Y$ → **X gagne** |
| Y | 15 | 41 | 44 | |
| **Fusion (n=200)** | | | | |
| X | 69 | 46 | 85 | $\max\{69,85,55,84\}=85=q_X$ → **Y gagne** |
| Y | 55 | 61 | 84 | |

X gagne dans chaque période et perd sur la fusion. Ce n'est pas un bug : c'est une propriété connue de la méthode.

**Réponses possibles.**
- *Fusion pure.* Produit des retournements inexplicables à l'utilisateur, et mélange des populations recrutées dans des contextes d'actualité différents (biais de composition en plus du paradoxe).
- *Séparation stricte : le résultat officiel est celui du jour J, gelé ; les rejouages alimentent un compteur séparé « depuis l'archive ».* Défendable, lisible, sans paradoxe.
- *Fusion avec fenêtre glissante (J à J+7).* Atténue sans supprimer le problème, et rend le résultat non reproductible.
- *Pondération temporelle des votes.* Sort du cadre axiomatique du JM (anonymat des votants) : à proscrire.

**Recommandation.** **Geler le résultat du jour J à minuit** et présenter les rejouages comme un corpus distinct, jamais additionné — c'est la seule option qui ne produit pas de retournement inexplicable.

---

## Q10. Un même utilisateur peut rejouer une question d'archive : son vote compte-t-il, et combien de fois ?

**Enjeu.** L'axiome d'anonymat et d'égalité des votants du JM s'effondre si un utilisateur peut voter dix fois sur la même question.

**Réponses possibles.**
- *Chaque rejouage compte.* Manipulation triviale par répétition ; le corpus d'archive devient inexploitable.
- *Un seul vote comptabilisé par utilisateur et par question (le premier).* Orthodoxe. Les rejouages ultérieurs sont un exercice personnel non comptabilisé.
- *Le dernier vote écrase le précédent.* Défendable (l'opinion évolue), mais rend le résultat d'archive non stable dans le temps.
- *Rejouage en mode « bac à sable » : on montre à l'utilisateur son vote face au résultat figé, sans rien modifier.* Le plus simple et le plus honnête pédagogiquement.

**Recommandation.** Mode bac à sable — le rejouage sert à *se situer*, pas à *voter*, et cela règle le problème sans arbitrage.

---

## Q11. Que faites-vous quand deux des quatre propositions sont logiquement incompatibles, ou quand l'une inclut l'autre ?

**Enjeu.** Le JM évalue chaque proposition **indépendamment** ; rien dans la méthode ne garantit que le classement produit soit logiquement cohérent, et l'app peut donc afficher un « gagnant » incompatible avec le deuxième, comme si les deux étaient conjointement approuvés.

**Réponses possibles.**
- *Ne rien faire, afficher le classement brut.* L'utilisateur lit « la France devrait faire A » en tête et « la France devrait faire non-A » en deuxième avec une bonne mention. Incohérence perçue attribuée à la méthode.
- *Contrainte de rédaction : les 4 propositions doivent être mutuellement exclusives et collectivement exhaustives (partition de l'espace des réponses).* Restaure l'interprétation « choix », mais appauvrit les questions et force l'IA dans un moule rigide.
- *Contrainte inverse : les 4 propositions sont explicitement **cumulables**, et la question est formulée « pour chacune de ces mesures, quelle est votre appréciation ».* Assume que le JM note des objets indépendants et supprime la lecture « élection ». C'est le cadrage le plus fidèle à la méthode.
- *Autoriser les relations d'inclusion mais les déclarer dans les métadonnées et afficher un avertissement de cohérence.* Coûteux, et transforme l'app en outil d'analyse logique.

**Recommandation.** Cadrer les propositions comme **mesures indépendamment évaluables** (« que vaut cette mesure ? ») et non comme options exclusives — c'est le seul cadrage où le JM est utilisé pour ce pour quoi il est fait : mesurer, pas choisir.

---

## Q12. Qu'appelez-vous « la proposition gagnante » : la mention majoritaire seule, ou le classement complet des quatre ?

**Enjeu.** Annoncer un gagnant unique quand quatre propositions partagent la même mention majoritaire donne un poids de vérité à un départage qui repose parfois sur quelques votes.

**Réponses possibles.**
- *Gagnant unique en gros, reste en petit.* Lisible, mais surinterprète le départage.
- *Classement des 4 avec la mention de chacune, gagnant mis en avant seulement si sa mention est **strictement supérieure** aux autres.* Distingue visuellement « gagne par la mention » et « gagne par le départage ».
- *Aucun gagnant : seulement les 4 distributions.* Fidèle mais illisible et sans récompense de lecture.
- *Gagnant + « marge » chiffrée.* Bonne idée si la marge est correctement définie (voir spécification), dangereuse si elle est inventée.

**Recommandation.** Classement complet, avec un **traitement typographique distinct quand le premier ne se détache que par le départage** — c'est la représentation qui ment le moins.

---

# STRUCTURANT

## Q13. Que signifie un vote qui attribue la même mention aux quatre propositions ?

**Enjeu.** Traiter ce vote comme du bruit, ou au contraire l'exclure, revient à décider unilatéralement de ce que veut dire un citoyen qui rejette (ou approuve) tout le cadrage de la question.

**Réponses possibles.**
- *Vote parfaitement valide, compté normalement.* Correct : il déplace les quatre médianes de façon égale et n'affecte le classement qu'à la marge. C'est un vote informatif sur la question, pas sur les propositions.
- *Vote signalé comme « rejet du cadrage » et remonté au producteur de questions.* Excellente métrique qualité : un taux élevé de « tout à rejeter » signale une question mal posée ou des propositions non pertinentes.
- *Vote exclu du calcul.* Injustifiable : rien dans l'axiomatique du JM ne permet d'écarter un bulletin cohérent.
- *Vote bloqué par l'UI (« vous devez différencier »).* Force un mensonge et corrompt les données.

**Recommandation.** Comptabiliser normalement **et** instrumenter le taux d'uniformité comme indicateur de qualité de la question — c'est un signal, pas une anomalie.

---

## Q14. Que mesure exactement le « profil de valeurs » de l'utilisateur, et sur quelles données ?

**Enjeu.** Un profil construit sur des mentions ordinales moyennées reproduit exactement l'erreur que le JM corrige.

**Réponses possibles.**
- *Moyenne des mentions converties en nombres.* À proscrire absolument : incohérent avec la méthode que l'app promeut.
- *Profil ordinal : fréquence de chaque mention, et taux d'accord avec la mention majoritaire collective.* Reste dans l'ordinal, honnête.
- *Profil par thème (économie, écologie, international) sur la base de la mention attribuée aux propositions étiquetées.* Plus riche, mais dépend entièrement de la qualité de l'étiquetage IA.
- *Pas de profil du tout, seulement une comparaison ponctuelle par question.* Le plus sobre, le moins engageant.

**Recommandation.** Profil strictement ordinal (distribution des mentions + taux de concordance), jamais de moyenne — sinon l'app contredit sa propre thèse dans son écran le plus consulté.

---

## Q15. Comment définissez-vous « la position de l'utilisateur par rapport à l'ensemble » ?

**Enjeu.** Une métrique mal choisie transforme une position ordinale en score de conformité, et suggère qu'être minoritaire est une erreur.

**Réponses possibles.**
- *Écart en nombre de crans entre la mention de l'utilisateur et la mention majoritaire, par proposition.* Simple, ordinal, lisible ; ne suppose pas que les crans sont égaux mais ne prétend pas non plus les additionner.
- *Percentile de l'utilisateur dans la distribution.* Correct statistiquement, mais parle de « plus sévère que 72 % des gens », ce qui est une information de conformité sociale.
- *Concordance sur le classement (l'utilisateur a-t-il le même podium que l'ensemble ?).* Le plus pertinent au regard de la méthode, puisque le JM produit un classement.
- *Score composite unique.* Séduisant en UI, indéfendable théoriquement.

**Recommandation.** Concordance sur le **classement** plus l'écart en crans proposition par proposition, sans score agrégé — le JM classe, donc la comparaison doit porter sur le classement.

---

## Q16. Comment comparez-vous un groupe privé à l'ensemble, sachant que le groupe n'est pas un échantillon de l'ensemble ?

**Enjeu.** Juxtaposer une médiane de 6 personnes et une médiane de 20 000 sans signaler l'asymétrie suggère un désaccord mesuré là où il n'y a que du bruit.

**Réponses possibles.**
- *Affichage côte à côte des deux mentions.* Trompeur si le groupe est minuscule.
- *Affichage des deux distributions, mentions du groupe masquées sous le seuil de Q8.* Cohérent avec Q8.
- *Affichage d'un indicateur « votre groupe est-il distinguable du hasard ? ».* Rigoureux mais demande un test statistique dont la formulation grand public est délicate.
- *Comparaison uniquement sur le classement (le groupe a-t-il le même podium ?), pas sur les mentions.* Plus robuste aux petits effectifs et plus parlant.

**Recommandation.** Comparer les **classements** et non les mentions, et appliquer le même seuil d'effectif qu'en Q8 — un désaccord de classement à 6 personnes reste discutable, mais un écart de mention à 6 personnes est du pur bruit.

---

## Q17. Affichez-vous une quelconque mesure d'incertitude sur la mention majoritaire ?

**Enjeu.** Sans incertitude, une mention obtenue à 51/49 et une mention obtenue à 85/5 se présentent identiquement.

**Réponses possibles.**
- *Rien.* Le plus courant, le moins honnête.
- *Afficher $p$ et $q$ (la jauge) sous la mention.* Gratuit à calculer, déjà nécessaire au départage, et dit exactement « combien pensent mieux / moins bien ». C'est la mesure native du JM.
- *Intervalle de confiance bootstrap sur la mention.* Rigoureux mais lourd, et l'intervalle sur un ordinal est difficile à lire.
- *Indicateur qualitatif (« mention nette » / « mention serrée ») calculé sur $|p-q|$.* Lisible ; à condition que le seuil soit documenté et non arbitraire.

**Recommandation.** Afficher $p$ et $q$ systématiquement — c'est la mesure d'incertitude propre à la méthode, elle ne coûte rien et elle enseigne le JM sans discours.

---

## Q18. Les résultats évoluent-ils en direct pendant la journée de vote, ou sont-ils figés ?

**Enjeu.** Un classement qui bascule en direct expose publiquement l'instabilité du départage et invite à la coordination de dernière minute.

**Réponses possibles.**
- *Direct total.* Effet de bandwagon, coordination facilitée, et l'utilisateur qui revient voit un résultat différent sans comprendre pourquoi.
- *Résultat figé, révélé à heure fixe.* Élimine l'influence intra-journée ; crée un rendez-vous. Coût : l'utilisateur qui vote à 8h attend.
- *Résultat individuel immédiat (sa position, la distribution actuelle) mais classement officiel figé.* Compromis : gratification immédiate sans effet d'entraînement sur le classement.
- *Direct avec un délai (résultats à J−1).* Décale le problème sans le résoudre.

**Recommandation.** Distribution visible immédiatement après le vote, **classement officiel figé et publié à heure fixe** — cela sépare la gratification de l'influence.

---

## Q19. L'utilisateur peut-il voir les résultats avant d'avoir voté ?

**Enjeu.** Le JM suppose un jugement sincère ; l'ancrage sur la distribution collective est le moyen le plus efficace de le détruire.

**Réponses possibles.**
- *Non, jamais avant vote.* Standard, protège la sincérité. Déjà prévu par le produit.
- *Oui pour les archives.* Contradictoire avec le mode « bac à sable » de Q10 si l'on veut mesurer un vrai jugement ; acceptable si le rejouage n'est pas comptabilisé.
- *Oui, avec avertissement.* Illusoire, l'ancrage est inconscient.
- *Non, et pas de partage de résultats permettant à un tiers de contourner.* Implique de gérer le partage social.

**Recommandation.** Verrou strict avant vote, y compris sur les liens partagés — et si le rejouage d'archive montre le résultat, alors il ne doit rien comptabiliser (cohérent avec Q10).

---

## Q20. Un groupe coordonné peut-il déplacer le résultat, et jusqu'où exactement ?

**Enjeu.** Le JM est réputé résistant à la manipulation, mais les résultats formels ne couvrent **ni la coordination ni l'information imparfaite** ; s'appuyer dessus sans nuance est une erreur de raisonnement.

**Ce que dit la théorie (vérifié).** Le JM est *strategy-proof* pour la mention majoritaire elle-même : un votant ne peut pas déplacer la mention d'une proposition dans la direction qu'il souhaite au-delà de ce que son jugement sincère produit. Il n'est que **partiellement** *strategy-proof* pour le **classement social**. Balinski et Laraki montrent que les règles à médiane la plus haute minimisent la part de l'électorat ayant intérêt à mentir — mais ce résultat ne s'applique **pas** en cas d'information imparfaite ou de collusion.

**Réponses possibles.**
- *Ne rien faire, invoquer la résistance du JM.* Faux sur le plan technique dès qu'il y a coordination.
- *Détecter les corrélations anormales (groupes d'utilisateurs votant à l'identique dans une fenêtre courte) et les signaler sans les exclure.* Mesuré ; exclure serait arbitraire.
- *Plafonner l'influence d'un groupe privé sur le résultat global.* Rompt l'anonymat/égalité des votants — sortie du cadre axiomatique.
- *Publier la volumétrie (nombre de votants, courbe horaire) pour rendre l'anomalie visible.* Transparence plutôt que police.

**Recommandation.** Transparence volumétrique plutôt que filtrage — et **ne jamais écrire dans l'app que le JM est « résistant à la manipulation »** sans la restriction « en information parfaite et hors collusion ».

---

## Q21. Comment traitez-vous un afflux soudain de votants (relais médiatique, brigade organisée) ?

**Enjeu.** Une question virale change de population de référence en cours de journée sans que rien ne le signale.

**Réponses possibles.**
- *Rien.* Le résultat de la journée devient le résultat d'une communauté ponctuelle, présenté comme celui d'« Endoxa ».
- *Publier la courbe d'arrivée des votes à côté du résultat.* Coût nul, information réelle.
- *Segmenter le résultat par cohorte (utilisateurs réguliers / nouveaux).* Informatif, mais introduit deux vérités.
- *Suspendre la publication en cas d'anomalie détectée.* Décision éditoriale forte, à assumer et à documenter publiquement.

**Recommandation.** Publier la courbe horaire des votes systématiquement — c'est le seul dispositif qui rende l'anomalie visible sans arbitrage éditorial.

---

## Q22. Que dites-vous à l'utilisateur qui découvre qu'exagérer ses mentions peut servir ses préférences ?

**Enjeu.** Prétendre que la sincérité est toujours optimale est faux et sera détecté ; ne rien dire laisse l'exagération se propager.

**Réponses possibles.**
- *Affirmer « avec le JM, il est inutile de voter stratégiquement ».* Techniquement faux sur le classement ; c'est une promesse que la méthode ne tient que partiellement.
- *Affirmer « avec le JM, exagérer sert moins qu'ailleurs ».* Vrai, prudent, et conforme au résultat de minimisation de la part de manipulateurs potentiels.
- *Ne rien dire.* Manque une occasion pédagogique alignée avec la mission de l'app.
- *Expliquer précisément le mécanisme (une mention au-delà de la médiane n'a plus d'effet marginal).* Juste, mais c'est exactement le cours de théorie du vote que le porteur veut éviter.

**Recommandation.** Formulation comparative et bornée (« exagérer rapporte moins ici qu'avec une note moyenne »), jamais absolue — une promesse fausse coûte plus cher que l'absence de promesse.

---

## Q23. Un utilisateur peut-il utiliser « sans avis » stratégiquement, et cela vous pose-t-il un problème ?

**Enjeu.** « Sans avis » sur les propositions concurrentes, mention haute sur la sienne : c'est une troncature, et le JM y est vulnérable (paradoxe de troncature documenté par Felsenthal & Machover).

**Réponses possibles.**
- *Accepter, l'exclusion des « sans avis » rend la manœuvre peu rentable.* Vrai en partie : ne pas juger ne baisse pas la médiane adverse, cela réduit seulement la base. Le gain stratégique est faible.
- *Obliger à noter les 4 propositions.* Supprime la troncature mais force des jugements insincères, ce qui est pire.
- *Compter « sans avis » comme mention la plus basse.* Rend la troncature rentable (c'est un rejet déguisé) : à proscrire ici.
- *Surveiller le taux de « sans avis » par utilisateur et le signaler dans son profil.* Curieux et intrusif pour un gain nul.

**Recommandation.** Laisser faire : avec exclusion pure, la troncature ne rapporte quasiment rien — c'est un argument en faveur de l'exclusion (Q6) plus qu'un problème en soi.

---

## Q24. Comment garantissez-vous que les 4 propositions rédigées par l'IA sont symétriques en désirabilité de formulation ?

**Enjeu.** Le JM agrège fidèlement des jugements ; il ne corrige rien d'un biais introduit à la rédaction, et une proposition mieux formulée gagnera sans que la méthode y soit pour quelque chose.

**Réponses possibles.**
- *Contrôle par prompt seul.* Insuffisant : les asymétries de longueur, de charge affective et de précision passent.
- *Contraintes formelles vérifiées automatiquement (longueur, absence d'adjectifs évaluatifs, structure syntaxique identique).* Mesurable, testable, faible coût.
- *Relecture humaine quotidienne.* Fiable, mais c'est un poste de travail.
- *Test A/B de formulations sur un panel avant publication.* Le plus rigoureux, incompatible avec un rythme quotidien.

**Recommandation.** Contraintes formelles automatiques + relecture humaine — parce que le biais de rédaction est aujourd'hui votre plus grande source d'erreur, bien devant le choix de la règle de départage.

---

## Q25. Quatre propositions et cinq mentions : avez-vous estimé la fréquence attendue des égalités sur la mention majoritaire ?

**Enjeu.** Si 70 % des questions se terminent par un départage, la promesse « le JM donne un gagnant clair » est fausse dans votre contexte particulier.

**Réponses possibles.**
- *Ne pas mesurer.* Vous découvrirez le problème après le lancement, dans les retours utilisateurs.
- *Simuler sur des distributions plausibles avant lancement et publier le chiffre en interne.* Une journée de travail, et cela conditionne les décisions Q2, Q4 et Q12.
- *Augmenter le nombre de mentions à 6 ou 7 pour réduire les égalités.* Effet réel, à arbitrer contre la lisibilité mobile (Q4).
- *Réduire à 3 propositions.* Réduit les égalités mécaniquement, appauvrit la question.

**Recommandation.** Simuler avant de figer l'échelle — le taux d'égalité attendu est le chiffre qui doit décider entre 5 et 6 mentions, pas l'esthétique de l'écran.

---

## Q26. Quelles représentations graphiques vous interdisez-vous ?

**Enjeu.** Certains graphiques réintroduisent visuellement la moyenne ou la cardinalité que le JM refuse, et enseignent donc l'inverse de ce que l'app veut promouvoir.

**Réponses possibles.**
- *Barres empilées à 100 % ancrées sur la mention majoritaire (diverging stacked bar), avec la mention marquée.* La représentation canonique du JM : elle montre $p$, $\alpha$ et $q$ d'un coup et respecte l'ordinal.
- *Jauge / compteur avec un score numérique.* Réintroduit une échelle cardinale : à proscrire.
- *Histogramme simple des 5 mentions.* Correct mais ne fait pas ressortir la médiane ni la jauge.
- *Radar / camembert.* Le camembert détruit l'ordre des mentions ; le radar suggère des dimensions indépendantes. À proscrire.

**Recommandation.** Barre empilée divergente centrée sur la mention majoritaire, jamais de score numérique unique — c'est la représentation qui *est* la méthode.

---

## Q27. Quand et comment montrez-vous que le résultat diffère de celui d'un scrutin classique ?

**Enjeu.** Une comparaison non qualifiée est trompeuse : les bulletins de JM ne contiennent pas l'information d'un bulletin de scrutin uninominal, donc « ce qu'aurait donné un vote classique » est une **reconstruction**, pas un fait.

**Réponses possibles.**
- *Comparaison systématique « au vote classique, c'est X qui aurait gagné ».* Trompeur : vous inférez un premier choix à partir de mentions, ce qui suppose une règle de conversion arbitraire (que fait-on quand deux propositions ont « Très bien » ?).
- *Comparaison uniquement quand la conversion est **non ambiguë** (un unique maximum strict par bulletin), avec la règle de conversion affichée.* Honnête, et rare — donc l'encart devient un événement, ce qui sert la pédagogie discrète.
- *Comparaison à la moyenne des mentions plutôt qu'à un scrutin classique.* Beaucoup plus défendable : les mêmes bulletins, deux agrégations. Aucune reconstruction. C'est la meilleure démonstration disponible.
- *Aucune comparaison.* Sûr, mais renonce à la mission.

**Recommandation.** Comparer **médiane vs moyenne sur les mêmes bulletins**, jamais JM vs scrutin uninominal reconstruit — c'est la seule comparaison qui ne fabrique aucune donnée, et c'est aussi la plus démonstrative.

---

# DÉTAIL

## Q28. Dans quel ordre affichez-vous les 4 propositions ?

**Enjeu.** L'effet de position (primauté / récence) fausse les mentions de quelques points, ce qui suffit à retourner un départage serré.

**Réponses possibles.** Ordre fixe (biais systématique) / aléatoire par utilisateur (annule le biais en agrégat, complique le partage de captures) / aléatoire mais stable par utilisateur (bon compromis) / ordre imposé par l'IA selon une logique éditoriale (biais non contrôlé).

**Recommandation.** Ordre aléatoire tiré par utilisateur et stable pour lui — annule le biais en agrégat sans dérouter l'utilisateur qui revient.

---

## Q29. L'utilisateur doit-il noter les 4 propositions pour valider son vote ?

**Enjeu.** Rendre l'exhaustivité obligatoire force des jugements insincères ; la rendre facultative sans défaut clair produit des bulletins partiels non intentionnels.

**Réponses possibles.** Obligation de renseigner les 4 (mention ou « sans avis ») / vote partiel accepté silencieusement / vote partiel avec confirmation explicite / pré-remplissage par défaut.

**Recommandation.** Obligation de **statuer** sur les 4 (mention *ou* « sans avis »), sans pré-remplissage — cela distingue l'abstention choisie de l'oubli.

---

## Q30. L'utilisateur peut-il modifier son vote après validation ?

**Enjeu.** Sans règle, on ne sait pas quel bulletin fait foi dans le résultat gelé.

**Réponses possibles.** Modification libre jusqu'à la clôture (dernier vote fait foi) / aucune modification / modification dans une fenêtre courte (5 min) / modification libre mais le résultat déjà affiché ne change pas visuellement (incohérence).

**Recommandation.** Fenêtre de correction courte, puis verrouillage — cela couvre l'erreur de manipulation sans ouvrir la porte au vote réactif au résultat.

---

## Q31. Comment arrondissez-vous et affichez-vous $p$ et $q$ ?

**Enjeu.** Deux valeurs affichées à 45 % qui décident d'un départage alors qu'elles valent 45,4 % et 44,6 % rendent le résultat incompréhensible.

**Réponses possibles.** Arrondi à l'entier (risque d'égalités apparentes) / une décimale (précis, moins lisible) / arrondi à l'entier + mention « départage serré » quand l'écart réel est sous 1 point / afficher les effectifs bruts plutôt que des pourcentages.

**Recommandation.** Effectifs bruts en petit sous les pourcentages arrondis — le calcul se fait toujours sur les entiers, jamais sur les pourcentages arrondis.

---

## Q32. Versionnez-vous la règle de calcul appliquée à chaque question archivée ?

**Enjeu.** Si vous changez de règle de départage dans six mois, les archives recalculées produiront des gagnants différents de ceux qui ont été affichés.

**Réponses possibles.** Stocker uniquement les bulletins et recalculer à la volée (résultats mouvants) / stocker le résultat figé (rigide mais reproductible) / stocker bulletins + identifiant de version de règle + résultat figé (le seul auditable) / ne rien versionner.

**Recommandation.** Bulletins + version de règle + résultat figé — le coût est nul et c'est ce qui rend l'app défendable en cas de contestation publique.

---

## Q33. Les données agrégées sont-elles exportables et le calcul reproductible par un tiers ?

**Enjeu.** Une app qui promeut le JM et dont le calcul est opaque discrédite la méthode qu'elle défend.

**Réponses possibles.** Rien d'exportable / export des distributions par question (suffisant pour recalculer tout le JM, sans donnée personnelle) / export des bulletins anonymisés (plus riche, risque de ré-identification par recoupement) / documentation publique de l'algorithme seulement.

**Recommandation.** Export public des **distributions** par proposition et par question, plus la documentation de l'algorithme — cela suffit à reproduire chaque résultat et ne crée aucun risque de ré-identification.

---

## Q34. Les libellés de mentions sont-ils traduisibles sans perte d'ordre perçu ?

**Enjeu.** Si l'app s'ouvre à d'autres langues, un lexique calibré sur le système scolaire français perd sa régularité perçue et les résultats deviennent incomparables entre langues.

**Réponses possibles.** Une seule langue / traduction littérale (perte de calibrage) / recalibrage par langue avec test de placement (coûteux, correct) / pas de comparaison inter-langues autorisée.

**Recommandation.** Traiter chaque langue comme une échelle distincte et interdire l'agrégation inter-langues — le « langage commun » est commun *dans une langue*, c'est toute la thèse de Balinski et Laraki.

---

# Spécification à figer

Chaque point ci-dessous doit être écrit noir sur blanc dans une spec, et chaque exemple doit devenir un test unitaire. Échelle de référence pour tous les tests : $g_5$ = Très bien > $g_4$ = Bien > $g_3$ = Assez bien > $g_2$ = Insuffisant > $g_1$ = À rejeter.

**S1 — Définition de la mention majoritaire.**
$\alpha$ = la plus basse mention $g$ telle que la part des votants exprimés attribuant une mention $\le g$ soit $\ge 50\%$.
*Test :* bulletins $\{g_5, g_4, g_3, g_2\}$ ($n=4$). Attendu : $\alpha = g_3$ (médiane basse). Un test qui renvoie $g_4$ signale une convention de médiane haute.

**S2 — Calcul de $p$ et $q$.**
$p$ = effectif strictement $> \alpha$, $q$ = effectif strictement $< \alpha$, sur la base des **exprimés uniquement**.
*Test :* 100 exprimés répartis $g_5$:30, $g_4$:25, $g_3$:10, $g_2$:20, $g_1$:15. Attendu : $\alpha = g_4$, $p = 30$, $q = 45$.

**S3 — Départage par la jauge, cas nominal.**
*Test :* A : $p=45$, $|\alpha|=45$, $q=10$. B : $p=48$, $|\alpha|=12$, $q=40$. Même $\alpha$.
Attendu **JM** : $\max\{45,10,48,40\} = 48 = p_B$ → **B gagne**.
Contrôle de divergence : jugement typique ($p-q$) donne A (35 > 8) ; jugement usuel ($ (p-q)/|\alpha|$) donne A (0,78 > 0,67) ; jugement central donne A (0,82 > 0,55). **Le test doit documenter que le JM est ici l'exception** — c'est la discontinuité relevée par Fabre.

**S4 — Départage par la jauge, maximum atteint plusieurs fois.**
*Test :* $n=3$. X = $\{g_5, g_3, g_1\}$ → $\alpha = g_3$, $p = 1/3$, $q = 1/3$. Y = $\{g_4, g_3, g_3\}$ → $\alpha = g_3$, $p = 1/3$, $q = 0$.
Le maximum $1/3$ est atteint par $p_X$, $q_X$ et $p_Y$ : **la jauge est indéterminée**. Le code doit basculer sur S5 et ne jamais renvoyer un résultat arbitraire.

**S5 — Départage récursif (règle de secours et définition normative).**
Retirer une occurrence de $\alpha$ de chaque multiensemble à égalité, recalculer $\alpha$, répéter.
*Test (suite de S4) :* X → $\{g_5, g_1\}$, $\alpha = g_1$ (médiane basse de 2). Y → $\{g_4, g_3\}$, $\alpha = g_3$. Attendu : **Y gagne**.

**S6 — Ex æquo irréductible.**
*Test :* deux propositions avec des multiensembles strictement identiques. Attendu : la récursion s'épuise, le code renvoie un statut `EX_AEQUO` explicite, et **aucun** tirage au sort n'est effectué.

**S7 — Exclusion des « sans avis » et quorum.**
*Test :* 100 votants, proposition Z : 60 « sans avis », exprimés = $g_5$:20, $g_1$:20. Attendu sur exprimés : $\alpha = g_1$, taux d'expression 40 %. Sous quorum 50 % → statut `NON_CONCLUANT`, Z exclue du classement, distribution affichée avec avertissement.

**S8 — Seuil d'effectif pour l'affichage de la mention.**
*Test :* groupe de 4 exprimés. Attendu : distribution affichée, **mention et classement masqués**, libellé « tendance » et non « mention majoritaire ».

**S9 — Non-additivité temporelle (échec du renforcement).**
*Test :* les trois tableaux de la Q9 (X gagne Jour J, X gagne Archive, Y gagne sur la fusion, toutes mentions majoritaires égales à $g_3$). Le test doit **passer**, c'est-à-dire reproduire le retournement — il documente que le comportement est une propriété de la méthode et non une régression. Conséquence de spec : **la fusion jour + archive est interdite dans le pipeline**.

**S10 — Idempotence du rejouage.**
*Test :* un utilisateur soumet trois bulletins successifs sur la même question d'archive. Attendu : le compteur officiel de la question est inchangé (mode bac à sable), ou exactement un bulletin est retenu si la comptabilisation est activée.

**S11 — Vote uniforme.**
*Test :* 10 votants sur 4 propositions, tous $g_1$ partout. Attendu : les 4 mentions valent $g_1$, $p = q = 0$ pour toutes, statut `EX_AEQUO` sur les 4, et l'indicateur `taux_uniformite = 1.0` remonté au pipeline qualité.

**S12 — Précision des calculs.**
Tous les départages se calculent sur des **effectifs entiers**, jamais sur des pourcentages arrondis.
*Test :* $n = 1001$, A : $p = 454$, B : $p = 453$, tous deux arrondis à 45 % à l'affichage. Attendu : A l'emporte, et l'UI affiche l'indicateur « départage serré ».

**S13 — Versionnage.**
*Test :* recalcul d'une question archivée avec `regle_version = 1` alors que la version courante est 2. Attendu : le résultat historique est restitué à l'identique, et non recalculé avec la règle courante.

---

# Sources vérifiées

> Avertissement : l'accès direct aux pages étant bloqué dans cet environnement, ces sources ont été consultées via extraits de résultats de recherche. Les définitions formelles (S1, S2, S5) doivent être confrontées à l'ouvrage de référence avant implémentation.

**Sources primaires et de référence**
- Michel Balinski, Rida Laraki, *Majority Judgment: Measuring, Ranking, and Electing*, MIT Press — https://mitpress.mit.edu/9780262545716/majority-judgment/ (recommandation de 6 mentions ; langage commun ; *strategy-proofness* partielle sur le classement)
- Balinski & Laraki, « Le jugement majoritaire : l'expérience d'Orsay », *Commentaire*, 2007 — https://cairn.info/revue-commentaire-2007-2-page-413.htm (échelle à 6 mentions testée en conditions réelles, présidentielle 2007)
- Balinski & Laraki, présentation au Collège de France — https://www.college-de-france.fr/media/pierre-rosanvallon/UPL8954465031560637643_Balinski__Laraki.pdf
- Page « Majority Judgment » de Rida Laraki — https://sites.google.com/site/ridalaraki/majority-judgment-jugement-majoritaire

**Règles de départage et alternatives**
- Adrien Fabre, « Tie-breaking the highest median: alternatives to the majority judgment », *Social Choice and Welfare* 56(1), 2021 — https://link.springer.com/article/10.1007/s00355-020-01269-9 ; version HAL : https://shs.hal.science/halshs-04363059v1 (jugement typique, usuel, central ; continuité du jugement usuel vs discontinuité du départage de Balinski-Laraki)
- Jugement majoritaire — Wikipédia (fr) : https://fr.wikipedia.org/wiki/Jugement_majoritaire (mention majoritaire vs mention médiane ; convention en effectif pair ; règle récursive)
- Jugement usuel — Wikipédia (fr) : https://fr.wikipedia.org/wiki/Jugement_usuel (signe $\pm$ de la mention majoritaire selon $p$ vs $q$)
- Majority Judgment — electowiki : https://electowiki.org/wiki/Majority_Judgment (procédure de retrait successif des mentions médianes)
- Majority Judgment (Balinski-Laraki) Calculator, MetricGate : https://metricgate.com/docs/majority-judgment-balinski/ (formulation de la jauge $(p, \alpha, q)$ ; tri par $\alpha$, puis $\mathrm{sign}(p-q)$, puis $\max(p,q)$)
- Mieux Voter, « Le jugement majoritaire » : https://mieuxvoter.fr/le-jugement-majoritaire et FAQ https://app.mieuxvoter.fr/fr/faq (règle de la « plus grande des 4 valeurs » telle qu'implémentée en production ; convention « ne pas cocher = rejeter » dans le cadrage électoral)

**Critiques et pathologies**
- Jean-François Laslier, « L'étrange "jugement majoritaire" », *Revue économique* 70(4), 2019 — https://shs.cairn.info/revue-economique-2019-4-page-569 ; préprint : https://shs.hal.science/halshs-01545883v1/document (non-conformité à Condorcet ; la médiane néglige la moitié de l'électorat)
- Michel Balinski, « Réponse à des critiques du jugement majoritaire », *Revue économique* 70(4), 2019 — https://www.cairn.info/revue-economique-2019-4-page-589.htm
- Dan Felsenthal & Moshé Machover, « Review of Paradoxes Afflicting Various Voting Procedures » — https://www.lse.ac.uk/cpnss/assets/documents/voting-power-and-procedures/workshops/2010/duBaffy2010-Felsenthal.pdf (JM vulnérable aux paradoxes de Condorcet, de troncature, de renforcement, du non-votant et du jumeau)
- Friedemann Kemm, « Why Majority Judgement is not yet the solution for political elections, but can help finding it », arXiv:2302.10858 — https://arxiv.org/abs/2302.10858 (limites des règles de départage ; traitement des abstentions)
- Balinski & Laraki, « Majority judgment vs. majority rule », *Social Choice and Welfare*, 2020 — https://link.springer.com/article/10.1007/s00355-019-01200-x
- « Majority judgment and strategy-proofness: a characterization », *International Journal of Game Theory*, 2019 — https://link.springer.com/article/10.1007/s00182-019-00666-4 (le JM n'est pas *coalitionally strategy-proof*)
- RangeVoting.org, critique de la méthode : https://rangevoting.org/MedianVrange.html
- CorteX, « Le jugement majoritaire et ses alternatives » : https://cortecs.org/chiffres-graphiques/le-jugement-majoritaire-et-ses-alternatives/

**Conception d'échelle**
- Échelle de Likert — Wikipédia (fr) : https://fr.wikipedia.org/wiki/%C3%89chelle_de_Likert
- « Échelles de Likert : 5 ou 7 points et erreurs à éviter » : https://www.tamers.fr/echelles-likert-5-7-points-erreurs-codage/ (effet de centralité ; recommandations d'Alwin & Krosnick sur la fiabilité selon le nombre de points ; dégradation au-delà de 7)

---

**Les trois décisions qui bloquent tout le reste :** Q2 (règle de départage normative), Q4 (5 ou 6 mentions) et Q9 (interdiction de fusionner jour et archives). Les vingt autres questions se déduisent en grande partie de ces trois-là.
