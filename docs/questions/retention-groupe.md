# Questions — Rétention et dynamique de groupe

> Produit par le rôle correspondant de l'équipe (voir `docs/methode.md`).
> Questions, pas solutions. Niveaux : **BLOQUANT**, **STRUCTURANT**, **DÉTAIL**.

---

**Légende :** `BLOQUANT` = la réponse change la nature du produit, à trancher avant tout développement. `STRUCTURANT` = à trancher avant la v1, réversible à coût élevé. `DÉTAIL` = à trancher pendant, réversible à coût faible.

---

## 1. L'usure de la promesse introspective

### Q1 — Au bout de combien de votes le profil de valeurs cesse-t-il de bouger de façon perceptible ? `BLOQUANT`

**Enjeu.** Si le profil se stabilise en trois semaines, le moteur de rétention n°1 est mort au jour 21 et tout le reste doit compenser.

**Options.**
- **A. On ne le sait pas et on lance quand même.** → Le produit découvre son plafond de rétention en production, sur de vrais utilisateurs qu'on ne récupérera pas. Coût : la première cohorte, qui est aussi la plus précieuse.
- **B. On le simule avant de coder** (données synthétiques + 100 répondants réels sur 30 questions) → on obtient une courbe de convergence chiffrée en 3 semaines, et on sait si le point d'inflexion est à J15, J60 ou J200.
- **C. On ralentit artificiellement la convergence** (le profil se « débloque » par paliers) → rétention mécanique, mais c'est un mensonge sur la donnée : le profil affiché n'est plus le profil calculé. Incompatible avec la promesse d'honnêteté.
- **D. On assume la stabilisation et on en fait l'événement** : « ton profil est stable, voici ce que ça veut dire » → transforme la fin de la construction en livrable plutôt qu'en abandon.

**Reco.** B puis D — mesurer la courbe avant de coder, parce que la date de stabilisation est le paramètre qui détermine à lui seul si ce produit est une app quotidienne ou un test qu'on passe une fois.

---

### Q2 — Que promet-on précisément à l'utilisateur au jour 90, quand il n'y a plus rien à découvrir sur soi ? `BLOQUANT`

**Enjeu.** Sans réponse à cette question, Endoxa est un produit à durée de vie de trois mois qui se croit quotidien.

**Options.**
- **A. Rien de nouveau — on mise sur l'habitude acquise.** → Optimiste : les rituels quotidiens sans nouveauté (Wordle, BeReal) décrochent massivement entre M3 et M6. Attendre 10-20 % de rétention J90.
- **B. On passe de l'état à la trajectoire** : « tu as changé d'avis 14 fois ce trimestre, sur quoi, dans quel sens ». La matière devient le mouvement, qui lui ne s'épuise pas.
- **C. On passe du soi au groupe** : à J90 la valeur bascule sur « où je me situe dans mon cercle », qui bouge à chaque nouveau membre et chaque sujet.
- **D. On introduit de la profondeur** (sous-dimensions Schwartz, conflits de valeurs internes, cohérence déclaré/choisi) → repousse le mur de 3 à 9 mois, ne l'enlève pas.

**Reco.** B en priorité, C en renfort — la dérive et l'incohérence sont les deux seules matières premières qui se renouvellent indéfiniment, contrairement au portrait.

---

### Q3 — « Se connaître » est-il un besoin récurrent ou un besoin ponctuel ? `BLOQUANT`

**Enjeu.** Toute l'architecture (quotidienne, notifiée, rituelle) suppose un besoin récurrent, alors que l'histoire des produits d'introspection dit l'inverse.

**Options.**
- **A. Récurrent** → on garde le format quotidien, et on accepte de devoir prouver ce postulat contre les données du marché.
- **B. Ponctuel, réactivé par événements** (élection, rupture, changement de job, rentrée) → l'app devient saisonnière : quelques sessions intenses par an, notifications rares, pas de streak. Modèle économique différent.
- **C. Ponctuel pour l'individu, récurrent pour le groupe** → le quotidien n'est justifié que par le collectif, ce qui invalide la v1 solo comme test.
- **D. Récurrent mais faible** (1-2 fois par semaine, pas quotidien) → question du jour devient question de la semaine, la charge éditoriale chute de 85 %, et la promesse tient mieux.

**Reco.** D est probablement la vérité et personne ne veut l'entendre — tester explicitement un rythme hebdomadaire contre un rythme quotidien, parce que le quotidien est un choix par défaut jamais justifié.

---

### Q4 — Accepte-t-on de concevoir un produit qu'on quitte volontairement après six mois ? `STRUCTURANT`

**Enjeu.** Un produit d'introspection honnête a une fin ; le refuser conduit mécaniquement aux mécaniques qu'on s'interdit par ailleurs.

**Options.**
- **A. Non, on vise l'usage à vie** → contradiction interne : on finira par ajouter des streaks et des relances, ou par échouer.
- **B. Oui, avec une sortie digne** : bilan annuel, export du profil, « reviens quand tu veux » → réputation excellente, rétention chiffrée médiocre, investisseurs impossibles.
- **C. Oui, avec réactivation événementielle** (chaque élection, chaque rentrée, chaque anniversaire de profil) → cycle de vie long et discontinu, plus honnête que le quotidien forcé.
- **D. Oui pour le solo, non pour le groupe** — l'individu sature, le groupe non.

**Reco.** C+D, écrit noir sur blanc dans les principes produit, parce qu'un objectif de rétention non assumé est ce qui fait dériver éthiquement les équipes, pas la mauvaise foi.

---

### Q5 — Les archives rejouables modifient-elles le profil rétroactivement ? `DÉTAIL`

**Enjeu.** Si oui, le profil devient manipulable par l'utilisateur ; si non, l'archive est un jeu sans conséquence donc sans intérêt.

**Options.**
- **A. Oui, tout compte** → un utilisateur peut « sculpter » son profil en rejouant 50 archives, la donnée perd son sens.
- **B. Non, l'archive est un bac à sable** → sans enjeu, peu rejoué.
- **C. Oui mais tracé séparément** : « ton profil du direct » vs « ton profil complet », l'écart devient une information en soi.
- **D. Oui, pondéré par la date** (un vote a posteriori pèse moins).

**Reco.** C — l'écart entre ce qu'on répond sous pression de l'actualité et ce qu'on répond à froid est exactement le genre de révélation que ce produit promet.

---

## 2. La validité du profil de valeurs

### Q6 — Comment prouve-t-on que le profil Schwartz calculé n'est pas de l'astrologie sophistiquée ? `BLOQUANT`

**Enjeu.** Inférer des valeurs universelles à partir de positions sur l'actualité n'est pas une opération scientifiquement établie ; si le profil est faux, la promesse centrale est un mensonge.

**Options.**
- **A. On ne prouve rien, on assume le registre du « miroir qui fait réfléchir ».** → Fonctionne commercialement (Co-Star, 16Personalities), incompatible avec un positionnement d'honnêteté, et fragile face à un article de presse un peu sérieux.
- **B. Validation externe** : 100 personnes passent le PVQ-21 officiel + 30 questions Endoxa, on corrèle. Coût ~300 €, 2 semaines. Si r < 0,4 sur les axes principaux, on sait.
- **C. On abandonne Schwartz** pour un vocabulaire propre à l'app, non scientifique mais non usurpé (« tu privilégies X quand X et Y s'opposent »).
- **D. On garde Schwartz mais en affichant l'incertitude** (« estimation basée sur 23 votes, fiabilité moyenne »).

**Reco.** B avant tout développement, puis D — parce qu'une promesse de connaissance de soi adossée à une mesure non validée est le seul risque qui peut tuer le projet par sa réputation plutôt que par ses chiffres.

---

### Q7 — Que fait-on de l'utilisateur qui ne se reconnaît pas dans son profil ? `STRUCTURANT`

**Enjeu.** C'est le moment de vérité du produit, et il arrive à tout le monde au moins une fois.

**Options.**
- **A. Rien, il part.** → Perte sèche du segment le plus réflexif, celui qui aurait parlé de l'app.
- **B. « Conteste ton profil »** : un bouton qui déclenche 5 questions ciblées pour affiner → transforme le désaccord en engagement, et améliore le modèle.
- **C. On explique le calcul** (« ce score vient de tes votes des 12/03, 15/03… ») → transparence radicale, coûteuse à construire, extrêmement différenciante.
- **D. On atténue tout** (profils flous, formulations Barnum) → tout le monde se reconnaît, personne n'apprend rien.

**Reco.** B+C — la traçabilité vote → valeur est le seul argument qui distingue Endoxa d'un test de magazine, et c'est aussi la meilleure défense juridique et médiatique.

---

### Q8 — Une session de 3 minutes tient-elle vraiment : jugement majoritaire sur 4 propositions + justification + origine de la conviction ? `STRUCTURANT`

**Enjeu.** Le jugement majoritaire demande 4 évaluations graduées, pas un choix — c'est 3 à 5 fois la charge cognitive annoncée.

**Options.**
- **A. On garde tout et on chronomètre en vrai** (10 testeurs, papier ou Figma) → on saura en 3 jours si c'est 3 ou 7 minutes.
- **B. On allège le MJ** (3 propositions, échelle à 4 mentions au lieu de 6-7).
- **C. On rend la justification et l'origine optionnelles** → -40 % de charge, mais l'origine de la conviction est probablement la donnée la plus intéressante du produit.
- **D. On alterne** : jour court (vote seul) / jour long (vote + justification + origine).

**Reco.** A immédiatement puis D — parce que 7 minutes par jour n'est pas un défaut de design, c'est un produit différent, et il vaut mieux le savoir avant.

---

### Q9 — Que se passe-t-il quand aucune des 4 justifications ne correspond ? `STRUCTURANT`

**Enjeu.** Forcer une justification fausse corrompt le profil et produit exactement le sentiment de non-reconnaissance de la Q7.

**Options.**
- **A. On force le choix** → donnée polluée, frustration silencieuse.
- **B. « Aucune des quatre »** comme cinquième option comptabilisée → donnée honnête, et signal éditorial précieux sur la qualité des propositions.
- **C. Champ libre** → riche, mais modération, coût, et impossible à intégrer au calcul.
- **D. B + champ libre facultatif non calculé**, lu par l'éditeur.

**Reco.** D — le taux de « aucune des quatre » devient l'indicateur qualité n°1 du travail éditorial, ce qui est bien plus utile qu'un taux de complétion.

---

## 3. La v1 solo alors que la cible est le groupe

### Q10 — Qu'est-ce que la v1 solo est censée valider, exactement, en une phrase ? `BLOQUANT`

**Enjeu.** Si la réponse n'est pas écrite avant le développement, la v1 validera « les gens installent une app gratuite », ce qui ne prouve rien.

**Options.**
- **A. La qualité du contenu quotidien et la faisabilité éditoriale.** → Objectif légitime et atteignable, mais il ne nécessite pas une app.
- **B. La rétention solo.** → Piège : la rétention solo mesure la mauvaise promesse, et un mauvais résultat tuera à tort un produit dont le moteur est social.
- **C. La reconnaissance du profil** (« est-ce que ce miroir dit vrai ? ») → c'est la vraie question fondatrice, et elle se teste sans app.
- **D. Rien — la v1 solo est un choix de facilité technique déguisé en stratégie.** → À envisager sérieusement.

**Reco.** Écrire l'objectif en une phrase falsifiable avant tout, et si cette phrase est « la rétention solo », renoncer à la v1 solo — on ne valide pas un produit social en supprimant le social.

---

### Q11 — Quel résultat chiffré de la v1 solo déclencherait un arrêt, et lequel déclencherait la v2 groupes ? `BLOQUANT`

**Enjeu.** Sans seuils fixés d'avance, tout résultat sera interprété comme encourageant.

**Options.**
- **A. Pas de seuils** → biais de confirmation garanti, 18 mois de développement en pure perte.
- **B. Seuils sur l'engagement** (J7, J30) → mesure ce qu'on a dit ne pas vouloir mesurer.
- **C. Seuils sur la valeur perçue** : ≥ 40 % des utilisateurs à J30 déclarent avoir appris quelque chose sur eux, ≥ 25 % en ont parlé à quelqu'un.
- **D. Seuil sur l'intention groupe** : ≥ 30 % des utilisateurs solo demandent spontanément à inviter quelqu'un.

**Reco.** C+D, écrits et datés avant la première ligne de code — D en particulier, car c'est le seul signal que la v1 solo peut légitimement donner sur la v2.

---

### Q12 — Peut-on tester le groupe dès maintenant sans application, via WhatsApp ? `BLOQUANT`

**Enjeu.** La vraie promesse est testable en 4 semaines pour quelques centaines d'euros ; construire une v1 solo à la place est un contournement coûteux.

**Options.**
- **A. Oui, protocole manuel** : 6 groupes réels, question envoyée à la main chaque jour, vote par formulaire, profil renvoyé le dimanche. → On apprend en un mois ce que la v1 solo n'apprendra jamais.
- **B. Non, trop artisanal, ça ne prouve rien.** → Argument classique et faux : si ça ne marche pas en manuel avec des groupes motivés et un humain qui relance, ça ne marchera jamais en automatique.
- **C. Oui mais après la v1 solo** → inversion de l'ordre d'apprentissage, 6 à 12 mois perdus.

**Reco.** A, avant tout développement, parce que le protocole manuel teste l'hypothèse centrale et que la v1 solo teste une hypothèse secondaire.

---

### Q13 — Le mode solo est-il un état permanent légitime ou un sas vers le groupe ? `STRUCTURANT`

**Enjeu.** Ça détermine si l'app doit être agréable seule pendant des mois, ou si elle doit pousser à inviter — deux produits opposés.

**Options.**
- **A. Sas** → l'app harcèle poliment pour inviter, l'utilisateur sans groupe se sent en échec dès la semaine 1.
- **B. Permanent** → l'expérience solo doit tenir 6 mois seule (voir Q2), ce qui est le problème le plus dur du projet.
- **C. Solo + comparaison à la moyenne nationale** comme groupe par défaut → l'utilisateur seul a un « autre » face à lui dès le jour 1, sans dépendre de ses proches.
- **D. Solo + groupes ouverts thématiques** (par ville, par âge) en attendant ses proches.

**Reco.** C — la moyenne générale est un partenaire de comparaison gratuit, disponible immédiatement, et qui résout à la fois le démarrage à froid et le groupe mort.

---

## 4. Le groupe : amorçage, taille, mort, déséquilibre

### Q14 — Combien d'invitations faut-il envoyer pour obtenir un groupe vivant de 5 personnes à 4 semaines ? `BLOQUANT`

**Enjeu.** Le porteur du projet imagine « j'invite 4 proches » ; la réalité est plutôt 15 à 20 invitations pour 5 actifs à J28.

**Options.**
- **A. On ne chiffre pas** → le premier utilisateur se retrouve seul, conclut que l'app est vide, et part. C'est le scénario le plus probable.
- **B. On mesure l'entonnoir sur les 6 groupes du test manuel** : invitation → installation → premier vote → vote à J28. Chiffre réel disponible en un mois.
- **C. On rend le groupe fonctionnel à 2** (voir Q16) → seuil d'amorçage divisé par deux.
- **D. On amorce par le haut** : on recrute des « animateurs » (prof, président d'asso, organisateur de famille) qui invitent 20 personnes d'un coup.

**Reco.** B pour connaître le chiffre, D pour la stratégie d'acquisition — un groupe ne s'amorce pas par un pair mais par quelqu'un qui a déjà une autorité de convocation.

---

### Q15 — Que coûte socialement l'échec d'une invitation à celui qui invite ? `STRUCTURANT`

**Enjeu.** Inviter sa famille à une app d'opinions politiques et essuyer quatre refus est une petite humiliation qui vaccine définitivement.

**Options.**
- **A. On ignore** → l'inviteur ne réessaiera pas, et c'est le seul canal d'acquisition du produit.
- **B. L'invitation ne montre jamais les refus** (pas de « 3 en attente » affiché en permanence) → réduit la honte, mais réduit aussi l'information utile.
- **C. On fournit le message tout fait**, court, non prosélyte, qui rend l'invitation facile à refuser sans gêne.
- **D. On invite à une occasion plutôt qu'à une app** : « la question du dimanche en famille », un one-shot, sans engagement quotidien.

**Reco.** D+C — demander à ses proches un essai unique plutôt qu'un engagement quotidien divise par trois le coût de la demande, et l'engagement se construit après.

---

### Q16 — En dessous de combien de membres l'anonymat des votes devient-il fictif ? `BLOQUANT`

**Enjeu.** À 3 ou 4 personnes, avec des profils de valeurs visibles et 4 propositions, tout le monde devine qui a voté quoi — et l'app aura promis l'anonymat.

**Options.**
- **A. On promet l'anonymat quand même** → promesse fausse, risque de conflit familial réel, et faute éthique caractérisée. À exclure.
- **B. Seuil d'affichage** : les résultats du groupe n'apparaissent qu'à partir de 5 votants, sinon on affiche seulement la comparaison à la moyenne générale.
- **C. On supprime l'anonymat en petit groupe et on l'assume** : « ici, tout le monde voit tout » → cohérent, mais change radicalement ce qu'on ose voter en famille.
- **D. On floute** (agrégats sur 7 jours, pas de résultat par question).

**Reco.** B, non négociable, avec une phrase explicite à la création du groupe — c'est un point où l'erreur ne se répare pas, parce qu'elle se paie en relations réelles, pas en désinstallations.

---

### Q17 — Au-dessus de combien de membres un « groupe » cesse-t-il d'être vécu comme un groupe ? `DÉTAIL`

**Enjeu.** Une classe de 32 et une famille de 6 n'ont pas la même dynamique ; au-delà d'un certain seuil on passe de l'intime au sondage.

**Options.**
- **A. Pas de limite** → les grands groupes deviennent des audiences, les profils individuels ne sont plus regardés, la promesse se dilue.
- **B. Plafond à 12-15** → protège la promesse, exclut la cible « classe ».
- **C. Deux objets distincts** : « cercle » (≤ 12, profils visibles) et « assemblée » (> 12, agrégats seulement).
- **D. Sous-groupes automatiques** dans les grands ensembles.

**Reco.** C — la visibilité des profils individuels n'a de sens que dans un cercle où l'on connaît chaque personne, et confondre les deux échelles abîme les deux.

---

### Q18 — Que fait l'application quand un groupe meurt : un membre continue, quatre ont décroché ? `STRUCTURANT`

**Enjeu.** Le cas le plus fréquent après 6 semaines, et celui qui produit le sentiment le plus toxique : « je suis seul et tout le monde le voit ».

**Options.**
- **A. On n'affiche rien de spécial** → le survivant voit chaque jour son vote seul sur cinq lignes vides, humiliation quotidienne, départ garanti.
- **B. Mise en veille automatique** : après 14 jours sans quorum, le groupe passe en sommeil, l'écran groupe disparaît, remplacé par la comparaison nationale.
- **C. Relance des inactifs** → transforme le produit en machine à culpabiliser, exactement ce qu'on s'interdit.
- **D. Réveil sur événement** : un membre peut « réveiller le groupe » une fois par mois avec une question choisie.

**Reco.** B avec D comme sortie — un groupe qui s'endort proprement peut se réveiller ; un groupe qui affiche son cadavre tous les jours ne revient jamais.

---

### Q19 — Comment évite-t-on qu'un rapport 1 actif / 4 passifs soit vécu comme un échec par l'actif et une pression par les passifs ? `BLOQUANT`

**Enjeu.** C'est la distribution normale, pas l'exception : dans tout petit groupe, 20 % font 80 % de l'activité.

**Options.**
- **A. On affiche la participation individuelle** (« Marie a voté 24/30, Paul 3/30 ») → pression, gêne, et conflit importé dans une famille réelle.
- **B. On n'affiche jamais rien d'individuel sur l'assiduité**, seulement le contenu des positions → le passif n'est pas en dette, l'actif n'est pas en attente.
- **C. On valorise la participation intermittente** : « Paul a voté 3 fois ce mois-ci, et il est le seul du groupe à avoir choisi X » → le rare devient précieux plutôt que déficient.
- **D. Rôles asymétriques assumés** : un « porteur » qui pose la question, des « répondants » qui répondent quand ils veulent.

**Reco.** B + C — supprimer toute métrique d'assiduité visible et rendre le vote rare valorisable est la seule façon de ne pas transformer un groupe familial en tableau de présence.

---

### Q20 — La notification « tout le groupe a voté » n'est-elle pas une notification de pression déguisée ? `STRUCTURANT`

**Enjeu.** Sa contrapposée est « il manque quelqu'un », et dans un groupe de 5 tout le monde sait qui.

**Options.**
- **A. On la garde telle quelle** → mécanique de complétion sociale, efficace et exactement du type qu'on prétend s'interdire.
- **B. On la supprime** → on perd le seul déclencheur collectif du produit.
- **C. On la déplace** : notification quand les résultats sont révélés (à heure fixe, quel que soit le nombre de votants) → le rendez-vous est temporel, pas conditionné aux autres.
- **D. On la conditionne au quorum, pas à l'unanimité** (3 votants sur 5 suffisent).

**Reco.** C — un rendez-vous à heure fixe crée le même rituel collectif sans jamais désigner un retardataire, et c'est la ligne exacte entre rituel et pression.

---

### Q21 — Famille, classe, association, équipe : est-ce le même produit ? `BLOQUANT`

**Enjeu.** Ces quatre contextes n'ont ni le même rythme, ni le même rapport de pouvoir, ni le même risque — et « équipe de travail » + opinions politiques est un problème juridique autant qu'un problème de design.

**Options.**
- **A. Un produit unique pour les quatre** → il sera médiocre pour les quatre, et dangereux pour deux.
- **B. On choisit la famille/le cercle d'amis** comme cible unique de v1 → rythme quotidien plausible, pas de hiérarchie, risque maîtrisé.
- **C. On choisit la classe** → usage hebdomadaire piloté par l'enseignant, mineurs (RGPD, consentement parental), marché B2B éducation, produit très différent.
- **D. On choisit l'entreprise** → à écarter : croiser opinions politiques et lien de subordination est un risque juridique et humain disproportionné.

**Reco.** B pour la v1, C comme piste distincte plus tard, D à écarter explicitement — un produit qui vise quatre contextes au démarrage n'en sert aucun.

---

## 5. Le contenu quotidien et sa charge émotionnelle

### Q22 — Qui écrit la question du jour, ses 4 propositions et ses 4 justifications, tous les jours, y compris le 25 décembre ? `BLOQUANT`

**Enjeu.** C'est le vrai coût de fonctionnement du produit, très supérieur au coût technique, et il ne baisse jamais avec l'échelle.

**Options.**
- **A. Le porteur du projet, seul.** → 1 à 2 h par jour de travail qualifié, soit ~40 h/mois, indéfiniment. Tenable 3 mois, pas 24.
- **B. Un stock d'avance** (90 questions intemporelles écrites en amont) → décorrèle de l'actualité, supprime l'urgence, mais perd la promesse « question d'actualité ».
- **C. IA générative + relecture humaine** → coût technique dérisoire (quelques euros/mois), coût de relecture ~20 min/jour, mais risque de biais et de fadeur à surveiller de très près.
- **D. Contributions de la communauté** → gratuit, ingérable en modération avant plusieurs milliers d'utilisateurs.

**Reco.** C avec relecture systématique et un stock de secours façon B, parce qu'une chaîne éditoriale reposant sur la disponibilité quotidienne d'une seule personne est le point de rupture le plus certain du projet.

---

### Q23 — Que fait l'application le jour d'un attentat, d'une catastrophe ou d'un drame national ? `BLOQUANT`

**Enjeu.** Une notification « voici la question du jour » à 18 h le jour d'un attentat est une faute qui se paie en captures d'écran.

**Options.**
- **A. Rien de prévu** → la faute arrivera, statistiquement, dans les 24 premiers mois.
- **B. Interrupteur manuel** : le porteur peut basculer sur une question de repli intemporelle et couper les notifications en une action.
- **C. Règle automatique** (détection d'actualité) → non fiable, à écarter.
- **D. Politique écrite d'avance** : critères de bascule, qui décide, quel message s'affiche, en combien de temps.

**Reco.** B+D, écrits avant le lancement — c'est un plan de crise d'une page qui évite le seul type d'incident capable de tuer la réputation du produit en un après-midi.

---

### Q24 — Quel est le ratio entre actualité politique lourde et dilemmes intemporels ? `STRUCTURANT`

**Enjeu.** Voter chaque jour sur retraites, immigration et guerre a un coût émotionnel réel qui produit un décrochage silencieux, jamais exprimé en verbatim.

**Options.**
- **A. 100 % actualité** → fatigue en 3 à 5 semaines, et corrélation de l'usage au cycle médiatique (voir Q42).
- **B. 100 % intemporel** → plus doux, mais perd l'ancrage et la conversation de groupe.
- **C. Alternance structurée** : 3 jours d'actualité, 2 jours de dilemmes personnels/éthiques, 2 jours légers → varie la charge, élargit le spectre Schwartz (l'actualité ne couvre pas Hédonisme ou Bienveillance).
- **D. L'utilisateur règle son curseur** « intensité » → autonomie réelle, complexité éditoriale multipliée.

**Reco.** C — l'alternance est aussi la seule façon de couvrir les 10 valeurs de Schwartz, que l'actualité politique seule ne permet pas d'atteindre.

---

### Q25 — Qui garantit la neutralité éditoriale, et comment le prouve-t-on à un utilisateur qui accuse l'app d'être orientée ? `BLOQUANT`

**Enjeu.** Une seule question mal cadrée suffit à faire basculer la perception vers « app militante », et sur ce sujet la perception ne se rattrape pas.

**Options.**
- **A. Bonne foi du rédacteur** → insuffisant, et impossible à défendre publiquement.
- **B. Charte éditoriale publique + relecture croisée** par deux personnes d'orientations différentes → coût réel, crédibilité forte.
- **C. Indicateur interne** : distribution des votes par question ; une question dont 85 % des utilisateurs choisissent la même proposition est mal construite.
- **D. Comité consultatif bénévole** (chercheurs, association type Mieux Voter) → crédibilité externe, lenteur.

**Reco.** B+C — l'indicateur de dispersion des réponses est un contrôle qualité automatique, objectif et gratuit, qui repère les questions biaisées mieux que l'intuition.

---

### Q26 — « Je préfère ne pas répondre » est-il une réponse légitime, et compte-t-elle ? `STRUCTURANT`

**Enjeu.** Le refus de trancher est une information sur les valeurs, pas un trou dans les données.

**Options.**
- **A. Impossible de passer** → force des réponses fausses, casse la validité du profil.
- **B. Passer sans trace** → confort, information perdue.
- **C. Passer avec motif** (« sujet trop personnel », « je ne suis pas informé », « la question est mal posée ») → donnée éditoriale et personnelle de grande valeur.
- **D. Passer casse la continuité** → mécanique de culpabilisation, à exclure.

**Reco.** C — le motif du refus est probablement plus révélateur que la moitié des votes, et il coûte trois lignes de code.

---

## 6. Les mécaniques de rétention qu'on s'interdit

### Q27 — Quelle est la liste écrite des mécaniques interdites, et qui a le pouvoir de la modifier ? `STRUCTURANT`

**Enjeu.** Une éthique non écrite cède toujours au premier mauvais chiffre de rétention.

**Options.**
- **A. Principes implicites** → érosion progressive, invisible, irréversible.
- **B. Liste publique dans l'app** (« ce qu'Endoxa ne fera jamais ») → engagement opposable, différenciation forte, et contrainte réelle.
- **C. Liste interne uniquement** → utile, mais sans coût à la transgression.
- **D. Liste + règle de modification** (délai de réflexion, annonce publique avant tout changement).

**Reco.** B+D — publier la liste est la seule façon de rendre coûteux le fait d'y renoncer un jour de panique.

---

### Q28 — La série quotidienne (« streak ») : interdite, ou reformulée ? `STRUCTURANT`

**Enjeu.** C'est la mécanique la plus efficace du marché et la plus culpabilisante ; il n'y a pas de version tiède.

**Options.**
- **A. Streak classique avec compteur et perte** → +20 à 40 % de rétention, et exactement le procédé que le projet dit refuser.
- **B. Aucune trace de régularité** → cohérent, on renonce à un levier puissant.
- **C. Compteur cumulatif sans rupture** : « 47 questions répondues » qui ne redescend jamais → progression sans punition, différence énorme en vécu, différence faible en code.
- **D. Régularité affichée en douceur** : calendrier de type contribution, sans score ni alerte.

**Reco.** C — un compteur qui ne peut pas être perdu conserve le plaisir de l'accumulation sans jamais fabriquer de dette.

---

### Q29 — Combien de notifications maximum par semaine, et l'utilisateur les règle-t-il dès l'onboarding ? `DÉTAIL`

**Enjeu.** Le produit prévoit déjà trois déclencheurs (question du jour, groupe complet, bilan dominical), ce qui fait 9 notifications par semaine avant même toute optimisation.

**Options.**
- **A. Tout activé par défaut** → désinstallation ou coupure globale des notifications dans les 2 semaines.
- **B. Seule la question du jour est activée par défaut**, le reste en opt-in explicite → volume divisé par trois, et la notification restante conserve sa valeur.
- **C. Plafond dur codé en dur** (max 5/semaine, quoi qu'il arrive).
- **D. Choix complet à l'onboarding** → allonge un onboarding déjà long (voir Q30).

**Reco.** B+C — le plafond en dur est une contrainte qu'on se donne à soi-même, et c'est la seule qui résiste aux futures bonnes idées.

---

### Q30 — L'onboarding par 10 dilemmes : combien de personnes abandonnent avant le dixième ? `DÉTAIL`

**Enjeu.** Dix dilemmes avant la première vraie question, c'est 5 à 8 minutes d'effort avant toute récompense.

**Options.**
- **A. 10 dilemmes obligatoires** → attendre 40 à 60 % d'abandon en cours d'onboarding.
- **B. 3 dilemmes puis entrée directe**, les 7 autres proposés plus tard → activation nettement supérieure, profil initial plus grossier.
- **C. 10 dilemmes mais avec restitution progressive** (une bribe de profil toutes les 3 réponses) → l'effort est payé au fur et à mesure.
- **D. Le premier vote réel sert d'onboarding**, les dilemmes viennent après.

**Reco.** C — le problème n'est pas la longueur mais l'absence de récompense intermédiaire ; trois restitutions changent tout sans réduire la qualité du profil.

---

### Q31 — Le bilan du dimanche : est-ce le bon moment ? `DÉTAIL`

**Enjeu.** Le dimanche soir est le créneau le plus saturé et le plus anxiogène de la semaine.

**Options.**
- **A. Dimanche soir** → concurrence maximale, humeur défavorable.
- **B. Dimanche matin / début d'après-midi** → temps disponible, disposition réflexive.
- **C. Créneau choisi par l'utilisateur.**
- **D. Bilan mensuel plutôt qu'hebdomadaire** → un profil ne bouge pas assez en 7 jours pour justifier un bilan hebdomadaire (voir Q1).

**Reco.** D avec option B — un bilan hebdomadaire qui ne montre aucun changement détruit sa propre crédibilité en trois semaines.

---

## 7. Le pronostic

### Q32 — Faut-il réintroduire le pronostic, sous une forme non ludique ? `STRUCTURANT`

**Enjeu.** Deviner ce que les autres vont répondre est le mécanisme qui produit le plus de surprise, et la surprise est la matière première d'un produit d'introspection.

**Options.**
- **A. Rester sans pronostic** → produit plus pur, mais on se prive du seul dispositif qui crée de l'étonnement à chaque session, indéfiniment.
- **B. Pronostic sur la moyenne nationale** (« selon toi, combien de Français choisiront X ? ») → mesure la justesse de sa représentation du pays, sans enjeu relationnel, et produit un vrai « je ne savais pas ».
- **C. Pronostic sur son groupe** → très puissant, et relationnellement risqué : se tromper sur son conjoint ou son père est une information intime, parfois blessante.
- **D. Pronostic scoré et classé** → transforme l'app en jeu, contredit la promesse.

**Reco.** B en v1, C uniquement en agrégé et jamais nominatif — la calibration sur le pays est un exercice d'humilité gratuit, la calibration sur les proches est une source de conflit qu'il faut manipuler avec précaution.

---

### Q33 — Comment restitue-t-on une erreur de pronostic sans humilier ni gamifier ? `DÉTAIL`

**Enjeu.** La formulation détermine si l'utilisateur apprend quelque chose ou se sent jugé.

**Options.**
- **A. Score de justesse en %** → jeu, classement implicite.
- **B. Formulation en écart** : « tu pensais 30 %, c'était 62 % — tu sous-estimes régulièrement l'attachement à la sécurité » → transforme l'erreur en connaissance de soi.
- **C. Bilan mensuel de calibration** (« sur quels sujets tu te trompes systématiquement ») → matière renouvelable, alignée sur la promesse.
- **D. Rien, on affiche juste le résultat.**

**Reco.** B+C — l'erreur systématique de perception est une information sur soi, ce qui est exactement le produit promis, et elle ne se stabilise jamais.

---

## 8. Le partage vers l'extérieur

### Q34 — Qu'est-ce qui est partageable publiquement sans être indécent ? `STRUCTURANT`

**Enjeu.** Partager « mon profil de valeurs » sur les réseaux revient à publier son orientation politique dérivée, avec des conséquences professionnelles et familiales réelles.

**Options.**
- **A. Partage du profil complet** → exposition politique involontaire, risque pour l'utilisateur, image d'app clivante.
- **B. Partage de la question du jour seule**, sans sa réponse → invitation à la conversation, zéro exposition, faible attrait.
- **C. Partage d'un écart intéressant sans position** : « 68 % des gens pensent le contraire de moi sur cette question » → intrigant, non identifiant politiquement.
- **D. Partage d'un changement d'avis** (« j'ai changé d'avis 4 fois ce mois-ci ») → valorise ce que le produit veut valoriser, socialement flatteur, non clivant.

**Reco.** D en priorité, C en second, jamais A par défaut — le changement d'avis est la seule chose que ce produit peut faire partager qui grandisse celui qui la partage.

---

### Q35 — Quel est le moment précis où quelqu'un a envie d'en parler à un tiers ? `STRUCTURANT`

**Enjeu.** Sans ce moment identifié, le produit n'a aucun canal d'acquisition et devra acheter ses utilisateurs.

**Options.**
- **A. La découverte du profil** → moment fort mais individuel, difficile à raconter sans se dévoiler.
- **B. Le désaccord surprenant avec son groupe** → conversation naturelle en face à face, non réplicable hors groupe (donc absent en v1 solo).
- **C. Une question du jour particulièrement bien posée** → l'objet partagé est le contenu, pas l'app ; le partage est fréquent, la conversion faible.
- **D. La révélation d'un biais de perception** (Q32/Q33) → « je pensais que les gens pensaient X » se raconte facilement et ne dévoile rien.

**Reco.** Instrumenter B et D comme moments de partage explicites, et noter que le meilleur d'entre eux (B) est absent de la v1 solo — ce qui est un argument de plus contre cette v1.

---

## 9. Données sensibles et risque juridique

### Q36 — Les opinions politiques sont des données sensibles au sens de l'article 9 du RGPD : le produit est-il conforme, et à quel coût ? `BLOQUANT`

**Enjeu.** Un profil de valeurs dérivé de positions politiques, visible par des tiers, dans un groupe pouvant contenir des mineurs, est le cas d'usage le plus encadré qui soit.

**Options.**
- **A. On verra plus tard** → risque de devoir tout refaire (consentement, minimisation, visibilité) après le lancement, ou d'être arrêté net sur le segment scolaire.
- **B. Conformité minimale** : consentement explicite et distinct, base légale claire, chiffrement, suppression totale sur demande, aucun transfert hors UE, hébergement français.
- **C. Conformité + privacy by design** : le profil ne quitte jamais l'appareil sauf choix explicite, agrégats groupe calculés sans stockage nominatif → coûteux, et argument de vente central.
- **D. On exclut les mineurs** (18+) → ferme la cible « classe », supprime la couche de risque la plus lourde.

**Reco.** B+D pour la v1, C comme cap — traiter la conformité comme une fonctionnalité de différenciation plutôt qu'une contrainte, parce que sur ce sujet c'est le principal argument de confiance disponible.

---

### Q37 — Que voit exactement un membre du groupe sur les autres, et que reste-t-il quand quelqu'un part ? `STRUCTURANT`

**Enjeu.** « Profils visibles entre membres » est une phrase qui recouvre dix niveaux de divulgation très différents.

**Options.**
- **A. Profil complet et permanent** → maximum d'intérêt, maximum de risque relationnel, et rien n'est réversible.
- **B. Profil visible sur les axes seulement** (4 valeurs d'ordre supérieur, pas les 10) → moins identifiant, moins intrusif, suffisant pour la comparaison.
- **C. Visibilité réciproque et révocable** : je vois ton profil si tu vois le mien, chacun peut se retirer → symétrie, contrôle.
- **D. Départ = effacement complet des traces dans le groupe** → protège le partant, casse l'historique du groupe.

**Reco.** B+C+D — la réciprocité et la révocabilité sont ce qui distingue un miroir partagé d'un fichier d'opinions, et cette distinction doit être visible dès l'écran de création de groupe.

---

## 10. Mesure du succès et seuil d'échec

### Q38 — Si on refuse de mesurer le temps passé et la fréquence, que mesure-t-on ? `BLOQUANT`

**Enjeu.** Sans indicateur de remplacement, l'équipe retombera sur les métriques d'engagement par défaut, et le produit dérivera avec elles.

**Options.**
- **A. Rien, au feeling** → impossible d'arbitrer, impossible de savoir si ça marche.
- **B. Métriques d'engagement standard** (DAU, J30, session) → mesure une promesse qu'on n'a pas faite.
- **C. Métriques de valeur déclarée** : % d'utilisateurs qui, à 30 jours, disent avoir appris quelque chose sur eux ; nombre de changements d'avis assumés ; % qui déclarent avoir eu une conversation réelle grâce à l'app.
- **D. Test de manque** (« si Endoxa disparaissait demain, seriez-vous très déçu ? ») → seuil de référence connu à 40 %, comparable, peu coûteux.

**Reco.** C+D, avec l'engagement suivi mais jamais optimisé — mesurer la conversation provoquée et le manque ressenti est ce qui correspond réellement à la promesse.

---

### Q39 — Comment mesure-t-on qu'une session a été utile plutôt que simplement consommée ? `STRUCTURANT`

**Enjeu.** Une session de 3 minutes machinale et une session de 3 minutes qui fait réfléchir sont identiques dans les logs.

**Options.**
- **A. Temps de réflexion avant réponse** → proxy imparfait mais gratuit : les réponses en moins de 2 secondes signalent du pilotage automatique.
- **B. Question directe occasionnelle** (« cette question t'a-t-elle fait hésiter ? ») une fois par semaine → mesure directe, léger coût d'interruption.
- **C. Taux de changement d'avis en archive** → signal fort de réflexion réelle.
- **D. Verbatims mensuels sur un panel de 20 utilisateurs** → qualitatif, irremplaçable, chronophage.

**Reco.** A+D — le taux de réponses instantanées est l'alerte la moins chère qui existe contre la dérive vers le clic automatique.

---

### Q40 — Quel chiffre, à quelle date, déclenche l'arrêt du projet — écrit et daté avant le lancement ? `BLOQUANT`

**Enjeu.** Sans seuil écrit d'avance, le projet ne s'arrête jamais : il s'éteint lentement en consommant deux ans de la vie du porteur.

**Options.**
- **A. Pas de seuil** → scénario le plus courant et le plus coûteux.
- **B. Seuil de volume** (« moins de 1 000 utilisateurs à 6 mois → stop ») → simple, mais le volume dépend surtout du budget d'acquisition.
- **C. Seuil de qualité** (« moins de 25 % des utilisateurs à J30 déclarent avoir appris quelque chose → stop ») → mesure la promesse, indépendant du budget.
- **D. Seuil de coût personnel** (« si j'y consacre plus de 15 h/semaine sans revenu au bout de 9 mois → stop ») → le seul seuil que le porteur contrôle entièrement.

**Reco.** C+D, écrits, datés, et communiqués à une personne extérieure qui aura mandat de poser la question à l'échéance — un seuil qu'on est seul à connaître ne se déclenche jamais.

---

## 11. Nature du projet, modèle économique, coûts

### Q41 — Projet personnel, associatif ou entreprise : à quelle date la décision est-elle prise, et qu'est-ce qu'elle change dès aujourd'hui ? `BLOQUANT`

**Enjeu.** Ce choix détermine dès maintenant l'architecture, la conformité, la propriété du code, l'ambition d'échelle et la définition même du succès.

**Options.**
- **A. Personnel** → contraintes : ~40 h/mois d'éditorial indéfiniment (Q22), plafond réaliste de quelques milliers d'utilisateurs, aucune obligation de croissance. Décisions : viser l'hebdomadaire, refuser toute cible scolaire, minimiser les coûts fixes.
- **B. Associatif** (loi 1901, angle éducation civique / éducation aux médias) → accès aux subventions, partenariats Éducation nationale, crédibilité de neutralité, lenteur, gouvernance à monter. Décisions : conformité mineurs dès le départ, format classe, licence ouverte.
- **C. Entreprise** → nécessite un modèle économique (Q42) et une taille de marché ; l'introspection quotidienne payante en France est un marché étroit. Décisions : structure juridique, propriété intellectuelle, métriques de croissance dès la v1.
- **D. On reporte la décision** → on prend par défaut les décisions du cas C sans en avoir les moyens, ce qui est le pire des trois.

**Reco.** Trancher avant le développement, et si le doute persiste, choisir A ou B — parce que les décisions de conception les plus lourdes (rythme, cible scolaire, conformité, ouverture) découlent directement de ce choix et sont très coûteuses à défaire.

---

### Q42 — Quels modèles économiques sont compatibles avec la promesse, et lesquels la détruisent ? `BLOQUANT`

**Enjeu.** Le produit accumule des opinions politiques : la plupart des modèles standard sont ici toxiques, pas seulement discutables.

**Options.**
- **A. Publicité** → à exclure catégoriquement : afficher des publicités à côté d'opinions politiques, ou cibler sur profil de valeurs, détruit la confiance en une capture d'écran.
- **B. Revente ou exploitation des données agrégées** (instituts, médias, partis) → revenus réels, et fin immédiate de la promesse dès que ça se sait. À exclure.
- **C. Abonnement individuel** (3-5 €/mois pour l'historique long, les bilans approfondis, l'export) → aligné avec la promesse, marché étroit, conversion attendue 2-4 %.
- **D. B2B éducation / associations** (licence pour une classe, un établissement, une association) → aligné, cycles de vente longs, dépendance à la cible « classe » et donc à la conformité mineurs.
- **E. Dons, mécénat, subventions** → cohérent avec le modèle associatif, revenus faibles et instables.

**Reco.** C en complément d'E ou D selon la nature retenue, et inscrire A et B dans la liste publique des interdits (Q27) — parce que sur un produit d'opinions, l'annonce de ce qu'on ne monétisera jamais vaut davantage que la fonctionnalité qu'on ajoute.

---

### Q43 — Quel est le coût mensuel réel de fonctionnement, éditorial compris, à 100, 1 000 et 10 000 utilisateurs ? `BLOQUANT`

**Enjeu.** Le porteur pense « serveurs » alors que le poste dominant est le temps humain, qui ne baisse jamais et ne se subventionne pas.

**Options.**
- **A. Estimation technique seulement** (hébergement, push, stockage : de l'ordre de 20 à 150 €/mois jusqu'à 10 000 utilisateurs, plus 99 €/an Apple et 25 € Google) → sous-estime le coût réel d'un facteur 10 à 30.
- **B. Coût complet** : technique + éditorial (40 h/mois valorisées) + modération + support + conformité → la vraie question devient « puis-je tenir 40 h/mois pendant 24 mois sans revenu ? ».
- **C. Réduction structurelle du coût** : passage à l'hebdomadaire (Q3), génération assistée avec relecture (Q22) → divise le poste éditorial par 4 à 5.
- **D. Mutualisation** (partenariat avec un média, une association, un chercheur qui fournit les questions).

**Reco.** Chiffrer B honnêtement avant de coder, puis appliquer C — le projet ne mourra pas d'une facture cloud, il mourra d'une lassitude éditorielle un mardi de novembre.

---

### Q44 — À partir de quand l'absence de revenu devient-elle un problème, et que se passe-t-il si ça marche ? `STRUCTURANT`

**Enjeu.** Le succès est aussi dangereux que l'échec pour un projet personnel non structuré : 10 000 utilisateurs, ce sont des demandes RGPD, du support, de la modération et une responsabilité juridique.

**Options.**
- **A. On avisera** → le succès arrive un week-end et n'est pas absorbé.
- **B. Budget-temps et budget-argent plafonnés d'avance** (ex. 150 €/mois et 12 h/semaine, seuil au-delà duquel on cherche une structure).
- **C. Plan de bascule prêt** : au-delà de X utilisateurs, on crée l'association ou la société, on ouvre un canal de dons, on recrute un relecteur.
- **D. Bridage volontaire** (invitations limitées, liste d'attente) → contrôle la croissance, préserve la qualité, frustre.

**Reco.** B+C, et D si le succès dépasse la capacité — un plan de bascule écrit sur une page évite de subir une croissance qu'on aura pourtant souhaitée.

---

## 12. Voisins et concurrents

### Q45 — Que retient-on de la trajectoire des applications civiques françaises, notamment Elyze et Voxe ? `STRUCTURANT`

**Enjeu.** Elyze a fait plus d'un million de téléchargements en quelques semaines début 2022 puis a disparu de l'usage après l'élection : le pic médiatique n'est pas un produit.

**Options.**
- **A. On vise le même type de pic** (élection 2027) → acquisition massive et gratuite, rétention quasi nulle après l'événement, et fin du projet en juin.
- **B. On construit d'abord la rétention hors période électorale**, et on utilise l'événement comme accélérateur sur une base déjà solide.
- **C. On assume la saisonnalité** (Q3, option B) et on construit un produit fait pour les pics et les creux.
- **D. On ignore la question** → on subira la saisonnalité sans l'avoir anticipée.

**Reco.** B, avec la contrainte que la base doit exister avant l'événement — sinon 2027 apportera un demi-million d'installations et zéro utilisateur en juillet.

---

### Q46 — Que retient-on du fait que les produits d'introspection à succès monétisent le test unique, pas l'habitude quotidienne ? `BLOQUANT`

**Enjeu.** 16Personalities, les tests de valeurs, les rapports de personnalité : le modèle dominant du secteur est un one-shot très rentable, pas un rituel quotidien — et c'est peut-être la forme naturelle de cette promesse.

**Options.**
- **A. On ignore et on maintient le quotidien** → on parie contre l'ensemble du secteur, ce qui est possible mais doit être conscient.
- **B. On inverse** : le cœur du produit devient un parcours de 40 questions étalé sur deux semaines, avec un rapport approfondi à la fin ; le quotidien devient une option pour ceux qui veulent continuer → conversion et satisfaction très supérieures, rétention faible et assumée.
- **C. Hybride** : one-shot introspectif comme porte d'entrée, groupe comme raison de rester → la partie payante est individuelle, la partie qui retient est collective.
- **D. On teste les deux formats en parallèle** avant de choisir (voir « Le pari central »).

**Reco.** C — c'est la seule configuration où la promesse individuelle (qui vend) et le moteur social (qui retient) ne se concurrencent pas, et elle mérite d'être comparée à l'option quotidienne avant tout développement.

---

### Q47 — Quels voisins directs faut-il étudier sérieusement avant de décider, et sur quoi précisément ? `DÉTAIL`

**Enjeu.** Plusieurs des problèmes listés ici ont déjà été résolus, ou échoués, par d'autres — les regarder coûte trois jours.

**Options.**
- **A. On ne regarde pas** → on réinvente des erreurs documentées.
- **B. Revue ciblée sur 4 axes** : le rituel quotidien en petit cercle (BeReal — le décrochage après la nouveauté), l'identité partagée entre amis (Co-Star — la comparaison comme moteur social durable), la délibération collective (Pol.is, Kialo, Tournesol — pourquoi ça reste confidentiel), le jugement majoritaire en France (Mieux Voter — pédagogie et adoption réelle).
- **C. Revue exhaustive** → trois semaines, rendements décroissants.
- **D. Entretiens avec 2-3 porteurs de projets voisins français** → le plus rentable en information par heure investie.

**Reco.** B en 3 jours puis D — deux conversations avec des gens qui ont déjà tenté ça en France valent plus que toute la veille documentaire.

---

# Le pari central

**L'hypothèse sur laquelle tout repose, en cinq lignes :**

1. Des adultes français accepteront de consacrer trois minutes par jour, pendant des mois, à juger l'actualité — non pour débattre, mais pour se voir eux-mêmes.
2. Le portrait de valeurs qu'on leur renvoie sera suffisamment juste et surprenant pour qu'ils s'y reconnaissent tout en y apprenant quelque chose.
3. Ils accepteront d'exposer ce portrait à leur famille, leur classe ou leur association — et cette exposition sera vécue comme un rapprochement, pas comme une mise à nu.
4. Le groupe fournira, une fois le portrait individuel stabilisé, une raison de revenir qui ne s'épuise pas.
5. Et tout cela sans aucune des mécaniques de contrainte qui font tenir les autres applications quotidiennes.

**Le maillon le plus fragile n'est pas le premier, c'est le quatrième combiné au cinquième :** ce produit parie qu'un groupe de cinq proches restera actif pendant des mois sans streak, sans classement et sans relance. Rien dans ce qu'on connaît des petits groupes ne le rend probable par défaut.

## Trois tests, moins de 1 000 € et moins d'un mois, sans écrire d'application

### Test 1 — Le magicien d'Oz sur WhatsApp (le test principal)
**Coût : ~100 €** (formulaire Tally/Typeform, quelques cartes cadeaux de remerciement) — **Durée : 4 semaines**

Recruter 6 groupes réels et déjà constitués (2 familles, 2 groupes d'amis, 1 association, 1 classe si accessible), 5 à 8 personnes chacun. Chaque jour, envoyer manuellement dans leur boucle WhatsApp la question du jour et un lien de vote. Chaque dimanche, envoyer à chaque personne son profil de valeurs, calculé à la main ou sur tableur, et au groupe sa comparaison à la moyenne des autres groupes.

**Ce que ça mesure :** le taux de participation à J7, J14, J28 par groupe et par personne ; la distribution actif/passif réelle ; le nombre de groupes encore vivants à 4 semaines ; et surtout, en entretien final, si les gens en ont parlé entre eux hors de l'outil.
**Ce qui invalide le pari :** moins de 2 groupes sur 6 encore actifs à J28 avec relance humaine. Si ça ne tient pas avec un humain qui anime et des volontaires motivés, aucune automatisation ne le sauvera.

### Test 2 — La validité du miroir
**Coût : ~350 €** (panel Prolific ou équivalent, 120 répondants) — **Durée : 2 semaines**

Faire passer à 120 personnes le questionnaire PVQ-21 de Schwartz, puis 30 questions au format Endoxa. Corréler les deux. En parallèle, renvoyer à 60 d'entre elles deux portraits — le leur et celui d'un autre répondant — sans dire lequel est lequel, et leur demander d'identifier le leur et de noter la justesse de chacun.

**Ce que ça mesure :** la validité du profil (corrélation par axe) et sa reconnaissabilité (test de Barnum).
**Ce qui invalide le pari :** corrélation inférieure à 0,4 sur les axes principaux, ou moins de 65 % de bonne identification de son propre portrait. Dans ce cas, la promesse « se connaître mieux » n'est pas tenue par le mécanisme, et il faut soit changer le mécanisme, soit changer la promesse.

### Test 3 — La fausse porte : quelle promesse fait cliquer, et pour combien
**Coût : ~450 €** (600 € max) de publicité Meta/Instagram ciblée France — **Durée : 2 semaines**

Trois pages d'atterrissage identiques sauf la promesse : (a) « Découvre tes vraies valeurs, une question par jour », (b) « Ce que ta famille pense vraiment — la question du jour, entre vous », (c) « 40 questions, un portrait de tes valeurs » (format one-shot, voir Q46). Chaque page se termine par une inscription à la liste d'attente, puis une seconde demande : « invite 3 proches maintenant ».

**Ce que ça mesure :** le coût par inscrit selon la promesse, et surtout le taux d'invitation spontanée — le seul indicateur pré-produit du démarrage à froid des groupes.
**Ce qui invalide le pari :** si (b) et (c) surpassent nettement (a), la promesse d'introspection quotidienne solo n'est pas celle qui recrute, et la v1 solo est à revoir. Si moins de 15 % des inscrits invitent quelqu'un, le modèle de croissance par le cercle proche n'existe pas et il faut un plan d'acquisition payant — ce qui ramène directement à la question de la nature du projet (Q41).
