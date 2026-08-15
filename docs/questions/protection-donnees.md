# Questions — Protection des données et sécurité

> Produit par le rôle correspondant de l'équipe (voir `docs/methode.md`).
> Questions, pas solutions. Niveaux : **BLOQUANT**, **STRUCTURANT**, **DÉTAIL**.

---

*45 questions. Endoxa traite des opinions politiques : ce n'est pas un chapitre
de conformité en fin de projet, c'est la contrainte qui décide de l'architecture.
Deux constats préalables commandent une grande partie de ce qui suit — ils sont
démontrés en section 5 et 6, et ils sont, en l'état du cadrage, des défauts de
conception, pas des risques diffus.*

*Classement : **BLOQUANT** (rien de solide ne peut être écrit avant), **STRUCTURANT**
(décidable en semaine 1-6, coûteux à changer ensuite), **DÉTAIL** (arbitrable au fil
de l'eau).*

---

## 0. Ce qui est certain, ce qui est interprétation

Distinction tenue dans tout le document. Les URL sont en fin de dossier.

**Obligations certaines (texte clair, applicable sans discussion)**

| Point | Source |
|---|---|
| Les opinions politiques sont interdites de traitement sauf exception de l'art. 9.2 | RGPD art. 9.1 |
| Les données qui **révèlent indirectement** une donnée sensible sont couvertes par l'art. 9 | CJUE C-184/20, 1er août 2022 |
| Conserver en mémoire informatisée des données faisant apparaître **directement ou indirectement** les opinions politiques sans **consentement exprès** est un délit : 5 ans et 300 000 € | Code pénal art. 226-19 |
| Un mineur de moins de 15 ans ne peut pas consentir seul à un service en ligne | LIL art. 45 |
| Discriminer sur les opinions politiques est un délit ; l'interdiction est aussi au Code du travail | C. pén. art. 225-1/225-2, C. trav. art. L.1132-1 |
| AIPD obligatoire pour un traitement à grande échelle de données de l'art. 9 | RGPD art. 35.3.b |
| Violation de données : notification CNIL sous 72 h, information des personnes si risque élevé | RGPD art. 33 et 34 |
| Les données pseudonymisées restent des données personnelles | RGPD cons. 26 ; doctrine CNIL |
| Le ciblage publicitaire politique sur données sensibles est interdit **même avec consentement** | Règl. (UE) 2024/900, applicable depuis le 10 oct. 2025 |

**Points d'interprétation (à trancher et à documenter, pas à supposer)**

- Le profil de valeurs de Schwartz relève-t-il de l'art. 9 ? (ma position : oui, par
  deux voies indépendantes — voir Q3.)
- Endoxa est-il « à grande échelle » au sens de l'art. 35.3.b et de l'art. 37.1.c ?
  Le seuil n'est pas chiffré dans le texte.
- Endoxa est-il un « hébergeur » au sens de l'art. 6 LCEN, donc soumis à la
  conservation d'un an des données d'identification ? (voir Q41.)
- Endoxa est-il un « réseau social en ligne » au sens de la loi française sur la
  majorité numérique ? (voir Q13.)
- L'exception de recherche scientifique (art. 9.2.j) est-elle mobilisable ? (voir Q8.)

---

## 1. Qualification des données

#### Q1 — [BLOQUANT] Reconnaît-on formellement, par écrit et dès maintenant, que la mention portée sur une proposition d'actualité politique est une donnée de l'article 9 ?
**Enjeu.** Juridiquement, c'est le point d'entrée de tout le régime — base légale, AIPD, DPO, sécurité renforcée, sanctions pénales ; humainement, c'est la reconnaissance que la base d'Endoxa est un fichier d'opinions politiques nominatif au pseudonyme près, c'est-à-dire l'objet le plus convoité qui soit dans ce domaine.
- **Oui, sans réserve** — impose le consentement explicite, l'AIPD, l'hébergement maîtrisé et la minimisation. C'est le régime le plus lourd, mais c'est le seul défendable.
- **Non, au motif que les questions sont « d'actualité » et pas « partisanes »** — indéfendable : une mention sur « faut-il taxer les hauts patrimoines » est une opinion politique, quel que soit l'habillage. Ce raisonnement est exactement celui que la CJUE a écarté en 2022.
- **Oui pour les mentions, non pour le reste** — position intenable en pratique : le reste est calculé à partir des mentions.
- **On tranchera plus tard** — chaque jour de collecte sous un régime non tranché est un jour de collecte potentiellement illicite, et l'art. 226-19 est un délit continu.

> **Reco.** Oui, sans réserve, et l'inscrire en tête du registre des traitements : tout le reste du dossier en découle mécaniquement.

#### Q2 — [BLOQUANT] La justification choisie et la source déclarée de conviction (« un principe moral », « ma confiance en quelqu'un ») sont-elles qualifiées séparément ?
**Enjeu.** « Un principe moral » est une donnée qui peut faire apparaître une conviction philosophique ou religieuse, catégorie distincte de l'opinion politique dans l'art. 9 ; l'enjeu réel pour la personne est qu'un profil épistémique croisé avec un sujet (« ses positions sur la laïcité viennent d'un principe moral ») est plus intrusif qu'un vote.
- **Qualification unique « opinions politiques »** — simple, mais l'information donnée à l'utilisateur devient inexacte, ce qui fragilise le consentement lui-même.
- **Double qualification, opinions politiques + convictions philosophiques** — plus honnête, aucun coût technique supplémentaire, et solidifie le consentement.
- **On retire la source « principe moral » du choix** — appauvrit gravement le profil épistémique, qui est la partie la plus originale du produit.
- **On ne conserve la source épistémique que sous forme agrégée, jamais liée à une question** — perd le croisement « sur l'écologie tu t'appuies sur des données, sur la sécurité sur ton vécu », qui est précisément la promesse.

> **Reco.** Double qualification et conservation du croisement, mais le croisement question × source ne doit jamais être visible par un tiers, même dans un groupe.

#### Q3 — [BLOQUANT] Le radar de valeurs de Schwartz est-il une donnée de l'article 9 ?
**Enjeu.** C'est la question la plus consultée du dossier, parce que c'est le seul objet qu'Endoxa rend **visible à des tiers** ; si le radar relève de l'art. 9, alors sa visibilité dans un groupe est une communication de données sensibles à des tiers, qui exige son propre consentement explicite, distinct de celui du traitement.
- **Oui, comme donnée dérivée révélant des opinions politiques** — position appuyée par C-184/20 (une donnée révélant indirectement une donnée sensible est sensible) et par l'art. 226-19 du Code pénal, qui vise expressément le « directement ou indirectement ». Le radar est calculé **exclusivement** à partir de jugements politiques : il n'existe pas de lecture où il n'en révèle rien.
- **Oui, comme conviction philosophique** — l'universalisme, la tradition, la conformité et la bienveillance sont des positions morales au sens ordinaire du terme. Voie indépendante de la précédente, et elle tient même si l'on conteste la première.
- **Non, c'est un trait de personnalité psychométrique, pas une opinion** — c'est ce que dit la littérature sur Schwartz, qui mesure des valeurs personnelles et non des positions politiques (le cadrage le reconnaît lui-même en §5). Mais la mesure ici n'est pas faite avec l'instrument de Schwartz : elle est faite avec des jugements sur des propositions politiques. L'argument tombe sur la méthode de collecte, pas sur la théorie.
- **Non tranché, on affiche quand même** — expose à la qualification pénale, sans défense possible.

> **Reco.** Oui, par les deux voies : c'est la seule position qui survive à un contrôle, et elle a l'avantage de forcer un consentement séparé pour la visibilité en groupe, qui est de toute façon nécessaire.

#### Q4 — [STRUCTURANT] Le fait de **ne pas** voter, ou de cocher « sans avis », est-il traité comme une donnée sensible ?
**Enjeu.** L'abstention systématique sur les questions touchant un sujet précis est un signal politique fort, et c'est un signal que l'utilisateur ne croit pas donner.
- **Oui, même régime que les mentions** — cohérent, coût nul.
- **Non, on ne stocke pas les non-réponses** — impossible : le produit doit savoir quelles questions ont été vues pour ne pas les reproposer, donc l'information existe de fait.
- **On stocke « vue / non répondue » sans horodatage fin** — réduit un peu le signal, ne le supprime pas.

> **Reco.** Même régime, et surtout : ne jamais exposer à un tiers, même agrégé, la carte des sujets qu'un membre esquive.

#### Q5 — [STRUCTURANT] La tranche d'âge et la région sont-elles collectées parce qu'un usage produit précis les exige, ou « au cas où » ?
**Enjeu.** Ce sont les deux quasi-identifiants qui font passer le pseudonymat de « raisonnable » à « fragile » (démonstration en Q26) ; le RGPD impose la minimisation, et la question posée à un contrôleur sera exactement celle-ci.
- **Elles servent aux comparaisons nationales par âge et région** — usage réel, mais le cadrage le place en « plus tard », donc la collecte en v1 est prématurée et non minimisée.
- **Elles servent à centrer le profil par cohorte** — argument psychométrique recevable, à faire trancher par le rôle Psychométrie : si le centrage est fait par utilisateur (cadrage §5), la cohorte n'est pas nécessaire.
- **Elles ne servent à rien en v1** — alors on ne les collecte pas, et on gagne 6 bits d'entropie de réidentification.
- **On les collecte en optionnel** — le taux de remplissage sera élevé et le caractère « optionnel » ne protège personne une fois la donnée en base.

> **Reco.** Ne rien collecter en v1 au-delà du pseudo ; réintroduire l'âge et la région seulement quand une fonctionnalité les exige, avec un découpage grossier décidé à ce moment-là.

#### Q6 — [DÉTAIL] Le pseudonyme est-il libre, ou contraint pour empêcher qu'il porte le nom réel ?
**Enjeu.** Un utilisateur sur trois saisira son prénom et son nom ; le pseudonymat annoncé devient alors nominatif, et c'est vous qui aurez collecté l'identité.
- **Libre** — le plus simple, et le pseudonymat annoncé est en partie fictif.
- **Généré aléatoirement, non modifiable** — protection maximale, coût social élevé dans un groupe familial où l'on veut être reconnu.
- **Généré par défaut, modifiable, avec un avertissement explicite** — bon compromis.
- **Libre mais filtré (rejet des formes « Prénom Nom »)** — filtrage imparfait, effet de bord agaçant.

> **Reco.** Généré par défaut, modifiable après un écran qui dit clairement « ce pseudo sera vu par les membres de vos groupes ».

---

## 2. Base légale et consentement

#### Q7 — [BLOQUANT] Le consentement explicite de l'art. 9.2.a est-il retenu comme unique base, et sur quelles finalités exactement ?
**Enjeu.** Le consentement est presque la seule porte ouverte ici, mais un consentement global « j'accepte le traitement de mes données » ne vaut rien pour l'art. 9 : il doit être spécifique par finalité, et une finalité oubliée aujourd'hui ne pourra être ajoutée qu'en re-sollicitant tout le monde.
- **Un consentement unique, global** — nul en droit ; expose à l'art. 226-19 puisque le « consentement exprès » ferait défaut.
- **Un consentement explicite pour le traitement des votes et du profil personnel, un second pour la visibilité en groupe, un troisième pour la contribution aux statistiques nationales** — c'est le découpage minimal défendable. Trois cases, trois retraits possibles indépendamment.
- **Le découpage précédent, plus un quatrième pour le rejeu d'archives et un cinquième pour les notifications** — plus fin, mais on approche le seuil où l'utilisateur clique sans lire, ce qui annule la valeur du consentement.
- **Intérêt légitime pour les statistiques nationales** — impossible : l'art. 9 ne connaît pas l'intérêt légitime comme exception.

> **Reco.** Exactement trois consentements séparés (traitement personnel / visibilité en groupe / contribution aux agrégats publics), le premier étant seul obligatoire pour utiliser le service.

#### Q8 — [STRUCTURANT] L'exception de recherche scientifique (art. 9.2.j) est-elle explorée comme base complémentaire ?
**Enjeu.** Elle allégerait la dépendance au consentement pour la partie statistique, mais elle exige une base dans le droit de l'Union ou national, proportionnée, avec les garanties de l'art. 89 — ce que n'apporte pas le simple fait de se déclarer « projet de recherche ».
- **On l'écarte** — plus sûr, et cohérent avec un projet grand public.
- **On la mobilise pour les agrégats nationaux uniquement** — supposerait un partenariat académique réel, un protocole, un comité, et probablement une saisine de la CNIL. Plusieurs mois.
- **On l'invoque sans partenariat** — le pire des cas : on croit avoir une base légale, on n'en a pas.

> **Reco.** L'écarter en v1 et la rouvrir seulement si un laboratoire s'associe formellement au projet ; le gain ne vaut pas le risque avant.

#### Q9 — [BLOQUANT] À quel moment exact du parcours le consentement est-il recueilli, sachant que l'onboarding commence par dix dilemmes ?
**Enjeu.** Les dix dilemmes d'amorçage produisent des données de l'art. 9 dès le premier écran : si le consentement arrive après, la collecte initiale est illicite et le premier geste du produit est une infraction.
- **Consentement avant le premier dilemme** — juridiquement propre, mais on demande un engagement lourd à quelqu'un qui n'a encore rien vu du produit : le taux d'abandon sera élevé.
- **Les dilemmes en local, dans le navigateur, sans transmission, puis consentement avant l'envoi** — le produit se montre avant de demander, et rien de sensible ne quitte l'appareil avant le consentement. Coût technique modéré (stockage local puis synchronisation).
- **Consentement après les dilemmes, avec suppression si refus** — la collecte a déjà eu lieu ; la suppression a posteriori ne régularise pas.
- **Deux dilemmes de démonstration non enregistrés, puis consentement** — compromis intermédiaire, moins bon que le calcul local.

> **Reco.** Dilemmes exécutés et scorés localement, consentement demandé au moment où l'utilisateur voit son premier radar : c'est le seul instant où il comprend ce qu'il donne et ce qu'il reçoit.

#### Q10 — [BLOQUANT] Que se passe-t-il, concrètement et dans la base, quand un utilisateur retire son consentement ?
**Enjeu.** Le retrait doit être aussi simple que le don ; l'enjeu réel est qu'un retrait qui laisse les données en place est une promesse rompue au moment précis où l'utilisateur exprime sa défiance.
- **Retrait = suppression totale du compte** — clair, mais brutal : l'utilisateur qui veut seulement quitter la visibilité de groupe perd tout.
- **Retrait par finalité, avec effet immédiat et distinct** — retrait de la visibilité en groupe : le radar disparaît des écrans des autres membres sous 24 h. Retrait de la contribution aux agrégats : les votes sortent des calculs futurs. Retrait du traitement personnel : suppression.
- **Retrait = gel, données conservées « au cas où l'utilisateur revienne »** — non conforme, et c'est exactement la pratique que la CNIL sanctionne.

> **Reco.** Retrait par finalité, avec un délai maximal affiché à l'utilisateur (« sous 24 heures pour les écrans, sous 35 jours pour les sauvegardes ») et tenu.

#### Q11 — [STRUCTURANT] L'authentification sans mot de passe repose-t-elle sur une adresse e-mail, et cette adresse est-elle alors la vraie identité de l'utilisateur ?
**Enjeu.** Un lien magique par e-mail transforme le pseudonymat en identité faible : l'adresse contient souvent le nom réel, elle relie le compte à un fournisseur tiers, et elle devient le premier objet de réquisition ou de vol.
- **Lien magique par e-mail** — simple, éprouvé, mais introduit un identifiant fortement nominatif dans une base d'opinions politiques, et fait transiter le lien par un prestataire d'envoi.
- **Clé d'accès (passkey / WebAuthn) sans e-mail** — aucune donnée nominative en base, mais perte de l'appareil = perte du compte, et taux d'échec réel non négligeable sur un public grand public.
- **Passkey en principal, e-mail en secours optionnel et chiffré** — meilleur des deux, coût de développement supérieur.
- **Mot de passe classique** — la CNIL a des exigences précises (délib. 2022-100) ; ce n'est pas plus sûr ici, et c'est plus de friction.

> **Reco.** Passkey en principal, adresse de secours facultative stockée chiffrée avec une clé hors base : c'est le seul montage où une saisie de la base ne livre pas d'identités.

---

## 3. Mineurs et usage scolaire

#### Q12 — [BLOQUANT] Quel est l'âge minimal pour ouvrir un compte Endoxa, et comment est-il vérifié ?
**Enjeu.** En dessous de 15 ans, le consentement doit être conjoint mineur + parent (LIL art. 45), et il s'agit ici d'un consentement explicite à un traitement d'opinions politiques d'enfant, ce qui est l'un des traitements les plus délicats qui soient ; le risque réel est qu'un dossier d'opinions politiques constitué à 13 ans suive la personne.
- **15 ans minimum, déclaratif** — le plus simple ; la CNIL admet un effort raisonnable, mais un simple champ « j'ai plus de 15 ans » est faible pour de l'art. 9.
- **15 ans minimum, avec estimation d'âge indépendante** — coûteux, intrusif, et contradictoire avec la minimisation.
- **18 ans minimum en v1** — ferme la porte à l'usage scolaire, qui est l'un des débouchés les plus naturels du produit, mais retire d'un coup toute la couche « mineurs » du dossier de conformité et de l'AIPD.
- **Moins de 15 ans avec consentement parental vérifié** — impose un parcours parent (double opt-in, vérification du lien de parentalité), soit plusieurs semaines de développement pour un usage qui n'existe pas encore.

> **Reco.** 18 ans en v1, déclaratif, avec la mention explicite que l'usage scolaire fera l'objet d'une version distincte : c'est la seule façon de ne pas payer maintenant le coût d'un usage prévu « plus tard ».

#### Q13 — [STRUCTURANT] Endoxa est-il un « réseau social en ligne » au sens de la loi française sur la majorité numérique, et si oui à partir de quelle version ?
**Enjeu.** La presse rapporte l'adoption définitive le 21 juillet 2026 d'une loi imposant vérification d'âge et autorisation parentale pour les moins de 15 ans, avec une première échéance au 1er septembre 2026 — soit dans deux semaines ; si Endoxa entre dans le champ, l'obligation n'est plus une option de conception. **Point à vérifier au texte publié au Journal officiel avant toute décision : je n'ai pu consulter que des sources de presse.**
- **La v1 solo n'est pas un réseau social** — position solide : aucun lien entre utilisateurs, aucun contenu publié.
- **La v2 avec groupes privés l'est probablement** — mise en relation d'utilisateurs, partage de contenus entre eux. La qualification est plausible.
- **On tranche après lecture du texte** — la bonne méthode, à condition de la faire avant d'écrire la v2.

> **Reco.** Faire lire le texte publié par un juriste avant la conception de la v2, et considérer par défaut que la v2 est dans le champ.

#### Q14 — [BLOQUANT] Dans un groupe de classe, qu'est-ce que l'enseignant voit exactement ?
**Enjeu.** Un enseignant qui voit le radar de valeurs de ses élèves détient une information sur leurs convictions politiques et morales, dans une relation d'autorité et de notation ; le risque juridique est réel, mais le risque humain est supérieur — un élève qui se sait lu ne répond plus honnêtement, et le produit ne mesure alors plus rien.
- **L'enseignant voit tout, comme un membre ordinaire** — inacceptable : c'est la constitution d'un fichier d'opinions d'élèves consultable par un agent public, et le consentement d'un élève à son professeur n'est pas libre.
- **L'enseignant ne voit que les agrégats de la classe, jamais un radar individuel** — position défendable, et pédagogiquement suffisante : « votre classe est très divisée sur ce sujet » est ce qui intéresse un cours.
- **L'enseignant ne voit rien du tout ; il ouvre le groupe et en sort** — le plus protecteur, difficile à tenir en pratique (il faut bien projeter quelque chose).
- **Rôle « animateur » sans appartenance, avec accès aux seuls agrégats au-dessus d'un seuil** — variante réaliste de la deuxième option.

> **Reco.** Rôle animateur distinct, agrégats seulement, seuil minimal de répondants, et aucune possibilité technique d'accéder à un radar individuel — pas même en base pour un administrateur.

#### Q15 — [STRUCTURANT] Qui est responsable de traitement dans un usage scolaire : Endoxa, l'établissement, ou les deux ?
**Enjeu.** Si l'établissement est responsable, la base légale peut basculer vers la mission d'intérêt public et le consentement parental n'est plus le pivot ; si Endoxa reste seul responsable, c'est à lui de recueillir le consentement de chaque famille, ce qui est industriellement infaisable.
- **Endoxa seul responsable** — infaisable à l'échelle d'une classe.
- **Responsabilité conjointe avec l'établissement, contractualisée** — le montage juste, mais il suppose un accord écrit par établissement, et l'Éducation nationale a ses propres circuits de validation.
- **Endoxa sous-traitant de l'établissement** — cohérent, mais Endoxa détermine seul les finalités et les moyens : la qualification serait fictive.
- **Pas d'usage scolaire** — la réponse par défaut si aucune des précédentes n'est instruite.

> **Reco.** Ne pas ouvrir l'usage scolaire avant d'avoir un modèle de convention de responsabilité conjointe validé ; d'ici là, le refuser explicitement dans les CGU.

#### Q16 — [DÉTAIL] Un compte ouvert à 17 ans dans un cadre scolaire devient quoi à 18 ans ?
**Enjeu.** L'historique constitué sous consentement parental n'est pas automatiquement couvert par le consentement de l'adulte qu'il devient.
- **Bascule automatique, rien ne change** — le consentement initial n'était pas le sien.
- **Re-sollicitation du consentement à la majorité, avec suppression à défaut de réponse sous 90 jours** — propre, et cohérent avec la doctrine CNIL sur les droits numériques des mineurs.
- **Purge de l'historique antérieur à la majorité, redémarrage du profil** — le plus protecteur, le plus coûteux pour l'utilisateur.

> **Reco.** Re-sollicitation à la majorité avec suppression par défaut : c'est aussi un excellent argument de confiance à afficher.

---

## 4. Le groupe d'entreprise et la subordination

#### Q17 — [BLOQUANT] Autorise-t-on des groupes en contexte professionnel, où un supérieur hiérarchique verrait le radar de valeurs de ses subordonnés ?
**Enjeu.** Le consentement d'un salarié à son employeur est réputé rarement libre en raison du lien de subordination (position constante CNIL et CEPD) ; et le risque réel dépasse le droit des données : discriminer sur les opinions politiques est un délit pénal, et un radar de valeurs consulté avant une promotion est un moyen de le commettre sans laisser de trace.
- **Autorisé sans restriction** — le consentement des salariés sera invalide, et Endoxa fournit l'outil d'une discrimination. C'est le scénario où un incident détruit le projet, indépendamment de toute sanction.
- **Interdit par conception : aucun groupe ne peut afficher de radar individuel** — supprime le problème à la racine, mais retire le cœur de la promesse pour tous les groupes, y compris familiaux.
- **Autorisé, mais les groupes déclarés « professionnel » n'affichent que des agrégats** — repose sur une déclaration, donc contournable en une seconde par un employeur qui coche « amis ».
- **Interdit par les CGU, sans mesure technique** — sans effet réel ; les CGU ne sont pas un contrôle d'accès.

> **Reco.** Interdire par conception que quiconque puisse **créer un groupe et en voir les radars sans y participer à l'identique** — un créateur de groupe est un membre comme les autres, son propre radar est visible de tous, et il n'a aucun pouvoir de lecture supplémentaire. La symétrie est la seule protection qui résiste à un mensonge sur le type de groupe.

#### Q18 — [STRUCTURANT] Un utilisateur peut-il refuser d'exposer son radar dans **un** groupe sans quitter le groupe ?
**Enjeu.** Sans cette granularité, l'appartenance multiple (famille + travail, prévue au cadrage) oblige à un choix binaire ; et le refus visible dans un groupe professionnel est lui-même un signal — « pourquoi masques-tu ton profil ? ».
- **Non, l'appartenance vaut exposition** — simple, et transforme la moindre invitation professionnelle en pression.
- **Oui, radar masquable par groupe** — nécessaire, mais crée le signal du masquage.
- **Oui, et le masquage est indiscernable : les radars ne sont visibles qu'au-dessus d'un seuil de membres exposés, et l'application n'indique jamais qui masque** — supprime le signal, coût de conception faible si décidé maintenant.

> **Reco.** Masquage par groupe, indiscernable, activé par défaut à l'entrée dans un nouveau groupe.

#### Q19 — [DÉTAIL] Que voit l'administrateur technique d'Endoxa, et sous quel contrôle ?
**Enjeu.** Le risque le plus banal est l'accès de routine : une personne seule avec un accès base de production peut lire les opinions politiques de tous les utilisateurs, y compris de gens qu'elle connaît.
- **Accès direct à la base en production** — normal en projet solo, et c'est le point faible le plus probable du dispositif.
- **Accès via des vues qui masquent les colonnes sensibles, accès brut sous procédure journalisée** — praticable même seul.
- **Chiffrement applicatif des colonnes sensibles, clé hors base** — l'administrateur voit du chiffré ; le déchiffrement est possible mais tracé.

> **Reco.** Chiffrement applicatif dès la v1 des mentions, justifications et radars, avec une clé qui n'est pas dans la base : c'est peu de travail au départ et impossible à rétrofitter proprement.

---

## 5. L'anonymat des votes face à la visibilité des profils

*Cette section contient le défaut de conception le plus grave du cadrage actuel.*

#### Q20 — [BLOQUANT] A-t-on vu que le radar visible permet de **reconstituer** le vote du jour, et qu'en l'état la promesse « votes anonymes » est fausse ?
**Enjeu.** Ce n'est pas un risque probabiliste, c'est une inversion déterministe ; et elle est réalisable par un membre du groupe sans aucune compétence technique particulière, ce qui en fait le scénario le plus probable de tous.

**Démonstration.** Le radar publié a 10 dimensions. Un vote quotidien comporte 5 inconnues : 4 mentions (chacune dans {−2, −1, 0, +1, +2} ou « sans avis », soit 6 valeurs) et 1 justification choisie parmi 4. Le cadrage §9 prévoit que les vecteurs de valeurs des propositions et des justifications soient **publics**. La différence entre le radar de la veille et celui du jour est donc un système de 10 équations à 5 inconnues discrètes, sur un espace de 6⁴ × 4 = 5 184 combinaisons — énumérable exhaustivement en quelques millisecondes. Le centrage par utilisateur et le compteur `nb_votes` (cadrage §5 et §10) n'ajoutent que des constantes connues ou déductibles. **L'inversion est exacte.** Il suffit à un membre du groupe de capturer le radar d'un autre chaque soir.

- **On accepte et on cesse de promettre l'anonymat** — honnête, mais détruit la promesse centrale du produit (« je ne sais pas comment mon oncle a voté »).
- **Le radar visible par les tiers n'est mis à jour que par lots, à date fixe (le dimanche), en intégrant sept jours d'un coup** — l'inversion porte alors sur 7 votes simultanés, soit 5 184⁷ combinaisons : elle devient impraticable. Coût : le radar des autres n'est plus « vivant » au quotidien, ce qui est acceptable puisque le rendez-vous hebdomadaire existe déjà (cadrage §14).
- **On ne publie pas les vecteurs de valeurs** — contredit frontalement l'engagement de transparence du cadrage §9, et ne protège que jusqu'à ce que quelqu'un les reconstitue par observation de son propre radar.
- **On quantifie grossièrement le radar affiché (5 niveaux par axe) et on ajoute du bruit** — réduit sans supprimer : l'accumulation d'observations reconstitue le signal, et le bruit dégrade le produit pour tout le monde.

> **Reco.** Radar tiers mis à jour uniquement au bilan du dimanche, par lots de sept jours minimum : c'est la seule contre-mesure qui rétablisse réellement l'anonymat sans rien retirer à la transparence promise.

#### Q21 — [BLOQUANT] Quel est le nombre minimal de membres au-dessus duquel un groupe affiche un résultat par question ?
**Enjeu.** Chaque membre connaît son propre vote et peut donc le soustraire de l'agrégat affiché ; à 2 membres la divulgation est totale, à 3 elle est fréquente, et une coalition de n−1 membres déanonymise le dernier **quel que soit n**.
- **Aucun seuil** — la promesse d'anonymat est fausse dès le premier groupe familial de trois personnes.
- **Seuil de 3** — insuffisant : un membre voit la distribution des 2 autres ; s'ils ont voté pareil, il connaît les deux.
- **Seuil de 5 votants sur la question concernée** — plancher défendable, aligné sur les pratiques de secret statistique. La divulgation résiduelle exige une coalition de 4 personnes sur 5.
- **Seuil de 5 votants ET 8 membres, avec suppression de l'affichage si un seul votant sépare deux publications successives** — protège aussi contre l'attaque différentielle (regarder l'agrégat avant et après l'arrivée d'un vote).

> **Reco.** 5 votants et 8 membres, plus le contrôle différentiel : le seuil seul ne suffit pas, c'est la variation entre deux affichages qui trahit.

#### Q22 — [BLOQUANT] Quelle est la formulation exacte de la promesse d'anonymat faite à l'utilisateur ?
**Enjeu.** « Vos votes sont anonymes » est une affirmation absolue qu'aucune architecture ne peut tenir ; une promesse fausse est un défaut d'information au sens du RGPD et, plus grave, une trahison quand elle sera démentie par un cas concret dans une famille.
- **« Vos votes sont anonymes »** — faux, et ce sera démontré publiquement un jour.
- **« Les autres membres ne voient jamais vos votes individuels »** — vrai au sens strict de l'affichage, trompeur quant à l'inférence.
- **« Personne ne voit vos votes. Vos jugements nourrissent votre profil, qui est visible : un observateur très attentif pourrait en déduire des choses. Voici comment nous limitons cela. »** — honnête, plus long, et c'est exactement le ton « calme, élégant » revendiqué.
- **« Vos votes sont anonymes entre membres non coalisés »** — exact, illisible.

> **Reco.** La troisième formulation, avec un lien vers une page qui explique l'inférence par le profil : dire soi-même la faiblesse est la seule façon qu'elle ne se retourne pas contre le produit.

#### Q23 — [STRUCTURANT] Les agrégats de groupe affichent-ils la distribution des mentions, ou seulement le résultat ?
**Enjeu.** Une distribution sur 5 mentions et 4 propositions dans un groupe de 8 est une empreinte : elle contraint fortement l'ensemble des votes individuels compatibles.
- **Distribution complète** — riche, et c'est le vecteur principal de l'attaque par soustraction.
- **Mention majoritaire par proposition et rien d'autre** — pauvre visuellement, très protecteur.
- **Distribution complète au-dessus de 12 membres, mention majoritaire seule en dessous** — palier progressif, cohérent.
- **Distribution arrondie à des paliers grossiers** — l'arrondi doit être conçu pour être non inversible, ce qui n'est pas trivial.

> **Reco.** Palier progressif : mention majoritaire seule jusqu'à 12 membres, distribution au-delà.

#### Q24 — [STRUCTURANT] La comparaison « votre groupe est plus divisé que la moyenne » est-elle recalculée à chaque nouveau vote ?
**Enjeu.** Un indicateur qui bouge à l'instant où une personne vote signale qu'elle vient de voter, et la direction du mouvement indique dans quel sens.
- **Recalcul temps réel** — signal direct, et c'est la fonctionnalité la plus tentante à écrire.
- **Recalcul à la clôture de la journée seulement** — supprime le canal, coût nul.
- **Recalcul temps réel mais affichage figé jusqu'à la clôture** — équivalent au précédent, plus complexe.

> **Reco.** Rien ne s'affiche avant la clôture de la journée, et surtout pas la notification « tout le monde a voté », qui est un signal d'horodatage sur le dernier votant.

#### Q25 — [STRUCTURANT] Le rejeu d'archives est-il visible d'une manière ou d'une autre par les autres membres ?
**Enjeu.** Rejouer une vieille question fait bouger le radar hors du rythme quotidien, ce qui isole une contribution unique et rend l'inversion de Q20 encore plus facile ; et le fait même de rejouer une question précise est une information.
- **Le rejeu nourrit le radar visible immédiatement** — pire cas : contribution isolée, donc parfaitement inversible.
- **Le rejeu nourrit le radar personnel, et n'entre dans le radar visible qu'au lot hebdomadaire** — cohérent avec la reco de Q20.
- **Le rejeu ne nourrit pas le radar visible du tout** — crée deux radars divergents, source de confusion.

> **Reco.** Tout ce qui est visible par des tiers passe par le lot hebdomadaire, sans exception ni cas particulier : une seule règle, tenable.

---

## 6. Réidentification

#### Q26 — [BLOQUANT] Combien de jours de votes faut-il pour qu'un utilisateur devienne unique dans la base ? A-t-on fait le calcul ?
**Enjeu.** Le pseudonymat n'est protecteur que tant que la séquence de réponses ne singularise pas la personne ; l'enjeu réel est qu'une base « pseudonymisée » exfiltrée puis recoupée avec n'importe quelle autre source devienne nominative.

**Estimation, avec sa méthode.** *(calcul propre à ce dossier, à faire vérifier par le rôle Architecture ; les hypothèses d'entropie effective sont conservatrices)*

L'espace théorique d'une journée est de 6⁴ (mentions) × 4 (justification) × 5 (source épistémique) = **25 920 combinaisons, soit 14,7 bits**. Les réponses réelles étant corrélées et déséquilibrées, retenons une entropie effective très prudente de **5 à 8 bits par jour**.

Pour singulariser une personne dans une population de N, il faut environ log₂(N) bits :

| Population | Bits nécessaires | Jours de votes (à 5 bits/j) | Jours (à 8 bits/j) |
|---|---|---|---|
| Groupe famille (6) | 2,6 | < 1 | < 1 |
| Groupe classe (30) | 4,9 | 1 | < 1 |
| 5 000 utilisateurs | 12,3 | 3 | 2 |
| 100 000 utilisateurs | 16,6 | 4 | 2 à 3 |
| 1 000 000 | 19,9 | 4 | 3 |

À quoi s'ajoutent les quasi-identifiants **avant tout vote** : 6 tranches d'âge (~2,4 bits réels) et 18 régions (~3,5 bits réels compte tenu de la concentration en Île-de-France) = **environ 6 bits offerts gratuitement**. Conséquence : à 100 000 utilisateurs, il ne reste que ~11 bits à trouver, soit **deux jours de votes**.

Second calcul, plus concret : région × tranche d'âge = 108 cellules. À 5 000 utilisateurs, la moyenne est de 46 par cellule, mais la répartition est très inégale — la Corse pèse ~0,5 % de la population, soit ~25 utilisateurs répartis sur 6 tranches, donc **des cellules de 2 à 8 personnes dès la v1**. Pour ces personnes, le triplet (pseudo, tranche d'âge, région) est déjà quasi identifiant sans aucun vote.

- **On accepte, on ne change rien** — alors il faut cesser d'appeler la base « pseudonymisée » dans la documentation et dans l'information des utilisateurs.
- **On supprime la région et la tranche d'âge en v1** — retire 6 bits et supprime le problème des petites cellules. C'est la mesure la plus efficace et la moins chère du dossier.
- **On regroupe : 3 tranches d'âge, 5 grandes zones** — réduit à ~3,7 bits et supprime les cellules à 2 personnes.
- **On chiffre les colonnes et on considère le problème réglé** — non : le chiffrement protège contre l'exfiltration, pas contre l'inférence par quelqu'un qui a un accès légitime.

> **Reco.** Ne pas collecter région et tranche d'âge en v1 (Q5) ; si elles reviennent, 3 tranches et 5 zones maximum, jamais croisées dans un affichage public.

#### Q27 — [STRUCTURANT] Les résultats nationaux publics sont-ils publiés croisés par âge et région ?
**Enjeu.** Le croisement transforme les agrégats publics en outil de réidentification par recoupement : c'est exactement le mécanisme documenté par la littérature sur l'unicité (4 points suffisent à identifier 95 % des individus dans un jeu de données de mobilité ; 15 attributs démographiques suffisent à réidentifier 99,98 % des Américains).
- **Publication croisée dès la v1** — donne à un tiers la table de correspondance dont il a besoin.
- **Publication nationale globale uniquement** — protège, et suffit largement au produit.
- **Croisement avec un plancher de 100 répondants par cellule** — praticable à grande échelle seulement.

> **Reco.** Agrégat national global en v1, aucun croisement ; le croisement se rouvre quand une cellule ne descend jamais sous 100 répondants, ce qui suppose plusieurs dizaines de milliers d'utilisateurs actifs.

#### Q28 — [STRUCTURANT] Existe-t-il un annuaire, une recherche par pseudo, ou un moyen de savoir si une personne donnée est sur Endoxa ?
**Enjeu.** C'est la porte d'entrée du ciblage d'un individu : sans elle, l'attaquant qui veut le profil d'une personne précise doit d'abord la trouver.
- **Recherche par pseudo pour inviter des amis** — commode, et transforme la base en annuaire d'opinions interrogeable.
- **Aucune recherche ; on entre dans un groupe uniquement par lien reçu** — protecteur, et suffisant pour des groupes qui existent déjà hors ligne (c'est la cible du produit).
- **Recherche par pseudo exact uniquement, avec limitation de débit** — l'énumération reste possible sur des pseudos devinables.

> **Reco.** Aucune recherche, aucun annuaire, liens d'invitation à usage unique et expirants : le produit vise des groupes préexistants, il n'a besoin d'aucune découverte.

---

## 7. Droits des personnes

#### Q29 — [BLOQUANT] Que signifie « rectifier » un profil de valeurs calculé, et le droit s'applique-t-il ?
**Enjeu.** Le droit de rectification porte sur l'exactitude ; un score inféré n'est ni vrai ni faux au sens factuel, mais l'utilisateur qui se voit décrit comme « très haut en pouvoir » et le conteste a une demande légitime que ni le déni ni le silence ne traitent.
- **On refuse : le profil est un calcul, il n'y a rien à rectifier** — position juridiquement contestable et humainement mauvaise.
- **On rectifie l'entrée, pas la sortie : l'utilisateur peut modifier ou supprimer n'importe lequel de ses votes passés, et le profil se recalcule** — c'est la bonne réponse, elle est cohérente avec « changer d'avis est valorisé », et elle est déjà à moitié dans le cadrage.
- **On permet de neutraliser une dimension du radar** — brise la validité de la mesure et n'est pas de la rectification.
- **On ajoute une contestation attachée au profil, sans recalcul** — utile en complément, insuffisant seul.

> **Reco.** Rectification par l'entrée avec recalcul complet, plus une explication accessible du calcul : c'est aussi le meilleur argument pédagogique du produit.

#### Q30 — [BLOQUANT] Un vote effacé disparaît-il d'un résultat national déjà publié ?
**Enjeu.** L'effacement doit être réel ; mais recalculer un résultat officiel publié le rend instable, et un résultat de jugement majoritaire qui change après coup ruine la crédibilité de la mécanique.
- **Le résultat publié est figé, les votes effacés y restent** — alors l'effacement n'est pas complet, et il faut le dire.
- **Le résultat est recalculé à chaque effacement** — l'archive devient mouvante ; un résultat historique cité par quelqu'un ne sera plus le même.
- **Le résultat publié est figé mais **anonymisé irréversiblement** à la clôture : on ne conserve que des compteurs, sans lien vers les utilisateurs** — l'effacement du vote individuel est alors total puisqu'il n'y a plus de vote individuel dans le résultat, seulement un nombre. Juridiquement propre, produit stable.
- **On ne publie pas de résultat national** — retire un pilier du produit.

> **Reco.** Anonymisation irréversible des agrégats à la clôture quotidienne : compteurs seuls, aucun identifiant, aucune possibilité de reconstruction. Cette décision doit être prise avant le schéma de données.

#### Q31 — [BLOQUANT] Le versionnement des votes (cadrage §10 : « `Vote` est versionné plutôt qu'écrasé ») survit-il au droit à l'effacement ?
**Enjeu.** L'historique des changements d'avis est une fonctionnalité revendiquée ; c'est aussi une conservation de toutes les opinions politiques successives d'une personne, y compris celles qu'elle a explicitement abandonnées, ce qui est le contraire de la minimisation.
- **On conserve toutes les versions indéfiniment** — difficile à justifier pour de l'art. 9, et c'est ce que réquisitionnerait une autorité.
- **On conserve les versions mais l'utilisateur peut purger son historique de changements d'un geste** — bon compromis, la fonctionnalité reste par défaut.
- **On ne conserve que le vote courant plus un compteur « a changé d'avis N fois »** — suffit à « célébrer » le changement d'avis sans conserver l'opinion abandonnée. Minimisation réelle.
- **On conserve les versions 90 jours puis on écrase** — arbitraire mais défendable.

> **Reco.** Vote courant + compteur de changements + date du dernier changement : c'est tout ce dont le produit a besoin, et cela retire de la base l'objet le plus sensible qu'elle contiendrait.

#### Q32 — [STRUCTURANT] À quoi ressemble l'export de portabilité, et contient-il le profil calculé ?
**Enjeu.** La portabilité porte sur les données fournies par la personne ; le profil calculé n'en fait pas partie au sens strict, mais l'exclure d'un export serait perçu comme une rétention de la seule chose qui a de la valeur.
- **Export des seuls votes bruts, JSON** — conforme au minimum, décevant.
- **Export des votes + du profil + des vecteurs utilisés pour le calculer** — au-delà de l'obligation, et c'est la démonstration la plus convaincante de la transparence revendiquée en §9.
- **Export en PDF lisible** — bon pour l'utilisateur, mauvais pour la portabilité (non réutilisable).
- **Les deux formats** — coût faible, effet maximal.

> **Reco.** JSON complet incluant les vecteurs, plus un PDF lisible : c'est le geste de confiance le moins cher du dossier.

#### Q33 — [STRUCTURANT] Combien de temps entre une demande de suppression et l'effacement effectif dans les sauvegardes ?
**Enjeu.** Une sauvegarde conservée un an contient les opinions politiques de personnes qui ont demandé leur effacement onze mois plus tôt ; c'est l'écart le plus courant entre la promesse et la réalité.
- **Sauvegardes tournantes sur 7 jours** — effacement quasi immédiat, mais protection faible contre un incident découvert tardivement.
- **Sauvegardes tournantes sur 35 jours, sans archive longue** — bon équilibre, et l'engagement « supprimé partout sous 35 jours » est tenable et vérifiable.
- **Sauvegardes mensuelles conservées un an** — confortable techniquement, indéfendable pour de l'art. 9.
- **Suppression immédiate en base + liste de suppression rejouée à chaque restauration** — pratique courante ; à condition que la liste soit elle-même testée.

> **Reco.** Rotation à 35 jours **et** liste de suppression rejouée à la restauration, avec un test de restauration effectif au moins une fois avant la mise en production.

---

## 8. Sous-traitance IA

#### Q34 — [BLOQUANT] Une donnée utilisateur part-elle, à un moment quelconque, chez un prestataire de modèle de langage ?
**Enjeu.** L'architecture décrite au cadrage §9 n'en a pas besoin — l'IA génère la question et les vecteurs **avant** tout vote, à partir de flux de presse publics ; si cette propriété est tenue, Endoxa peut affirmer qu'aucune opinion d'utilisateur ne quitte jamais son infrastructure, ce qui est un argument rare et vérifiable.
- **Aucune donnée utilisateur ne part, jamais** — atteignable par conception, et c'est la seule réponse qui supprime la question du transfert hors UE, du contrat de sous-traitance, du risque d'entraînement et de la rétention côté fournisseur.
- **Des données agrégées partent (« résumer les tendances de la semaine »)** — un agrégat portant sur un petit groupe reste une donnée personnelle ; la porte, une fois ouverte, ne se referme pas.
- **Des données individuelles partent pour personnaliser un texte** — impose un contrat art. 28, des clauses contractuelles types si l'hébergement est hors UE, une analyse d'impact du transfert, et le consentement explicite au transfert. Plusieurs semaines de travail pour un gain cosmétique.
- **On ne sait pas encore** — inacceptable : c'est une propriété d'architecture, elle se décide avant le premier appel d'API.

> **Reco.** Poser comme invariant d'architecture qu'aucune donnée utilisateur ne transite par un modèle tiers, l'écrire dans le registre, et le vérifier par un test automatisé qui échoue si un champ utilisateur apparaît dans un prompt sortant.

#### Q35 — [STRUCTURANT] Le pipeline IA est-il isolé des contenus utilisateur, y compris des signalements d'équité ?
**Enjeu.** Le signalement d'équité (cadrage §9) est un texte ou un motif fourni par un utilisateur ; s'il remonte dans un prompt de la seconde IA de contrôle, l'invariant de Q34 est rompu, et une injection de prompt devient possible.
- **Les signalements ne sont jamais transmis à l'IA** — traitement humain ou par seuil purement numérique.
- **Seuls des compteurs numériques remontent** — préserve l'invariant.
- **Les motifs de signalement sont transmis pour aider la révision** — rompt l'invariant et ouvre l'injection.

> **Reco.** Compteurs numériques uniquement, motifs en liste fermée (pas de texte libre), traitement humain au-delà du seuil.

#### Q36 — [STRUCTURANT] Les flux RSS entrants sont-ils traités comme des données non fiables, susceptibles de contenir des instructions ?
**Enjeu.** Le pipeline lit du texte public non maîtrisé et le donne à une IA qui rédige la question du jour ; c'est une surface d'injection directe sur le contenu politique vu par tous les utilisateurs, et c'est probablement la vulnérabilité la plus intéressante pour les « hackers politiques » redoutés.
- **Traitement brut, confiance implicite** — un article piégé peut orienter la question du jour de toute la France.
- **Séparation stricte instruction / donnée, contenu injecté en balise fermée, sortie contrainte à un schéma strict** — nécessaire, coût faible.
- **Le précédent, plus une revue humaine obligatoire avant publication** — la seule protection réellement solide, mais elle impose une astreinte quotidienne (voir le dossier Architecture, Q2).

> **Reco.** Sortie contrainte à un schéma + revue humaine obligatoire en v1 : tant que le volume est d'une question par jour, la revue humaine coûte dix minutes et vaut toutes les défenses techniques.

---

## 9. Analyse d'impact, registre, gouvernance

#### Q37 — [BLOQUANT] L'AIPD est-elle obligatoire, et quand est-elle écrite ?
**Enjeu.** L'AIPD n'est pas un document de conformité tardif : elle est obligatoire **avant** le traitement, et si elle est écrite après la mise en production, elle constate au lieu de décider.

Endoxa réunit, selon les critères du CEPD repris par la CNIL, au moins cinq des neuf critères (données sensibles ; évaluation ou notation ; croisement de données ; personnes vulnérables si des mineurs sont admis ; usage innovant — IA générative). Deux suffisent. Par ailleurs l'art. 35.3.b la rend obligatoire dès que le traitement de données de l'art. 9 est « à grande échelle », seul point discutable.

- **Oui, écrite avant la première ligne de code** — la position juste ; l'AIPD sert alors de cahier des charges de sécurité.
- **Oui, écrite avant l'ouverture aux premiers utilisateurs** — acceptable en pratique.
- **Non, au motif que l'échelle est petite** — le raisonnement peut tenir pour l'art. 35.3.b seul, mais il ne tient pas face aux critères croisés ; et il ne tient plus du tout dès que le service croît.
- **Plus tard, si ça décolle** — c'est le moment où elle sera la plus coûteuse et la moins utile.

> **Reco.** Écrite avant le code, avec l'outil PIA de la CNIL (gratuit, open source) ; compter 3 à 5 jours de travail pour une première version sérieuse, ou 3 000 à 8 000 € si elle est confiée à un cabinet. Elle se met à jour, elle ne se refait pas.

#### Q38 — [STRUCTURANT] Faut-il désigner un délégué à la protection des données ?
**Enjeu.** L'art. 37.1.c rend le DPO obligatoire quand l'activité de base consiste en un traitement à grande échelle de données de l'art. 9 ; pour Endoxa, l'activité de base **est** ce traitement — seul le critère d'échelle protège encore.
- **Pas de DPO en v1, échelle réduite, avec un seuil déclencheur écrit** — position tenable si le seuil est fixé maintenant (par exemple 10 000 utilisateurs actifs) et respecté.
- **DPO externe mutualisé dès le départ** — 200 à 600 €/mois pour une petite structure ; achète surtout un interlocuteur en cas de contrôle.
- **DPO volontaire non déclaré** — sans effet juridique, mais utile en interne.
- **Pas de DPO, pas de seuil** — c'est la configuration où l'obligation se déclenche sans que personne ne s'en aperçoive.

> **Reco.** Pas de DPO en v1 mais un seuil écrit dans le registre et une alerte automatique quand il approche.

#### Q39 — [DÉTAIL] Qui est le responsable de traitement, nommément, tant que la nature du projet n'est pas tranchée ?
**Enjeu.** Le cadrage laisse la nature du projet ouverte (§16) ; en l'absence de personne morale, le responsable de traitement est une personne physique, qui répond personnellement, y compris au pénal de l'art. 226-19.
- **Personne physique, projet personnel** — juridiquement clair et personnellement exposé.
- **Association loi 1901** — quelques semaines, coût faible, écran de responsabilité et continuité au-delà d'une personne.
- **Société** — pertinent seulement si un modèle économique existe.

> **Reco.** Créer la structure **avant** l'ouverture aux utilisateurs : c'est la seule décision de ce dossier qui protège le porteur du projet lui-même.

---

## 10. Sécurité, conservation, hébergement

#### Q40 — [BLOQUANT] Où est hébergée la base, chez qui, et sous quelle juridiction ?
**Enjeu.** Une base d'opinions politiques françaises hébergée chez un fournisseur soumis à une législation extraterritoriale est un problème dont aucune clause contractuelle ne débarrasse ; c'est aussi le premier point que regardera un utilisateur méfiant.
- **Fournisseur français, données et sauvegardes en France** — la réponse attendue, et elle est disponible à coût comparable.
- **Fournisseur européen non soumis au CLOUD Act** — équivalent.
- **Fournisseur américain avec région européenne** — les données sont en Europe, la société ne l'est pas ; défendable juridiquement depuis le cadre d'adéquation, indéfendable en communication sur ce produit précis.
- **Qualification SecNumCloud** — hors de proportion pour la v1, mais c'est la référence à citer si un usage institutionnel apparaît.

> **Reco.** Fournisseur français, base et sauvegardes, et le dire en clair dans l'application : sur ce produit, l'hébergement est un argument, pas une ligne de configuration.

#### Q41 — [STRUCTURANT] Endoxa est-il soumis à l'obligation de conservation d'un an des données d'identification (art. 6 LCEN, décret 2021-1362) ?
**Enjeu.** Cette obligation, si elle s'applique, contredit directement la minimisation : elle imposerait de conserver un an ce qu'on cherche à ne pas avoir. Le point est ouvert : les utilisateurs d'Endoxa ne publient pas de contenu au sens habituel, ils choisissent parmi des propositions.
- **On considère qu'elle ne s'applique pas (aucun contenu créé et mis en ligne par l'utilisateur)** — position raisonnable, à documenter par écrit et à faire valider.
- **On considère qu'elle s'applique et on conserve un an** — on constitue volontairement le fichier qu'on redoute.
- **On ne tranche pas** — en cas de réquisition, on improvisera dans l'urgence.

> **Reco.** Trancher par écrit dans l'AIPD, en sens « non applicable », en s'appuyant sur l'absence de contenu produit par l'utilisateur ; et concevoir pour n'avoir de toute façon presque rien à donner.

#### Q42 — [STRUCTURANT] Que contiennent les journaux techniques, et combien de temps sont-ils gardés ?
**Enjeu.** Les journaux sont l'angle mort classique : une adresse IP associée à un horodatage de vote reconstitue le lien entre une personne et une opinion, même si la base applicative est irréprochable.
- **Journaux complets avec IP, 12 mois** — le fichier d'opinions horodatées qu'on prétendait ne pas avoir.
- **IP tronquées ou hachées avec sel tournant, 6 mois** — protège l'essentiel tout en conservant l'utilité de sécurité.
- **Aucune IP dans les journaux applicatifs, IP seulement dans le pare-feu à 7 jours** — le plus protecteur, suffisant pour la lutte anti-abus si la détection est faite autrement.
- **Journaux d'accès administrateur conservés séparément, 12 mois, non modifiables** — nécessaire dans tous les cas, et c'est la seule catégorie qu'il faut garder longtemps.

> **Reco.** Aucune IP dans les journaux applicatifs, IP en pare-feu à 7 jours, journaux d'accès administrateur séparés et immuables à 12 mois.

#### Q43 — [STRUCTURANT] Quelle est la durée de conservation d'un compte inactif ?
**Enjeu.** Un compte abandonné est une base d'opinions politiques sans propriétaire pour la surveiller ; la conservation « au cas où » est le défaut le plus fréquemment sanctionné.
- **Indéfinie** — indéfendable pour de l'art. 9.
- **24 mois d'inactivité, avec relance à 22 mois puis suppression** — standard raisonnable.
- **12 mois** — plus protecteur, et cohérent avec un produit dont l'usage est quotidien : un an sans revenir signifie un abandon.
- **36 mois** — trop long ici.

> **Reco.** 12 mois d'inactivité, relance à 11 mois, suppression complète ensuite : le rituel est quotidien, un an d'absence n'a pas d'ambiguïté.

#### Q44 — [DÉTAIL] Qu'est-ce qui est chiffré, avec quelles clés, et où sont les clés ?
**Enjeu.** « Les données sont chiffrées » ne veut rien dire si la clé est dans la même base ou le même dépôt ; l'enjeu réel est le scénario d'exfiltration, où seule la séparation des clés change quelque chose.
- **TLS + chiffrement de volume par le fournisseur** — protège contre le vol de disque, pas contre l'accès applicatif ni contre une injection SQL. C'est le niveau par défaut, souvent présenté comme suffisant.
- **Le précédent + chiffrement applicatif des colonnes sensibles (mentions, justifications, sources épistémiques, radars) avec une clé en gestionnaire de secrets** — une exfiltration de base ne livre que du chiffré.
- **Chiffrement de bout en bout côté client** — supprimerait tout calcul serveur, donc le jugement majoritaire et les agrégats. Incompatible avec le produit.
- **Sauvegardes chiffrées avec une clé distincte, chez un autre fournisseur** — indispensable en complément : la sauvegarde est la cible la plus faible.

> **Reco.** Chiffrement applicatif des colonnes sensibles avec clé hors base, sauvegardes chiffrées avec une clé distincte chez un second fournisseur ; à décider avant le schéma, impossible à rétrofitter sans migration complète.

---

## 11. Fin de vie du service

#### Q45 — [BLOQUANT] Que devient la base en cas de revente, de rachat ou de faillite ?
**Enjeu.** Le consentement explicite a été donné à Endoxa pour des finalités précises : il ne suit pas automatiquement un repreneur, et la CNIL rappelle qu'un fichier ne peut être cédé que s'il a été constitué régulièrement et que le repreneur doit informer les personnes et démontrer leur consentement pour tout nouvel usage. En liquidation, c'est le liquidateur qui décide, pas le fondateur — sauf si la décision a été prise avant.
- **Rien n'est prévu** — le liquidateur cherchera à valoriser l'actif, et un fichier d'opinions politiques de plusieurs dizaines de milliers de Français est un actif que quelqu'un voudra.
- **Engagement public de destruction en cas d'arrêt, inscrit dans les CGU et dans les statuts** — les CGU seules sont opposables mais faibles face à une procédure collective ; les statuts d'une association qui prévoient la dévolution et la destruction sont plus solides.
- **Engagement de destruction + effacement automatique programmé si le service ne répond plus pendant 90 jours** — le seul dispositif qui fonctionne sans intervention humaine, donc le seul qui fonctionne en faillite.
- **Cession possible avec re-consentement de chaque personne** — techniquement correct, pratiquement équivalent à une destruction (le taux de re-consentement sera faible).

> **Reco.** Les trois à la fois : clause statutaire, engagement public, et mécanisme d'effacement automatique par défaut d'activité. Cette décision se prend à la création de la structure, jamais après.

#### Q46 — [STRUCTURANT] Que se passe-t-il en cas de violation de données, et le scénario a-t-il été répété ?
**Enjeu.** Une violation portant sur des données de l'art. 9 déclenche presque certainement l'obligation d'informer chaque personne concernée, en plus de la notification CNIL sous 72 heures ; le risque réel est qu'il faille annoncer à des dizaines de milliers de personnes que leurs opinions politiques ont fuité, avec un pseudonyme qui ne les protège pas (voir Q26).
- **On improvisera** — 72 heures est très court quand on découvre l'incident un samedi.
- **Procédure écrite : qui décide, quel modèle de notification, quel canal vers les utilisateurs, quel gel du service** — deux heures de rédaction, valeur maximale.
- **Le précédent, plus un exercice à blanc une fois avant l'ouverture** — la seule façon de savoir si la procédure tient.

> **Reco.** Procédure écrite et un exercice à blanc, avant l'ouverture aux utilisateurs ; et vérifier que l'on dispose d'un canal pour joindre les utilisateurs, ce qui n'est pas évident si l'on a choisi de ne pas collecter d'adresse e-mail (Q11).

---

## Décisions irréversibles

*Choix qui, mal pris au départ, ne pourront plus être corrigés — au sens fort : soit la correction est impossible, soit elle impose de détruire les comptes existants ou de re-solliciter toute la base.*

| Décision | Pourquoi c'est irréversible | Dernier moment pour la prendre |
|---|---|---|
| **Fraîcheur du radar visible par les tiers** (Q20) | Une fois les radars observés quotidiennement, les votes passés de tous les membres sont reconstituables par quiconque a archivé des captures. Rien ne rattrape ce qui a été observé. | Conception du schéma de données de la v1 — l'historique des radars publiés doit exister dès le début |
| **Publication des vecteurs de valeurs** (Q20) | Une fois publiés, ils sont copiés ; on ne les dépublie pas. Ils rendent l'inversion possible rétroactivement sur tout l'historique. | Première publication d'une question |
| **Versionnement des votes** (Q31) | Un historique d'opinions abandonnées, une fois constitué, ne peut être purgé sélectivement sans détruire la fonctionnalité qu'il servait. | Schéma de données v1 |
| **Anonymisation des agrégats à la clôture** (Q30) | Si les agrégats gardent un lien vers les utilisateurs, tous les résultats déjà publiés restent liés à des personnes, définitivement. | Schéma de données v1 |
| **Collecte de la région et de la tranche d'âge** (Q5, Q26) | Les données collectées en granularité fine ne redeviennent pas grossières : la finesse est déjà en base et en sauvegarde. | Premier écran d'inscription |
| **Identifiant de compte (e-mail ou non)** (Q11) | Passer d'un modèle avec e-mail à un modèle sans détruit tous les comptes existants. | Ouverture des inscriptions |
| **Découpage du consentement en finalités** (Q7) | Une finalité oubliée exige de re-solliciter toute la base ; le taux de réponse à une re-sollicitation se situe entre 20 et 40 %. | Premier utilisateur réel |
| **Chiffrement applicatif des colonnes sensibles** (Q44) | Rétrofitter impose une migration complète et laisse les sauvegardes anciennes en clair. | Schéma de données v1 |
| **Envoi de données utilisateur à un modèle tiers** (Q34) | Ce qui est parti ne revient pas, et le fournisseur peut l'avoir conservé ou utilisé. | Premier appel d'API du pipeline |
| **Sort de la base en fin de vie** (Q45) | Après l'ouverture d'une procédure collective, la décision appartient au liquidateur. | Création de la structure juridique |
| **Autorisation des groupes professionnels** (Q17) | Une fois des groupes d'entreprise installés, les supprimer casse des usages et des relations existants. | Conception de la v2 |
| **Seuil d'affichage des agrégats de groupe** (Q21) | Relever le seuil ne rattrape pas ce qui a déjà été affiché à des membres. | Conception de la v2 |

---

## Modèle de menace

*Capacité : niveau de compétence et de moyens nécessaires. Impact : conséquence si l'attaque réussit.*

| Attaquant | Motivation | Capacité requise | Impact | Contre-mesure minimale en v1 |
|---|---|---|---|---|
| **Le membre curieux du groupe** (l'oncle, le chef, le prof) | Savoir comment un proche a voté | Aucune : une capture d'écran par jour du radar d'un autre membre | Rupture de la promesse centrale ; conflit familial, pression professionnelle. **Scénario le plus probable de tous.** | Radar tiers mis à jour uniquement par lots hebdomadaires (Q20) ; seuils d'affichage (Q21) ; formulation honnête de la promesse (Q22) |
| **La brigade militante organisée** (les « hackers politiques ») | Faire dire au résultat national ce qui les arrange, puis le citer | Moyenne : création automatisée de comptes, scriptage du parcours de vote | Le résultat public devient un instrument de communication ; la crédibilité du produit s'effondre en une journée | Pas de compte instantané en masse (vérification par passkey ou e-mail, limitation par IP) ; détection des parcours trop rapides et des comptes corrélés ; publication du résultat seulement à la clôture, avec le nombre de votants |
| **L'attaquant externe cherchant la base** | Un fichier d'opinions politiques de Français a une valeur marchande réelle, et un vol retentissant a une valeur symbolique | Moyenne à élevée : injection SQL, vol de secrets, accès à un stockage de sauvegarde mal configuré | Catastrophique : notification à toutes les personnes (art. 34), fin probable du projet | Chiffrement applicatif des colonnes sensibles avec clé hors base (Q44) ; sauvegardes chiffrées avec clé distincte chez un second fournisseur ; aucun export administrateur en un clic ; journaux d'accès admin immuables |
| **L'employeur** | Connaître les convictions de ses salariés avant une promotion ou un licenciement | Aucune : il crée le groupe et invite | Discrimination sur les opinions politiques — délit pénal (art. 225-1/225-2 C. pén.), et Endoxa en est l'outil | Symétrie stricte : aucun créateur de groupe n'a de pouvoir de lecture supplémentaire (Q17) ; masquage de radar par groupe, indiscernable (Q18) |
| **L'empoisonneur du pipeline** | Orienter les profils de valeurs de tous les utilisateurs, durablement et sans que cela se voie | Moyenne : article de presse piégé injecté dans un flux RSS, ou signalements d'équité coordonnés | Le miroir ment ; l'atteinte est invisible et rétroactive sur tout l'historique | Séparation stricte instruction/donnée dans les prompts, sortie contrainte à un schéma (Q36) ; revue humaine avant publication ; vecteurs figés et versionnés une fois publiés ; signalements en liste fermée sans texte libre (Q35) |
| **L'autorité requérante** (justice, renseignement) | Identifier l'auteur d'un propos, ou suivre une personne | Aucune : la réquisition est légale et opposable | Le fichier d'opinions devient consultable par l'État ; aucune défense juridique possible | Ne pas avoir la donnée : pas d'IP dans les journaux applicatifs (Q42), pas d'e-mail obligatoire (Q11), pas de versions de votes conservées (Q31) ; position écrite sur l'art. 6 LCEN (Q41) |
| **Le harceleur ciblant une personne précise** (ex-conjoint, journaliste, élu) | Obtenir le profil politique d'un individu nommé | Faible : ingénierie sociale sur un lien d'invitation, ou création d'un groupe piège | Gravité maximale sur une personne, y compris physique | Aucun annuaire, aucune recherche par pseudo (Q28) ; liens d'invitation à usage unique et expirants ; acceptation explicite avant d'entrer dans un groupe |
| **Le réidentifiant par recoupement** (courtier, chercheur, journaliste) | Relier les pseudos à des identités réelles | Élevée : croisement des agrégats publics et des séquences de votes avec d'autres sources | Le pseudonymat tombe pour une fraction importante de la base | Ne pas collecter région et tranche d'âge en v1 (Q5) ; aucun agrégat public croisé (Q27) ; anonymisation irréversible des agrégats à la clôture (Q30) |
| **Le maître-chanteur** | Monnayer le silence auprès d'une personne exposée dont le profil contredit la position publique | Faible s'il est déjà membre d'un groupe ; élevée sinon | Grave et individuel ; c'est le scénario qui produit un titre de presse | Cumul des contre-mesures « membre curieux » et « harceleur ciblé » |
| **Le repreneur ou le liquidateur** | Valoriser la base comme un actif | Aucune : la procédure lui en donne le droit | Le fichier change de mains et de finalité, sans que personne n'ait rien à dire | Clause statutaire de destruction + effacement automatique programmé par défaut d'activité (Q45) |
| **Le sous-traitant de modèle de langage** | Rétention, entraînement, conservation par défaut | Aucune : ce sont les conditions contractuelles par défaut | Transfert hors UE de données de l'art. 9, sans base légale | Invariant d'architecture : aucune donnée utilisateur dans un prompt sortant, vérifié par un test automatisé (Q34) |
| **L'utilisateur contre lui-même** | Partager fièrement son bilan du dimanche | Aucune | Auto-divulgation d'opinions politiques sur un réseau social tiers, irréversible | Le partage exporte une image sans le radar brut ni les votes ; avertissement au premier partage |

---

## Sources

**Note de méthode.** L'accès direct aux sites `cnil.fr`, `legifrance.gouv.fr`, `eur-lex.europa.eu` et `edpb.europa.eu` est bloqué par la politique de sortie réseau de cet environnement. Les contenus ont été vérifiés par extraction via moteur de recherche, ce qui est un niveau de preuve inférieur à la consultation directe. **Les URL ci-dessous doivent être ouvertes et relues avant toute décision engageante.** Les deux points les plus incertains sont signalés en tant que tels : l'état exact de la loi française sur la majorité numérique (Q13) et l'applicabilité de l'art. 6 LCEN (Q41).

### Textes officiels et jurisprudence

| Référence | URL |
|---|---|
| RGPD, règlement (UE) 2016/679 — art. 9, 33, 34, 35, 37 | https://eur-lex.europa.eu/legal-content/FR/TXT/?uri=CELEX:32016R0679 |
| Code pénal, art. 226-19 — conservation de données révélant directement ou indirectement les opinions politiques sans consentement exprès : 5 ans et 300 000 € | https://www.legifrance.gouv.fr/codes/article_lc/LEGIARTI000037825496 |
| Loi Informatique et Libertés, art. 45 — consentement des mineurs, seuil de 15 ans | https://www.legifrance.gouv.fr/loda/article_lc/LEGIARTI000037817552 |
| CJUE, C-184/20, *OT c. Vyriausioji tarnybinės etikos komisija*, 1er août 2022 — les données révélant indirectement une donnée sensible relèvent de l'art. 9 | https://eur-lex.europa.eu/legal-content/FR/TXT/?uri=CELEX:62020CJ0184 |
| CNIL, délibération n° 2018-327 du 11 octobre 2018 — liste des traitements soumis à AIPD | https://www.legifrance.gouv.fr/jorf/id/JORFTEXT000037559521 |
| CNIL, délibération n° 2022-100 du 21 juillet 2022 — recommandation mots de passe et secrets partagés | https://www.legifrance.gouv.fr/jorf/id/JORFTEXT000046432885 |
| Décret n° 2021-1362 du 20 octobre 2021 — conservation des données d'identification, art. 6 LCEN | https://www.legifrance.gouv.fr/jorf/id/JORFTEXT000044228912 |
| Règlement (UE) 2024/900 du 13 mars 2024 — transparence et ciblage de la publicité à caractère politique ; interdiction du ciblage sur données sensibles, applicable depuis le 10 octobre 2025 | https://eur-lex.europa.eu/legal-content/FR/TXT/PDF/?uri=OJ:L_202400900 |
| DSA, règlement (UE) 2022/2065, art. 28 — protection des mineurs, interdiction de la publicité par profilage | https://www.arcom.fr/espace-professionnel/reglement-sur-les-services-numeriques-ou-dsa-obligations-et-services-concernes |

### Doctrine et recommandations d'autorités

| Référence | URL |
|---|---|
| CNIL — Les fichiers de communication politique (données sensibles, base légale, consentement explicite) | https://www.cnil.fr/fr/les-fichiers-de-communication-politique |
| CNIL — Ce qu'il faut savoir sur l'AIPD | https://www.cnil.fr/fr/ce-quil-faut-savoir-sur-lanalyse-dimpact-relative-la-protection-des-donnees-aipd |
| CNIL — Listes des traitements pour lesquels une AIPD est requise ou non | https://www.cnil.fr/fr/listes-des-traitements-pour-lesquels-une-aipd-est-requise-ou-non |
| CNIL — Infographie « Dois-je faire une AIPD ? » (les 9 critères) | https://www.cnil.fr/sites/cnil/files/atoms/files/infographie_aipd.pdf |
| CEPD — Lignes directrices 5/2020 sur le consentement (consentement explicite, liberté du consentement, déséquilibre de pouvoir) | https://www.edpb.europa.eu/our-work-tools/our-documents/guidelines/guidelines-052020-consent-under-regulation-2016679_fr |
| G29 / CEPD — Lignes directrices sur le consentement, WP259 rev. 0.1 | https://www.cnil.fr/sites/cnil/files/atoms/files/ldconsentement_wp259_rev_0.1_fr.pdf |
| CNIL — 8 recommandations pour renforcer la protection des mineurs en ligne (2021) | https://www.cnil.fr/fr/la-cnil-publie-8-recommandations-pour-renforcer-la-protection-des-mineurs-en-ligne |
| CNIL — Recommandation 4 : rechercher le consentement d'un parent pour les mineurs de moins de 15 ans | https://www.cnil.fr/fr/recommandation-4-rechercher-le-consentement-dun-parent-pour-les-mineurs-de-moins-de-15-ans |
| CNIL — Recommandation 1 : encadrer la capacité d'agir des mineurs en ligne | https://www.cnil.fr/fr/recommandation-1-encadrer-la-capacite-dagir-des-mineurs-en-ligne |
| CNIL — Anonymisation et pseudonymisation ; les données pseudonymisées restent des données personnelles | https://www.cnil.fr/fr/recherche-scientifique-hors-sante-enjeux-et-avantages-de-lanonymisation-et-de-la-pseudonymisation |
| CNIL — Recommandations pour les diffuseurs de données ouvertes (open data), juin 2024 | https://www.cnil.fr/sites/cnil/files/2024-06/recommandations_diffuseurs_de_donnees_ouvertes_open_data.pdf |
| CNIL — Fiches pratiques IA (sous-traitance, base légale, sécurité des systèmes d'IA, 2025) | https://www.cnil.fr/fr/les-fiches-pratiques-ia |
| CNIL — IA et RGPD, recommandations finalisées | https://www.cnil.fr/fr/ia-finalisation-recommandations-developpement-des-systemes-ia |
| CNIL — Vente de fichiers clients : la CNIL rappelle les règles (décembre 2022, affaire Camaïeu) | https://www.cnil.fr/fr/vente-de-fichiers-clients-la-cnil-rappelle-les-regles |
| CNIL — Encadrement de la publicité politique ciblée : mise à jour de doctrine | https://www.cnil.fr/fr/encadrement-de-la-publicite-politique-ciblee-la-cnil-met-jour-sa-doctrine |
| Insee — Principe 5 : secret statistique et protection des données (seuils de diffusion) | https://www.insee.fr/fr/information/4174982 |

### Littérature scientifique (chiffres de réidentification cités en Q26 et Q27)

| Référence | URL |
|---|---|
| Rocher, Hendrickx, de Montjoye, « Estimating the success of re-identifications in incomplete datasets using generative models », *Nature Communications*, 2019 — 15 attributs démographiques réidentifient 99,98 % des Américains | https://www.nature.com/articles/s41467-019-10933-3 |
| de Montjoye *et al.* — l'unicité comme métrique ; 4 points suffisent à identifier 95 % des individus dans un jeu de données de mobilité de 1,5 million de personnes | https://www.science.org/doi/10.1126/sciadv.adn7053 |
| « The risk of re-identification remains high even in country-scale location datasets », *Patterns*, 2021 — 93 % d'unicité sur 60 millions de personnes avec 4 points | https://www.sciencedirect.com/science/article/pii/S2666389921000143 |

### Sources de presse — à confirmer au texte officiel

| Sujet | URL |
|---|---|
| Loi sur la majorité numérique à 15 ans, adoption rapportée le 21 juillet 2026, première échéance annoncée au 1er septembre 2026 — **non vérifié au Journal officiel** | https://www.macg.co/services/2026/07/majorite-numerique-15-ans-fin-de-la-recre-pour-les-reseaux-sociaux-309877 |
| Application du règlement (UE) 2024/900 depuis le 10 octobre 2025 (ARPP) | https://www.arpp.org/actualite/publicite-politique-nouvelles-regles-publicite-politique-applicable-10-octobre-2025/ |

---

*Ce dossier est un premier jet au sens de `docs/methode.md` §1 : il n'a pas encore
été contesté par un autre rôle. Les contradictions les plus utiles viendraient
d'Architecture technique (faisabilité du chiffrement applicatif et du lot
hebdomadaire), de Psychométrie (validité du radar si la mise à jour visible est
différée) et de Rétention & groupe (coût produit des seuils d'affichage).*
