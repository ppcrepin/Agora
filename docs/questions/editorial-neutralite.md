# Questions — Éditorial et neutralité de l'IA

> Produit par le rôle correspondant de l'équipe (voir `docs/methode.md`).
> Questions, pas solutions. Niveaux : **BLOQUANT**, **STRUCTURANT**, **DÉTAIL**.

---

**Note de cadrage éditorial — 44 questions**
*Rôle de ce document : il ne propose aucun contenu. Il liste ce que le porteur du projet doit avoir décidé, par écrit, avant qu'une seule question soit mise en ligne devant un public français.*

---

## Avertissement liminaire

Trois constats structurent tout ce qui suit.

**1. Endoxa n'est pas un hébergeur, c'est un éditeur.** Il n'y a aucun contenu utilisateur en texte libre (décision actée du cadrage : justifications à choisir, pas à écrire). Tout ce qui s'affiche est produit par le dispositif. Le régime de responsabilité limitée des intermédiaires ne s'applique donc pas : Endoxa répond de chaque mot publié comme un journal répond de ses articles. « C'est l'IA qui l'a écrit » n'est pas un moyen de défense.

**2. La publication automatique est en tension frontale avec le cadre en vigueur.** L'article 50(4) du règlement européen sur l'IA impose de déclarer l'origine artificielle d'un texte publié pour informer le public sur des questions d'intérêt public — avec une exception lorsque le contenu a fait l'objet d'une révision humaine et qu'une personne assume la responsabilité éditoriale. Le choix « publication automatique » revient à renoncer volontairement à cette exception ([artificialintelligenceact.eu, art. 50](https://artificialintelligenceact.eu/article/50/)). C'est un choix possible, mais il doit être conscient et documenté.

**3. Le cadrage actuel qualifie le biais de formulation de « risque principal du produit » et lui oppose quatre parades, dont trois ne sont pas des contrôles.** La diversité des sources est une hygiène d'amont, pas un contrôle. Les prompts publics sont de la transparence, pas un contrôle. La notation par les utilisateurs est un contrôle *a posteriori*, après que des dizaines de milliers de personnes ont déjà voté. Il ne reste qu'un seul contrôle *a priori* : la seconde IA. Le document ci-dessous consacre une section entière à sa fiabilité, parce que tout le dispositif repose dessus.

**Répartition :** 17 BLOQUANT · 19 STRUCTURANT · 8 DÉTAIL

---

# I. Statut juridique et déontologique

### Q1 — Endoxa produit-il des « sondages » au sens de la loi du 19 juillet 1977 ? **[BLOQUANT]**

*Enjeu :* si la réponse est oui, une notice doit être déposée à la Commission des sondages à chaque publication, le texte intégral des questions et les marges d'erreur doivent être affichés, et la publication devient interdite la veille et le jour de chaque scrutin.

*Options :*
- **A — Poser que non, échantillon auto-sélectionné donc hors champ, et s'arrêter là.** Juridiquement défendable : la Commission considère que les consultations en ligne fondées sur le volontariat, sans échantillon représentatif, ne relèvent pas de la loi de 1977. Mais cette réponse ne protège de rien d'autre : elle laisse entier le risque de qualification par l'usage (voir Q2) et n'empêche pas la Commission d'émettre une mise au point publique sur l'emploi du vocabulaire.
- **B — Poser que non, mais s'imposer volontairement le standard de la loi** (texte intégral de la question archivé et consultable, nombre de répondants, dates, absence de marge d'erreur *expliquée* plutôt que masquée). Coût : un bandeau méthodologique permanent. Bénéfice : le dispositif devient inattaquable sur ce terrain et se dote d'une norme opposable en interne.
- **C — Demander un avis écrit à la Commission des sondages avant lancement.** Délai de plusieurs semaines, mais produit un document qu'on peut opposer à tout contradicteur, et fait entrer le projet dans le champ de vision de l'institution avant qu'un incident ne l'y fasse entrer.

*Reco :* **B + C.** Le hors-champ juridique est probable mais fragile ; s'imposer la norme coûte peu et retire l'argument à l'adversaire avant qu'il ne s'en serve.

---

### Q2 — Quels mots sont interdits, dans l'application, dans la communication et dans le code ? **[BLOQUANT]**

*Enjeu :* la Commission des sondages s'est opposée à l'emploi du terme « sondage » pour des opérations ne respectant pas les règles de l'art, notamment sans échantillon représentatif ; c'est le vocabulaire, plus que la méthode, qui déclenche la controverse.

*Options :*
- **A — Aucune liste, on écrit naturellement.** Garantit qu'un jour un écran, un tweet ou une capture affichera « 62 % des Français jugent… ». Cette phrase existera ensuite indépendamment de vous.
- **B — Lexique interdit court** : sondage, les Français, l'opinion publique, la France pense, majorité des Français. Lexique imposé : *les utilisateurs d'Endoxa ayant voté ce jour*. Appliqué aux écrans, aux notifications, aux métadonnées de partage et à la presse.
- **C — B, plus un test automatique en intégration continue** qui fait échouer le build si un terme interdit apparaît dans les chaînes de caractères de l'interface ou dans les gabarits de partage.

*Reco :* **C.** Le vocabulaire dérive toujours par le bas ; seul un contrôle mécanique tient dans la durée, et le coût est de deux heures de développement.

---

### Q3 — Qui est le directeur de la publication, et que signe-t-il ? **[BLOQUANT]**

*Enjeu :* le droit français impose un directeur de la publication à tout service de communication au public en ligne ; une personne physique nommée répondra de contenus qu'aucun humain n'aura lus avant publication.

*Options :*
- **A — Le porteur du projet, sans dispositif d'astreinte.** Il assume pénalement des textes qu'il découvre en même temps que le public, sans capacité de les retirer dans l'heure.
- **B — Personne physique nommée + astreinte de retrait** : un bouton de dépublication immédiate, une obligation de réponse sous deux heures en journée, une procédure écrite d'escalade.
- **C — Relecture humaine obligatoire avant publication**, ce qui abandonne le « publication automatique » du cadrage mais restaure l'exception éditoriale de l'article 50(4) du règlement IA et rend le rôle tenable.

*Reco :* **B au minimum, C si le sujet du jour est classé sensible** (voir Q11) — une responsabilité pénale sans capacité d'intervention n'est pas une gouvernance, c'est un pari.

---

### Q4 — Quelle base légale pour collecter des opinions politiques, et où sont hébergées les données ? **[BLOQUANT]**

*Enjeu :* les opinions politiques relèvent de l'article 9 du RGPD, dont le traitement est interdit par principe sauf exception ; un profil de valeurs sur dix dimensions construit à partir de jugements sur l'actualité en est un cas d'école.

*Options :*
- **A — Consentement implicite par l'usage** (« en votant vous acceptez »). Non conforme : l'article 9.2.a exige un consentement *explicite*, distinct, spécifique, révocable.
- **B — Consentement explicite au premier vote**, écran dédié, mention que le radar de valeurs est une inférence d'opinions politiques, révocation en un geste, export et suppression natifs, hébergement UE.
- **C — B + minimisation forte** : pas de tranche d'âge ni de région tant que l'usage statistique n'en est pas démontré, agrégats à seuil, et durée de conservation écrite.

*Reco :* **C.** Le cadrage identifie déjà ce risque comme élevé ; la seule vraie parade est de ne pas détenir ce qu'on n'exploite pas encore.

---

### Q5 — L'application est-elle ouverte aux mineurs ? **[BLOQUANT]**

*Enjeu :* l'usage scolaire est évoqué dans le cadrage « plus tard », mais l'ouverture publique le rendra effectif dès le premier mois, sans que rien n'ait été prévu.

*Options :*
- **A — Ouvert à tous, sans vérification.** Collecte d'opinions politiques de mineurs sans consentement parental : exposition réglementaire directe, et sujet de presse évident.
- **B — 15 ans et plus déclaratif** + interdiction explicite du profil de valeurs pour les comptes déclarés mineurs.
- **C — Majeurs uniquement en v1**, mode scolaire distinct traité comme un produit séparé avec son propre cadre (consentement parental, pas de profil individuel, agrégats de classe seulement).

*Reco :* **C.** Un dispositif qui infère les valeurs politiques d'un adolescent et les affiche à son groupe est un produit différent, qui exige un cadrage différent.

---

### Q6 — Comment déclare-t-on l'origine artificielle du contenu ? **[BLOQUANT]**

*Enjeu :* l'article 50(4) du règlement IA impose la divulgation pour les textes générés par IA publiés afin d'informer le public sur des questions d'intérêt public, sauf révision humaine et responsabilité éditoriale assumée ; les obligations de transparence sont entrées en application le 2 août 2026 (le calendrier des obligations de marquage/détection a fait l'objet d'ajustements — à vérifier à date).

*Options :*
- **A — Mention en page « à propos ».** Ne remplit pas l'obligation : la divulgation doit être perceptible là où le contenu est consommé.
- **B — Mention visible sur l'écran de la question du jour**, formulée simplement (« question, propositions et chiffres rédigés par une IA à partir de la revue de presse du jour, sans relecture humaine »), reprise dans les images de partage.
- **C — B + marquage machine-readable** des contenus générés et journal d'audit horodaté par question.

*Reco :* **B immédiatement, C avant toute API ou export.** Et noter que la phrase « sans relecture humaine » est le meilleur test de vérité du projet : si elle est inconfortable à afficher, c'est le dispositif qu'il faut changer, pas la phrase.

---

# II. Le choix du sujet

### Q7 — Quelle est la définition écrite d'un sujet éligible ? **[BLOQUANT]**

*Enjeu :* sans critère positif écrit, l'éligibilité sera définie *de facto* par le prompt, c'est-à-dire par personne.

*Options :*
- **A — « Actualité chaude, saillante dans plusieurs sources ».** C'est le cadrage actuel : critère de notoriété, pas d'éligibilité. Il sélectionne mécaniquement ce qui divise le plus, puisque c'est ce que la presse traite le plus.
- **B — Trois conditions cumulatives** : (1) le sujet porte sur une **décision publique** ou une orientation d'action collective, pas sur un fait ni sur une personne ; (2) il existe au moins **trois positions défendables** publiquement soutenues par des acteurs légitimes ; (3) le désaccord porte sur des **valeurs ou des arbitrages**, pas sur l'existence de faits.
- **C — B + quota** : pas plus de X questions par mois sur un même champ (immigration, sécurité, fiscalité), pour empêcher la dérive d'agenda.

*Reco :* **C.** Le critère (3) est le plus important : il exclut à lui seul la quasi-totalité des pièges décrits plus bas.

---

### Q8 — Quelle est la liste d'exclusion, et est-elle publique ? **[BLOQUANT]**

*Enjeu :* une liste d'exclusion non écrite n'existe pas ; une liste écrite mais secrète est elle-même un objet de soupçon.

*Options :*
- **A — Consignes dans le prompt uniquement.** Non testable, non opposable, modifiable en silence.
- **B — Liste publique et versionnée** : faits divers ; personnes physiques nommées ; affaires judiciaires en cours ; caractéristiques des personnes (origine, religion, orientation sexuelle, handicap, identité de genre) ; victimes identifiables ; santé individuelle ; sujets où une position constitue une infraction pénale.
- **C — B + liste des sujets *écartés* publiée chaque semaine**, avec le motif d'exclusion.

*Reco :* **C.** Publier ce qu'on refuse est le seul moyen de prouver qu'on refuse, et cette liste sera la meilleure défense le jour d'une accusation de partialité.

---

### Q9 — Que fait-on des sujets où « les deux camps » n'existent pas ? **[BLOQUANT]**

*Enjeu :* le format à quatre propositions équilibrées présuppose un désaccord légitime ; appliqué à un fait établi ou à un crime, il fabrique une controverse là où il n'y en a pas.

*Options :*
- **A — L'IA décide au cas par cas.** Garantit qu'un jour l'application demandera de juger au jugement majoritaire quatre positions sur la réalité du changement climatique, dont une négationniste — et l'affichera comme un désaccord respectable.
- **B — Règle d'asymétrie assumée** : quand le désaccord porte sur l'existence d'un fait établi par consensus scientifique ou judiciaire, le sujet est inéligible ; on peut en revanche interroger les **réponses** à ce fait (que faire face au changement climatique) et non le fait.
- **C — B + interdiction du format « pour ou contre » pour toute réalité factuelle**, contrôlée par une vérification dédiée du deuxième modèle.

*Reco :* **B, formulée comme une distinction fait / arbitrage et inscrite en tête de charte** — c'est la frontière qui sépare un outil d'introspection d'une machine à légitimer.

---

### Q10 — Traite-t-on les conflits armés en cours ? **[STRUCTURANT]**

*Enjeu :* un vote quotidien sur un conflit où des gens meurent transforme des victimes en objet de jugement majoritaire, et l'application en cible de campagnes coordonnées.

*Options :*
- **A — Oui, comme tout autre sujet international.** Attendez-vous à des opérations d'influence organisées dès la première occurrence, et à des captures d'écran hors contexte.
- **B — Non, exclusion totale des conflits armés en cours.** Ampute le volet international d'une partie de l'actualité, mais retire le risque le plus lourd.
- **C — Uniquement les décisions publiques *françaises* liées au conflit** (aide, accueil, sanctions, budget militaire), jamais la légitimité des belligérants ni la qualification des faits.

*Reco :* **C.** Elle préserve le champ international réel — les arbitrages que la France doit trancher — sans demander à personne de noter un conflit sur cinq mentions.

---

### Q11 — Qui, ou quoi, choisit *la* question parmi les sujets saillants du jour ? **[BLOQUANT]**

*Enjeu :* le pouvoir éditorial d'Endoxa n'est pas dans la formulation, il est dans le choix quotidien : une question par jour, c'est un agenda, et un agenda est une ligne éditoriale.

*Options :*
- **A — Le sujet le plus saillant dans les flux.** Délègue l'agenda au volume de production de la presse, donc aux sujets les plus polarisants et les plus repris.
- **B — Tirage aléatoire pondéré parmi les sujets éligibles**, avec équilibrage sur des thématiques prédéfinies (économie, environnement, institutions, société, international) sur une fenêtre glissante de 30 jours.
- **C — Sélection humaine quotidienne parmi les 3 à 5 candidats produits par le pipeline.** Restaure la responsabilité éditoriale et l'exception de l'article 50(4), mais crée un poste à tenir 365 jours par an.

*Reco :* **B, avec la répartition thématique publiée et auditable *a posteriori*.** L'aléatoire encadré est plus défendable qu'un choix humain non justifié et bien plus défendable que le pur volume de presse.

---

### Q12 — Peut-on nommer une personne ? **[STRUCTURANT]**

*Enjeu :* diffamation, présomption d'innocence, et transformation d'un outil d'introspection en machine à noter des individus.

*Options :*
- **A — Oui pour les responsables politiques en exercice, sur leurs actes.** Frontière impraticable à tenir automatiquement : « la politique de X » glisse en « X » en une reformulation.
- **B — Aucune personne physique nommée dans la question ni dans les propositions.** Les fonctions et les institutions restent nommables (le gouvernement, le Parlement, la Commission européenne).
- **C — B + interdiction également dans les justifications et dans les arguments de camp**, où la mention se réintroduit le plus facilement.

*Reco :* **C.** Le format « juger 4 propositions sur cinq mentions » appliqué à un individu nommé est structurellement diffamatoire, quelle que soit la prudence des formulations.

---

# III. La formulation et la mesure du biais

### Q13 — Qu'appelle-t-on exactement un « biais de formulation », en termes mesurables ? **[BLOQUANT]**

*Enjeu :* tant que le biais n'est pas défini par une grandeur observable, aucune des parades annoncées n'est vérifiable — ni la seconde IA, ni la notation des utilisateurs.

*Options :*
- **A — Définition qualitative** (« la question ne doit pas être orientée »). Non testable ; deux relecteurs de bords opposés la déclareront respectivement satisfaite et violée.
- **B — Définition par l'effet** : une formulation est biaisée si une paraphrase sémantiquement équivalente déplace la distribution des mentions au-delà d'un seuil ε. Mesurable, mais ne se mesure qu'après diffusion sur des humains.
- **C — Définition par l'effet + proxy pré-publication** : distribution des jugements simulés par un ensemble de modèles distincts, sur N paraphrases et N ordres de présentation. Mesurable avant publication ; validité limitée, mais reproductible.
- **D — B + C, avec C comme filtre bloquant et B comme mesure de contrôle *a posteriori***, publiée.

*Reco :* **D.** Sans grandeur chiffrée, « neutralité » reste une intention ; avec elle, elle devient un seuil que le pipeline franchit ou non.

---

### Q14 — Quel protocole de test avant publication, et qu'est-ce qui le fait échouer ? **[BLOQUANT]**

*Enjeu :* la littérature est claire — les réponses des modèles à des questions à choix multiples ne sont pas robustes à la paraphrase ni au format de contrainte (Röttger et al., ACL 2024), et varient fortement selon l'ordre des options.

*Options :*
- **A — Une passe de relecture par un second modèle.** C'est le cadrage actuel : un seul appel, pas de mesure, pas de seuil, pas de trace.
- **B — Batterie systématique** : 5 paraphrases de la question × 3 ordres des propositions × 3 modèles de familles différentes ; on mesure la dispersion de la proposition gagnante et la variance des mentions.
- **C — B + test de retournement** : on réécrit la question en inversant la charge (formulation « pour » / formulation « contre ») et on vérifie que le gagnant ne change pas. Si le gagnant change, la question est rejetée, pas corrigée.
- **D — C + jeu de questions-témoins** dont la réponse attendue est connue, rejouées quotidiennement pour détecter une dérive du pipeline.

*Reco :* **C, avec D dès le deuxième mois.** Un rejet non négociable vaut mieux qu'une correction : une question qui bascule sous retournement est une question qui n'aurait pas dû exister.

---

### Q15 — La seconde IA de contrôle est-elle indépendante de la première ? **[BLOQUANT]**

*Enjeu :* si le générateur et le contrôleur sont le même modèle, ou deux modèles de la même famille, leurs erreurs sont corrélées et le contrôle valide précisément les biais qu'il est censé détecter.

*Options :*
- **A — Même modèle, prompt différent.** Le contrôle est décoratif : il approuvera systématiquement les biais partagés par le modèle, qui sont exactement ceux qui comptent.
- **B — Modèle d'un autre fournisseur, prompt de contrôle écrit indépendamment**, sans accès aux consignes de génération.
- **C — B + décision par vote de trois modèles de familles distinctes**, publication seulement à l'unanimité, sinon rejet.
- **D — C + audit humain mensuel** sur un échantillon aléatoire de 30 questions publiées, par deux relecteurs de sensibilités déclarées différentes, avec taux d'accord publié.

*Reco :* **C + D.** C'est le seul contrôle *a priori* du dispositif : s'il n'est pas redondant et hétérogène, il n'existe pas.

---

### Q16 — Qui arbitre quand les contrôles se contredisent ? **[BLOQUANT]**

*Enjeu :* un désaccord entre modèles, ou entre modèles et utilisateurs, doit avoir une issue prévue à l'avance, faute de quoi elle sera improvisée sous pression.

*Options :*
- **A — Le porteur du projet, au cas par cas.** Chaque arbitrage devient contestable comme un choix personnel, et le sera.
- **B — Règle mécanique : tout désaccord = rejet.** Simple, non contestable, coûteux en taux de rejet.
- **C — Comité de relecture externe de 3 à 5 personnes de sensibilités déclarées**, saisi sur les cas litigieux, décisions publiées avec motivation.
- **D — B en régime courant, C pour les décisions de doctrine** (modification de la charte, de la liste d'exclusion, du corpus de sources).

*Reco :* **D.** L'arbitrage quotidien doit être mécanique pour être crédible ; l'arbitrage sur les règles doit être collégial pour être légitime.

---

### Q17 — Que se passe-t-il quand aucune question ne passe les contrôles ? **[STRUCTURANT]**

*Enjeu :* le cadrage pose qu'« il ne doit jamais y avoir un jour sans question » ; cette contrainte est exactement celle qui fera baisser les seuils un jour de tension.

*Options :*
- **A — Réserve de questions intemporelles pré-générées.** C'est le cadrage actuel : la réserve n'a pas subi les mêmes contrôles et devient la porte de sortie systématique.
- **B — Réserve soumise aux mêmes contrôles**, constituée à froid, marquée comme telle dans l'interface et exclue des statistiques du jour.
- **C — Assumer le jour sans question**, avec un écran explicite : « aucune question n'a passé nos contrôles d'équité aujourd'hui ».

*Reco :* **B avec l'option C conservée**, et le taux de recours à la réserve publié mensuellement — le jour où il dépasse 30 %, c'est le pipeline qu'il faut revoir, pas la réserve qu'il faut agrandir. Un dispositif qui n'a jamais le droit de se taire finit toujours par publier n'importe quoi.

---

### Q18 — Le vecteur de valeurs Schwartz attaché à chaque proposition est-il un jugement politique ? **[STRUCTURANT]**

*Enjeu :* décider qu'une proposition « pèse +0,8 en sécurité et −0,5 en autonomie » est une opération interprétative lourde, générée par la même IA que le contenu, et elle détermine le miroir que l'utilisateur reçoit.

*Options :*
- **A — Scoring caché.** Le cadrage l'exclut déjà à juste titre.
- **B — Scoring consultable à la demande.** C'est le cadrage actuel : nécessaire, insuffisant — visible ne veut pas dire validé.
- **C — B + validation du scoring par accord inter-modèles** (mêmes exigences qu'en Q15), et publication du taux d'accord.
- **D — C + calibration humaine périodique** sur un échantillon, avec mesure de l'accord entre annotateurs humains et modèle.

*Reco :* **C, D avant toute ouverture publique du radar entre membres d'un groupe.** Un profil de valeurs erroné affiché à sa famille est un dommage personnel, pas une imprécision statistique.

---

# IV. L'équilibre des quatre propositions

### Q19 — Comment démontre-t-on que les 4 propositions couvrent l'espace des positions défendables ? **[BLOQUANT]**

*Enjeu :* quatre options ne sont pas un espace, c'est un découpage, et le découpage est le choix politique le plus lourd du dispositif.

*Options :*
- **A — Confiance dans la génération.** L'espace couvert sera celui que le modèle juge raisonnable, c'est-à-dire centré sur la position médiane de son corpus d'entraînement.
- **B — Ancrage externe** : les propositions doivent être adossées à des positions effectivement soutenues dans le corpus de presse du jour, avec citation de la source qui les porte.
- **C — B + grille de couverture** : sur chaque sujet, vérifier que les positions présentes dans le débat public français (mesurées par exemple sur les positions des groupes parlementaires) trouvent chacune un représentant parmi les quatre, ou que leur absence est motivée.
- **D — C + affichage assumé du reste** : « les positions suivantes existent dans le débat et ne sont pas proposées aujourd'hui ».

*Reco :* **B + D.** L'ancrage rend l'espace vérifiable, l'affichage du reste transforme l'incomplétude inévitable en transparence plutôt qu'en escamotage.

---

### Q20 — Comment détecte-t-on une proposition-épouvantail ? **[BLOQUANT]**

*Enjeu :* le mécanisme d'influence le plus efficace dans ce format n'est pas la question orientée, c'est la proposition adverse rédigée pour être rejetée — et il est presque invisible en relecture.

*Options :*
- **A — Consigne dans le prompt (« ne caricature pas »).** Sans effet mesurable.
- **B — Test de l'avocat** : pour chaque proposition, demander à un modèle *distinct* de la défendre avec conviction ; si elle est indéfendable en une phrase honnête, elle est une caricature et la question est rejetée.
- **C — B + contrainte de symétrie formelle** mesurée : longueur en mots (± 15 %), niveau de généralité, présence de modalisateurs (« sans doute », « éventuellement »), présence de termes affectivement chargés — mesurés, pas jugés.
- **D — C + test de plausibilité de rejet** : si une proposition obtient « à rejeter » de plus de 80 % des votants pendant plusieurs occurrences, ouvrir une revue du générateur.

*Reco :* **B + C en bloquant, D en surveillance continue.** Le test de l'avocat est le contrôle le plus efficace du document pour le coût le plus faible.

---

### Q21 — Propose-t-on les positions extrêmes ? **[BLOQUANT]**

*Enjeu :* les exclure fabrique un centre artificiel et donne raison à ceux qui accusent le dispositif de censurer ; les inclure les légitime et expose à la reprise d'une position minoritaire présentée comme gagnante.

*Options :*
- **A — Fenêtre « raisonnable » implicite.** C'est ce qui se produira par défaut, et l'accusation de biais centriste sera fondée.
- **B — Critère de seuil de représentation** : une position est proposable si elle est portée par un acteur institutionnel disposant d'une représentation mesurable (groupe parlementaire, organisation syndicale ou professionnelle représentative, exécutif). Critère externe, vérifiable, non idéologique.
- **C — B assorti d'une limite dure** : jamais de position dont la mise en œuvre constituerait une infraction pénale ou une atteinte aux droits fondamentaux, indépendamment de son niveau de soutien.
- **D — Aucune position hors du consensus.** Confortable, mais rend le produit inutile : un miroir de valeurs qui n'offre que des options moyennes ne reflète rien.

*Reco :* **B + C, écrits et publics.** C'est le seul couple qui permette de répondre « voici le critère » plutôt que « voici mon jugement » quand la question sera posée — et elle le sera.

---

### Q22 — Dans quel ordre s'affichent les quatre propositions ? **[STRUCTURANT]**

*Enjeu :* l'ordre de présentation modifie les réponses — c'est documenté chez les humains (effets d'ordre en méthodologie d'enquête) comme chez les modèles (biais de position et biais de jeton sur les identifiants d'option).

*Options :*
- **A — Ordre de génération.** Le modèle place les options selon ses propres priors ; le biais du générateur se transmet directement à l'électeur.
- **B — Ordre aléatoire par utilisateur.** Neutralise l'effet en moyenne sur l'agrégat, mais rend les comparaisons individuelles moins lisibles.
- **C — B + mesure** : conserver l'ordre affiché dans la table `Vote` et publier l'effet d'ordre mesuré. Coût nul, valeur méthodologique forte.

*Reco :* **C.** La randomisation est la seule protection ; enregistrer l'ordre transforme un biais subi en donnée exploitable et fait d'Endoxa un dispositif honnête sur ses propres effets.

---

### Q23 — Le choix des cinq mentions du jugement majoritaire est-il neutre ? **[DÉTAIL]**

*Enjeu :* Balinski et Laraki soulignent que l'échelle de mentions doit être verbale, commune à tous et calibrée ; « À rejeter · Insuffisant · Passable · Bien · Très bien » est une échelle scolaire française dont le centre de gravité perçu n'est pas au milieu.

*Options :*
- **A — Conserver l'échelle scolaire.** Familière, mais « Passable » est perçu négativement en France, ce qui décale l'échelle vers le bas et déplace les médianes.
- **B — Échelle symétrique explicite** autour d'un point neutre nommé.
- **C — Test A/B sur deux échelles pendant la phase fermée**, avec mesure du déplacement des mentions majoritaires.

*Reco :* **C puis B.** L'échelle est un instrument de mesure : elle se calibre une fois, avant la mise en ligne, pas après.

---

# V. Le dossier factuel

### Q24 — D'où viennent réellement les chiffres ? **[BLOQUANT]**

*Enjeu :* le cadrage prévoit d'agréger des **flux RSS de titres de presse** ; un flux RSS contient des titres et des chapeaux, pas des données — les « trois chiffres sourcés » ne peuvent donc pas provenir du corpus déclaré.

*Options :*
- **A — Le modèle produit les chiffres de mémoire et attribue une source plausible.** C'est ce qui se produira par construction si rien n'est prévu : fabrication de chiffres avec attribution — la faute éditoriale la plus grave possible, et la plus facile à démontrer publiquement.
- **B — Base de données de sources primaires** (INSEE, Eurostat, DREES, ministères, Banque de France, organismes internationaux), interrogée par le pipeline ; aucun chiffre n'est publié s'il ne provient pas d'un enregistrement de cette base.
- **C — B + récupération du texte intégral des articles cités**, et non des seuls titres.
- **D — Supprimer les chiffres** et ne conserver que le contexte qualitatif.

*Reco :* **B, obligatoire, avec C comme complément.** C'est probablement la faille la plus concrète du cadrage actuel : elle rend le dossier factuel non implémentable en l'état.

---

### Q25 — Comment vérifie-t-on qu'un chiffre figure bien dans la source citée ? **[BLOQUANT]**

*Enjeu :* le cadrage confie cette vérification à la seconde IA, qui n'a aucun moyen de vérifier quoi que ce soit si elle n'accède pas au document source.

*Options :*
- **A — Vérification par le modèle, sans accès au document.** Le modèle jugera la plausibilité, pas la véracité, et l'appellera vérification.
- **B — Vérification par correspondance textuelle** : le chiffre doit apparaître littéralement dans le document récupéré, avec citation de l'extrait et de l'URL exacte, ancrée si possible.
- **C — B + affichage de l'extrait source** au clic sur le chiffre, dans l'application.

*Reco :* **C.** Un chiffre non vérifiable mécaniquement ne doit pas être publié ; afficher l'extrait est à la fois le contrôle et la preuve du contrôle.

---

### Q26 — Que fait-on quand deux sources fiables se contredisent ? **[STRUCTURANT]**

*Enjeu :* c'est le cas normal, pas l'exception (chômage, immigration, délinquance, émissions), et le choix du chiffre est un acte éditorial majeur.

*Options :*
- **A — Retenir le plus repris dans la presse.** Le nombre de reprises mesure la viralité, pas la justesse.
- **B — Retenir la source primaire officielle.** Simple, mais fait d'Endoxa le porte-parole d'une statistique publique parfois elle-même contestée.
- **C — Afficher les deux chiffres avec leur périmètre** (« X selon A, périmètre P1 ; Y selon B, périmètre P2 »). Plus long à lire, mais c'est exactement le matériau que le produit prétend fournir pour distinguer ce qu'on sait de ce qu'on ressent.
- **D — Rejeter le sujet.** Élimine mécaniquement les sujets les plus intéressants.

*Reco :* **C.** Le désaccord entre sources est une information de premier ordre pour un produit d'introspection épistémique — le masquer trahirait sa promesse.

---

### Q27 — Que fait-on d'un chiffre exact mais contesté ? **[STRUCTURANT]**

*Enjeu :* un chiffre peut être correctement mesuré et néanmoins constituer un argument, par le périmètre choisi, la période retenue ou l'unité employée.

*Options :*
- **A — Publier le chiffre seul.** Le cadrage devient un argument déguisé en fait.
- **B — Publier chiffre + périmètre + date + producteur** systématiquement, sans exception.
- **C — B + mention explicite de la controverse** quand elle est documentée dans les sources.
- **D — Interdire les chiffres dont le périmètre est contesté.** Élimine la majorité des indicateurs socialement intéressants.

*Reco :* **C.** Le périmètre affiché fait plus contre la manipulation par les chiffres que n'importe quel filtre.

---

### Q28 — « L'argument principal de chaque camp » : combien de camps, et qui les définit ? **[BLOQUANT]**

*Enjeu :* la formule « chaque camp » du cadrage présuppose deux camps, alors que le vote en propose quatre — l'incohérence structurelle réduit un espace à quatre positions à un affrontement binaire juste avant le vote.

*Options :*
- **A — Deux camps, pour et contre.** Contredit le format, réintroduit la binarité que le jugement majoritaire est censé dépasser, et cadre le vote juste avant qu'il ait lieu.
- **B — Un argument par proposition** (quatre arguments), avec les mêmes contraintes de symétrie qu'en Q20.
- **C — B + attribution** : chaque argument est attribué à l'acteur ou au titre qui le porte, jamais présenté comme une voix neutre.

*Reco :* **C.** C'est aussi le seul moyen d'éviter que l'IA ne parle en son nom propre au moment le plus déterminant du parcours.

---

# VI. Les justifications proposées

### Q29 — Comment évite-t-on de mettre des mots dans la bouche des gens ? **[BLOQUANT]**

*Enjeu :* le cadrage supprime la saisie libre pour éviter la modération ; la conséquence est que l'utilisateur ne peut exprimer que ce que l'IA a prévu, et le profil de valeurs est construit sur ces mots-là.

*Options :*
- **A — Quatre justifications, choix obligatoire.** L'utilisateur est contraint d'endosser une formulation qui n'est pas la sienne ; le radar mesure alors les catégories de l'IA, pas ses valeurs.
- **B — Quatre justifications + « aucune de ces raisons »**, qui n'alimente pas le profil mais est comptabilisée et publiée.
- **C — B + « je préfère ne pas répondre »**, distinct du précédent.
- **D — B + suivi du taux de « aucune »** par question, avec seuil au-delà duquel le jeu de justifications est considéré comme raté.

*Reco :* **B + D.** Le taux de « aucune de ces raisons » est la meilleure métrique de qualité éditoriale dont ce produit puisse disposer — et elle est gratuite.

---

### Q30 — Comment garantit-on que les justifications ne sont pas plus flatteuses d'un côté ? **[BLOQUANT]**

*Enjeu :* une justification peut être noble d'un côté (« par souci de justice ») et mesquine de l'autre (« parce que ça me coûterait cher ») ; l'asymétrie de désirabilité sociale déplace les réponses sans qu'aucune question ne soit orientée.

*Options :*
- **A — Consigne au modèle.** Non mesurable.
- **B — Contrainte structurelle** : chaque justification doit invoquer une valeur positive de Schwartz nommée, jamais un intérêt personnel ni une émotion négative. Vérifié mécaniquement contre le vecteur de valeurs déjà produit.
- **C — B + test de permutation** : les justifications sont soumises à un modèle tiers qui doit les classer par désirabilité perçue ; un écart trop important entraîne régénération.

*Reco :* **B + C.** La désirabilité sociale est le biais le plus fort et le moins visible de tout questionnaire d'opinion ; c'est le seul point du dispositif où un test automatisé mesure vraiment quelque chose d'utile.

---

### Q31 — La question « d'où vient ta conviction ? » est-elle neutre entre ses cinq modalités ? **[DÉTAIL]**

*Enjeu :* le cadrage affirme qu'aucune source n'est présentée comme supérieure ; l'ordre d'affichage « Des données · Un ressenti · Mon expérience vécue · Un principe moral · La confiance en quelqu'un » place les données en premier et la confiance en dernier, ce qui est déjà une hiérarchie.

*Options :*
- **A — Ordre fixe.** Hiérarchie implicite, contraire à la promesse.
- **B — Ordre aléatoire par utilisateur**, avec l'ordre enregistré.
- **C — B + reformulation des libellés** pour égaliser la charge (« un ressenti » est plus faible que « des données » dans le registre courant).

*Reco :* **C.** C'est le point du parcours où le produit prétend le plus explicitement à la neutralité ; l'échec y serait le plus visible.

---

# VII. Le corpus de presse

### Q32 — Quels titres, selon quels critères écrits ? **[BLOQUANT]**

*Enjeu :* le choix des sources **est** la ligne éditoriale ; tout le reste du dispositif en découle mécaniquement.

*Options :*
- **A — « Sensibilités variées », choisi au jugé.** Non défendable publiquement, et la première accusation de partialité portera là.
- **B — Critères écrits** : diffusion ou audience mesurée (ACPM), adhésion à une charte déontologique, existence d'un directeur de publication identifié, absence de condamnation récente pour manquement grave — et couverture explicite de l'éventail des sensibilités reconnues.
- **C — B + parité structurelle** : nombre égal de titres dans chaque grande famille éditoriale, avec la classification retenue publiée et sourcée.
- **D — C + agences de presse (AFP, Reuters) comme socle factuel**, distinct des titres d'opinion.

*Reco :* **B + D, C si la classification retenue est empruntée à un travail académique existant et citée** — classer soi-même les journaux, c'est prendre parti ; emprunter une classification établie, c'est se donner un contradicteur.

---

### Q33 — Qui peut contester le corpus, et par quelle procédure ? **[STRUCTURANT]**

*Enjeu :* un corpus non contestable est un corpus arbitraire, quelle qu'en soit la qualité.

*Options :*
- **A — Personne, décision du porteur.** Chaque modification alimentera le soupçon.
- **B — Liste publique + adresse de contestation + réponse motivée publiée.**
- **C — B + révision annuelle par le comité de relecture** (Q16), avec journal des entrées et sorties du corpus et motifs.

*Reco :* **C.** Le journal des modifications du corpus est le document que réclamera le premier journaliste qui s'intéressera au projet.

---

### Q34 — Que fait-on du biais d'agenda du corpus — ce dont la presse ne parle pas ? **[STRUCTURANT]**

*Enjeu :* un pipeline qui sélectionne les sujets présents dans plusieurs sources reproduit fidèlement les angles morts collectifs de la presse française, et les présente comme l'actualité.

*Options :*
- **A — Ne rien faire.** Endoxa devient un amplificateur de l'agenda médiatique, avec une couche de légitimité méthodologique.
- **B — Quota de sujets « peu couverts »** (une question par semaine issue d'un sujet peu repris mais éligible).
- **C — B + sources non journalistiques** en veille (rapports d'institutions, avis d'autorités indépendantes, textes en discussion au Parlement) pour l'identification des sujets.

*Reco :* **C.** C'est aussi ce qui différencie le produit : les arbitrages publics réels sont largement traités hors des cycles d'actualité chaude.

---

### Q35 — Le corpus est-il stable, ou changeant selon les sujets ? **[DÉTAIL]**

*Enjeu :* adapter le corpus au sujet du jour permet toutes les manipulations sans jamais toucher à une formulation.

*Options :*
- **A — Corpus adaptatif.** Ingérable et indéfendable.
- **B — Corpus fixe, modifiable seulement par décision datée et publiée**, jamais dans les 24 heures précédant une publication.

*Reco :* **B.** Un corpus qui change le jour même est un corpus choisi pour le résultat.

---

# VIII. Le contrôle par les utilisateurs

### Q36 — À quel seuil une question est-elle retirée ? **[BLOQUANT]**

*Enjeu :* le seuil est le paramètre le plus attaquable du dispositif : trop bas, il donne un droit de veto à un groupe organisé ; trop haut, le contrôle n'existe pas.

*Options :*
- **A — Seuil en valeur absolue de signalements.** Trivial à atteindre par coordination : quelques centaines de comptes suffisent.
- **B — Seuil en proportion des votants**, avec un plancher de volume — par exemple 15 % de signalements sur au moins 1 000 votants.
- **C — B + condition de dispersion** : le retrait n'est déclenché que si les signalements proviennent de profils de valeurs **hétérogènes**. Une question signalée uniquement par des utilisateurs situés du même côté du radar n'est pas retirée ; elle est mise en revue.
- **D — Aucun retrait automatique, uniquement une mise en revue humaine.**

*Reco :* **C.** C'est la seule règle du document qui neutralise structurellement le brigading, et elle est directement implémentable puisque le profil de valeurs existe déjà.

---

### Q37 — Comment protège-t-on le signalement contre la coordination ? **[BLOQUANT]**

*Enjeu :* le jour où l'application sera visible, un appel à signaler une question circulera — c'est une certitude, pas un risque.

*Options :*
- **A — Un signalement par compte.** Insuffisant : la création de comptes est libre.
- **B — Signalement réservé aux comptes ayant voté ce jour-là et actifs depuis N jours**, avec anti-abus classique.
- **C — B + détection de rafales** (signalements concentrés dans le temps, provenant d'un canal de référence identique) → suspension du compteur et bascule en revue humaine, avec incident publié.
- **D — C + publication systématique de la courbe de signalements** pour chaque question retirée.

*Reco :* **C + D.** Publier la courbe rend la manipulation visible et donc coûteuse pour celui qui la tente.

---

### Q38 — Que signifie « retirée » pour les votes déjà émis ? **[STRUCTURANT]**

*Enjeu :* le cadrage prévoit le retrait et la publicité du retrait, mais rien sur le sort des jugements déjà exprimés ni sur les profils de valeurs qu'ils ont déplacés.

*Options :*
- **A — Retrait de l'affichage, conservation des effets.** Le profil de valeurs de chacun reste altéré par une question reconnue biaisée.
- **B — Retrait + annulation des contributions au radar**, votes conservés en archive marquée.
- **C — B + notification aux votants** expliquant le retrait et son motif.
- **D — Suppression totale.** Interdit toute vérification ultérieure et détruit la trace de l'erreur.

*Reco :* **C.** Le produit valorise le changement d'avis ; il doit s'appliquer à lui-même la même règle, visiblement.

---

### Q39 — La note d'équité est-elle publique, et à quel moment est-elle demandée ? **[DÉTAIL]**

*Enjeu :* le cadrage la place à l'étape 8, après l'affichage des résultats — c'est-à-dire quand l'utilisateur sait s'il a gagné ou perdu, ce qui contamine mécaniquement le jugement d'équité.

*Options :*
- **A — Après les résultats.** Mesure la satisfaction du résultat, pas l'équité de la question.
- **B — Avant le vote**, juste après lecture de la question et des propositions.
- **C — Avant *et* après**, avec publication de l'écart — qui est lui-même une mesure de biais intéressante.

*Reco :* **C.** L'écart entre équité perçue avant et après résultat est probablement l'indicateur le plus original que ce produit puisse produire.

---

# IX. Périodes sensibles

### Q40 — Que fait-on en période électorale ? **[BLOQUANT]**

*Enjeu :* même hors du champ de la loi de 1977, publier des résultats agrégés de jugements politiques la veille d'un scrutin est intenable ; la loi interdit publication, diffusion et commentaire de tout sondage électoral à partir du samedi zéro heure précédant le scrutin et jusqu'à la fermeture du dernier bureau de vote.

*Options :*
- **A — Rien de spécial.** Une capture d'écran d'Endoxa circulant le samedi soir avant un second tour est un incident de niveau national.
- **B — Suspension totale du dispositif** du samedi 0 h à la fermeture des bureaux, pour tout scrutin national.
- **C — B + basculement sur des sujets non électoraux** pendant les 30 jours précédant un scrutin national (l'actualité électorale reste inéligible, le produit continue de tourner).
- **D — C + gel de l'affichage des résultats agrégés** pendant cette même période : on vote, on voit son propre radar, on ne voit pas la moyenne nationale.

*Reco :* **C + D, écrit dans la charte avant le lancement, avec le calendrier électoral programmé à l'avance.** Une règle décidée pendant la campagne n'aura aucune crédibilité, quelle qu'elle soit.

---

### Q41 — Que fait-on en cas d'attentat, de catastrophe ou de crise majeure ? **[BLOQUANT]**

*Enjeu :* le pipeline automatique produira une question sur l'événement dans les heures qui suivent, parce que c'est le sujet le plus saillant de tous les flux — et ce sera indéfendable.

*Options :*
- **A — Le pipeline tourne.** Garantie de publier, dans les 24 heures suivant un attentat, quatre propositions à noter sur cinq mentions.
- **B — Détection automatique de crise** (pic de convergence des flux sur un événement à victimes) → bascule immédiate sur la réserve intemporelle, pour une durée minimale écrite (72 h par exemple).
- **C — B + interrupteur manuel accessible en permanence**, et procédure de reprise écrite : qui décide, sur quel critère, avec quelle publication.

*Reco :* **C.** La détection automatique gère la nuit et le week-end ; l'interrupteur manuel gère tout le reste. Les deux sont nécessaires.

---

### Q42 — Qui a autorité pour suspendre, en combien de temps, et comment redémarre-t-on ? **[STRUCTURANT]**

*Enjeu :* une règle de suspension sans titulaire nommé ni délai n'est pas une règle.

*Options :*
- **A — Le porteur, quand il le voit.** Défaillant la nuit, en vacances, en cas d'indisponibilité.
- **B — Deux personnes nommées, astreinte alternée, délai cible d'une heure**, procédure écrite de reprise avec motif publié.
- **C — B + suspension automatique par défaut** en cas d'indisponibilité des deux (le dispositif s'arrête plutôt que de publier sans surveillance).

*Reco :* **C.** Le comportement par défaut d'un système automatique en cas de perte de supervision doit être l'arrêt, jamais la poursuite.

---

# X. Résultats, transparence, récupération, correction

### Q43 — Qu'affiche-t-on obligatoirement à côté de tout résultat agrégé ? **[BLOQUANT]**

*Enjeu :* un pourcentage affiché sans qualification sera lu comme représentatif, quelle que soit la prudence du texte d'accompagnement ; et il circulera séparé de son contexte.

*Options :*
- **A — Le résultat seul.** Il deviendra « X % des Français » dans les 48 heures suivant la première reprise.
- **B — Résultat + effectif + mention de non-représentativité** en clair (« participants volontaires, non représentatifs de la population française »).
- **C — B + incrustation de cette mention dans l'image de partage elle-même**, non supprimable, avec la date et le libellé intégral de la question.
- **D — C + refus d'afficher un résultat sous un effectif plancher.**

*Reco :* **C + D.** Le seul avertissement qui compte est celui qui voyage avec le chiffre ; tous les autres restent dans l'application.

---

### Q44 — Que publie-t-on du fonctionnement, et sous quelle forme ? **[STRUCTURANT]**

*Enjeu :* le cadrage promet des prompts publics ; c'est nécessaire mais ce n'est pas ce qui permet de vérifier quoi que ce soit — un prompt ne dit pas ce qui a été produit.

*Options :*
- **A — Prompts publiés.** Utile symboliquement, faible en vérifiabilité.
- **B — A + registre public quotidien** : question intégrale, propositions, sources et extraits des chiffres, modèles et versions utilisés, résultats des contrôles, effectif, distribution des mentions.
- **C — B + statistiques mensuelles** : taux de rejet au contrôle, taux de recours à la réserve, taux de retrait après signalement, taux de « aucune de ces raisons », écart d'équité avant/après résultat, liste des sujets écartés avec motifs.
- **D — C + archive téléchargeable**, horodatée et versionnée, permettant à un tiers de refaire l'analyse.

*Reco :* **C, D dès qu'un chercheur ou un journaliste le demande.** Publier les taux d'échec est ce qui distingue la transparence de la communication : un dispositif qui ne publie que ses succès n'est pas transparent, il est promotionnel.

---

### Q45 — Que fait-on le jour où un responsable politique ou un média cite un résultat d'Endoxa ? **[BLOQUANT]**

*Enjeu :* cela arrivera, probablement à l'occasion d'un résultat commode pour celui qui le cite, et l'absence de réaction vaudra approbation.

*Options :*
- **A — Rien, on ne contrôle pas les reprises.** Le silence sera lu comme un accord, et le chiffre s'installera.
- **B — Communiqué type pré-rédigé**, publié sous 24 h, rappelant l'effectif, la non-représentativité, le libellé intégral et la méthode — quel que soit le camp qui cite.
- **C — B + page publique permanente « reprises »**, listant chaque citation constatée et la mise au point associée, sans exception ni sélection.
- **D — C + règle de symétrie écrite** : la mise au point est systématique et identique, y compris quand la reprise est flatteuse pour le projet.

*Reco :* **D.** La symétrie est tout : une mise au point émise seulement quand la reprise dérange est elle-même un acte partisan, et sera relevée comme tel.

---

### Q46 — Que fait-on d'une question déjà votée qui se révèle biaisée ou fausse ? **[STRUCTURANT]**

*Enjeu :* c'est la situation qui survient toujours, et c'est celle qui n'est jamais prévue.

*Options :*
- **A — Suppression discrète.** Détruit la confiance dès la première capture d'écran comparative, et supprime la preuve.
- **B — Correction visible** : la question reste en archive, marquée « corrigée » ou « retirée », avec la nature de l'erreur, la date de constat et la décision prise. Le résultat reste consultable mais est exclu des statistiques agrégées.
- **C — B + notification aux votants + retrait des contributions au radar** (cohérent avec Q38).
- **D — B + rejeu proposé** : la question corrigée est reproposée en archive, et l'écart entre les deux distributions est publié.

*Reco :* **C, et D quand l'erreur portait sur un chiffre.** L'écart entre avant et après correction est la démonstration empirique la plus forte que ce produit puisse offrir sur le pouvoir de la formulation — et la meilleure preuve de sa bonne foi.

---

# Charte minimale

*Les règles à écrire, publier et dater **avant** la première question mise en ligne. Elles doivent être versionnées publiquement : toute modification datée, motivée, avec conservation des versions antérieures.*

**1. Nature du dispositif.** Endoxa n'est pas un sondage. Les participants sont volontaires et auto-sélectionnés. Aucun résultat n'est représentatif de la population française. Les termes « sondage », « les Français », « l'opinion publique » ne sont employés nulle part dans le produit ni dans sa communication.

**2. Origine du contenu.** Question, propositions, chiffres, arguments et justifications sont produits par des systèmes d'intelligence artificielle. Le mode de supervision humaine effectivement en place est indiqué sur l'écran de la question du jour, en termes littéraux.

**3. Responsabilité.** Une personne physique nommée est directeur de la publication. Elle dispose d'un moyen de dépublication immédiate et d'une astreinte écrite.

**4. Ce qu'Endoxa ne traite jamais.** Faits divers. Personnes physiques nommées. Affaires judiciaires en cours. Caractéristiques des personnes (origine, religion, orientation sexuelle, handicap, identité de genre). Victimes identifiables. Santé individuelle. Légitimité des belligérants d'un conflit armé. Existence de faits établis par consensus scientifique ou décision de justice. Toute position dont la mise en œuvre constituerait une infraction pénale.

**5. Ce qu'Endoxa traite.** Des décisions publiques et des arbitrages collectifs, portant sur au moins trois positions défendables, effectivement soutenues dans le débat public, et dont le désaccord porte sur des valeurs et non sur des faits.

**6. Critère de proposabilité.** Une position est proposable si elle est portée par un acteur disposant d'une représentation mesurable. Ce critère est externe, écrit et opposable. Les positions du débat non retenues un jour donné sont mentionnées.

**7. Contrôles avant publication.** Toute question franchit : le test de retournement (inversion de charge — si le gagnant change, rejet, sans correction), le test de l'avocat (chaque proposition doit être défendable honnêtement en une phrase), la vérification textuelle des chiffres contre leur source primaire, et l'accord d'au moins trois modèles de familles distinctes. Un seul désaccord entraîne le rejet.

**8. Provenance des chiffres.** Aucun chiffre n'est publié s'il ne provient d'une source primaire identifiée et si son extrait n'est pas consultable dans l'application. Périmètre, date et producteur sont affichés systématiquement. Les sources contradictoires sont affichées ensemble, jamais arbitrées en silence.

**9. Randomisation.** L'ordre des propositions et l'ordre des modalités de la question épistémique sont aléatoires par utilisateur et enregistrés. Les effets d'ordre mesurés sont publiés.

**10. Sortie garantie.** Une option « aucune de ces raisons » est toujours proposée, ne contribue pas au profil de valeurs, et son taux est publié par question.

**11. Corpus de presse.** La liste des titres et les critères de sélection sont publics. Le corpus ne change jamais dans les 24 heures précédant une publication. Toute modification est datée, motivée et publiée. Un contradicteur peut la contester par une procédure écrite avec réponse publique.

**12. Signalement et retrait.** Le retrait est déclenché par une proportion de signalements sur un effectif plancher, **et** seulement si les signalements proviennent de profils de valeurs hétérogènes. Toute rafale coordonnée suspend le compteur et déclenche une revue humaine. Chaque retrait est publié avec sa courbe de signalements.

**13. Suspension.** Suspension totale du samedi 0 h à la fermeture du dernier bureau de vote pour tout scrutin national. Aucun sujet électoral et aucun affichage de résultat agrégé dans les 30 jours précédant un scrutin national. Bascule immédiate sur contenus intemporels en cas d'événement à victimes, pour 72 heures minimum. En cas de perte de supervision humaine, le dispositif s'arrête.

**14. Affichage des résultats.** Effectif et mention de non-représentativité accompagnent tout résultat, y compris incrustés dans les images de partage, avec la date et le libellé intégral de la question. Aucun résultat sous l'effectif plancher.

**15. Correction.** Toute question erronée ou biaisée est marquée comme telle en archive, jamais supprimée. Ses contributions aux profils de valeurs sont annulées et les votants en sont informés.

**16. Reprises externes.** Toute citation publique d'un résultat d'Endoxa donne lieu à une mise au point publiée sous 24 heures, identique quel que soit l'auteur de la citation et quel que soit le sens du résultat cité. Les reprises et les mises au point sont listées sur une page publique permanente.

**17. Registre public.** Publication quotidienne du contenu intégral et de ses sources ; publication mensuelle des taux de rejet, de recours à la réserve, de retrait, de « aucune de ces raisons », et de la liste des sujets écartés avec leurs motifs.

**18. Données personnelles.** Les opinions politiques sont des données sensibles. Consentement explicite et spécifique, révocable en un geste, hébergement dans l'Union européenne, minimisation, export et suppression natifs, durée de conservation écrite.

---

# Sources vérifiées

**Cadre juridique français — sondages**
- Loi n° 77-808 du 19 juillet 1977 relative à la publication et à la diffusion de certains sondages d'opinion, texte consolidé — https://www.legifrance.gouv.fr/loda/id/JORFTEXT000000522846
- Article 1er de la loi n° 77-808 (définition du sondage, champ « débat électoral », version issue de 2016) — https://www.legifrance.gouv.fr/loda/article_lc/LEGIARTI000032454569
- Article 11 de la loi n° 77-808 (interdiction de publication la veille et le jour du scrutin) — https://www.legifrance.gouv.fr/loda/article_lc/LEGIARTI000032454547
- Loi n° 2016-508 du 25 avril 2016 de modernisation de diverses règles applicables aux élections — https://www.legifrance.gouv.fr/jorf/id/JORFTEXT000032451722
- Commission des sondages — mission et présentation — https://www.commission-des-sondages.fr/presentation/presentation.htm
- Commission des sondages — « Qu'est-ce qu'un sondage d'opinion ? » (définition, position sur les consultations en ligne auto-sélectionnées et sur l'emploi du terme « sondage ») — https://www.commission-des-sondages.fr/competences/competences.htm
- Commission des sondages — jurisprudence — https://www.commission-des-sondages.fr/hist/jurisprudence.htm
- Sénat, rapport « Sondages et démocratie : pour une législation plus respectueuse de la sincérité du débat politique » — https://www.senat.fr/rap/r10-054/r10-054_mono.html
- Sénat, question écrite « Contrôle juridique des sondages électoraux par internet » — https://www.senat.fr/questions/base/2007/qSEQ070326695.html
- CNCCEP, communiqué sur l'interdiction de publication et de diffusion de sondages la veille et le jour des scrutins — https://www.cnccep.fr/pdf-cp8.html
- Conseil constitutionnel, textes de référence — sondages électoraux — https://presidentielle2017.conseil-constitutionnel.fr/tout-savoir/en-resume/textes-de-reference/autres-dispositions-generales/sondages-electoraux-loi-n-77-808-19-juillet-1977/

**Cadre français — manipulation de l'information**
- Loi n° 2018-1202 du 22 décembre 2018 relative à la lutte contre la manipulation de l'information (obligations de transparence des plateformes dans les 3 mois précédant un scrutin national, devoir de coopération, rapport à l'Arcom) — https://www.legifrance.gouv.fr/jorf/id/JORFTEXT000037847559/
- IRSEM, analyse de la loi du 22 décembre 2018 — https://www.irsem.fr/storage/file_manager_files/2025/03/la-loi-du-22-decembre-2018-relative-a-la-lutte-contre-la-manipulation-de-linformation.pdf

**Cadre européen**
- Règlement (UE) sur l'IA, article 50 — obligations de transparence, dont le § 4 sur les textes générés par IA publiés pour informer le public sur des questions d'intérêt public et l'exception de révision humaine / responsabilité éditoriale — https://artificialintelligenceact.eu/article/50/
- Guide pratique de l'article 50 — https://artificialintelligenceact.eu/transparency-rules-article-50/
- Commission européenne, FAQ sur les obligations de transparence de l'article 50 — https://digital-strategy.ec.europa.eu/en/faqs/transparency-obligations-under-article-50-ai-act
- DSA, article 34 (évaluation des risques systémiques, dont les effets négatifs sur le discours civique et les processus électoraux) — https://www.eu-digital-services-act.com/Digital_Services_Act_Article_34.html
- DSA, article 35 (atténuation des risques) — https://www.eu-digital-services-act.com/Digital_Services_Act_Article_35.html
- Commission européenne, lignes directrices sur l'atténuation des risques systémiques pour les processus électoraux — https://digital-strategy.ec.europa.eu/en/library/digital-services-act-summary-report-public-consultation-guidelines-providers-very-large-online

**Données personnelles**
- CNIL, données sensibles et RGPD — https://www.cnil.fr/fr/cnil-direct/question/1823
- RGPD article 9, catégories particulières de données (dont opinions politiques) et conditions d'exception, dont le consentement explicite — https://www.leto.legal/articles-rgpd/article-9

**Mesure du biais des modèles de langage**
- Röttger, Hofmann, Pyatkin, Hinck, Kirk, Schuetze, Hovy, « Political Compass or Spinning Arrow? Towards More Meaningful Evaluations for Values and Opinions in Large Language Models », ACL 2024 — https://aclanthology.org/2024.acl-long.816/ (préprint : https://arxiv.org/abs/2402.16786 ; code : https://github.com/paul-rottger/llm-values-pct)
- « Uncovering Political Bias in Large Language Models using Parliamentary Voting Records » (ancrage sur des votes parlementaires réels plutôt que sur des tests d'orientation) — https://arxiv.org/html/2601.08785v1
- « Benchmarking Gender and Political Bias in Large Language Models » — https://arxiv.org/pdf/2509.06164
- « Large Language Models Sensitivity to the Order of Options in Multiple-Choice Questions » — https://www.researchgate.net/publication/382633315_Large_Language_Models_Sensitivity_to_The_Order_of_Options_in_Multiple-Choice_Questions
- « Quantifying and Mitigating Selection Bias in LLMs » (biais de position et biais de jeton sur les identifiants d'options, méthodes de calibration) — https://arxiv.org/pdf/2511.21709
- « Order-Independence Without Fine Tuning » — https://arxiv.org/pdf/2406.06581

**Méthodologie des questionnaires d'opinion**
- Pew Research Center, « Writing Survey Questions » (effets de formulation, effets d'ordre, alternatives équilibrées) — https://www.pewresearch.org/writing-survey-questions/
- Pew Research Center, « Survey experiments can measure effects of question wording » (méthode du split-ballot) — https://www.pewresearch.org/short-reads/2019/01/29/good-jobs-vs-jobs-survey-experiments-can-measure-the-effects-of-question-wording-and-more/
- Pew Research Center, Methods 101 — Question Wording — https://www.pewresearch.org/methods/2018/03/21/methods-101-video-question-wording/

**Jugement majoritaire**
- Balinski & Laraki, « Jugement majoritaire versus vote majoritaire », Revue française d'économie — https://www.cairn.info/revue-francaise-d-economie-2012-4-page-11.htm
- Balinski & Laraki, présentation au Collège de France (échelle verbale commune, 5 à 7 mentions) — https://www.college-de-france.fr/media/pierre-rosanvallon/UPL8954465031560637643_Balinski__Laraki.pdf
- Page de référence de Rida Laraki sur le jugement majoritaire — https://sites.google.com/site/ridalaraki/majority-judgment-jugement-majoritaire

**Déontologie et IA**
- Reporters sans frontières, Charte de Paris sur l'IA et le journalisme, 10 novembre 2023 (dont : le jugement humain doit rester central dans les décisions éditoriales) — https://rsf.org/sites/default/files/medias/file/2023/11/Charte%20de%20Paris%20sur%20l'IA%20et%20le%20journalisme_1.pdf
- Présentation de la Charte de Paris — https://rsf.org/en/paris-charter-ai-and-journalism

*Note d'accès : legifrance.gouv.fr, commission-des-sondages.fr, artificialintelligenceact.eu et digital-strategy.ec.europa.eu n'ont pas pu être consultés directement depuis cet environnement (blocage réseau). Les contenus correspondants ont été établis à partir de résultats de recherche concordants ; les points juridiques précis — en particulier le calendrier d'application exact des obligations de marquage de l'article 50 du règlement IA et le régime du directeur de la publication en ligne (LCEN art. 6-III, renvoyant à l'article 93-2 de la loi du 29 juillet 1982) — doivent être confirmés sur les textes consolidés avant toute décision engageante.*
