# Questions — UX et parcours utilisateur

> Produit par le rôle **UX & parcours**. Questions, pas solutions.
> Niveaux : **BLOQUANT** (rien ne peut être dessiné sans la réponse),
> **STRUCTURANT** (change la forme du produit), **DÉTAIL** (peut attendre le
> prototype).

---

## BLOQUANT

### B1. Endoxa est-il un projet personnel, un service civique ou une startup ?

**Enjeu :** ce choix détermine qui arbitre la neutralité, ce qu'on fait des données de valeurs, et si l'app doit croître — trois contraintes UX incompatibles entre elles.

- **Projet personnel/artistique** : pas d'obligation de croissance, on peut assumer une audience de 500 personnes, la modération peut rester manuelle et lente ; en contrepartie, aucun budget pour l'accessibilité sérieuse ni la modération 7j/7.
- **Bien commun civique (asso, fondation)** : impose la transparence de l'algorithme de profil et une gouvernance éditoriale explicite ; oblige à traiter la neutralité comme un engagement public opposable, donc à outiller lourdement le signalement.
- **Startup** : impose un modèle économique, donc soit de la donnée de valeurs monétisable (juridiquement toxique, voir C8), soit un abonnement — ce qui change tout l'onboarding (mur de valeur, essai, conversion).

**Recommandation :** trancher « bien commun civique » — c'est le seul cadre où « profil de valeurs » et « contrôle de neutralité par les utilisateurs » ne sont pas des promesses intenables.

### B2. Quelle est l'unité d'interaction du vote : une proposition à la fois, ou une grille 4×5 ?

**Enjeu :** c'est la différence entre 4 décisions séquentielles et 20 cases à arbitrer simultanément — l'écran le plus important de l'app en dépend, et la session de 3 minutes avec.

- **Séquentiel, une proposition par écran** : 4 décisions bien cadrées, très accessible, mais l'utilisateur ne voit jamais ses jugements côte à côte et ne peut donc pas les calibrer les uns par rapport aux autres — ce qui est le cœur du jugement majoritaire.
- **Grille complète visible** : permet la comparaison et la correction, mais sur un écran mobile 20 cibles tactiles produisent une densité difficilement accessible et un effet « formulaire ».
- **Séquentiel puis récapitulatif modifiable** : coût d'un écran supplémentaire, mais réconcilie décision guidée et calibration finale ; rallonge la session.
- **Tri/glisser des 4 propositions puis attribution de mentions** : très engageant, mais exclut d'emblée motricité fine et lecteurs d'écran.

**Recommandation :** séquentiel + écran de calibration final modifiable — le jugement majoritaire n'a de sens que si l'utilisateur peut voir et ajuster ses mentions ensemble.

### B3. Que fait l'utilisateur quand aucune des 4 justifications proposées par l'IA ne correspond à ce qu'il pense ?

**Enjeu :** c'est le point d'abandon le plus probable du parcours et le plus grand risque pour la promesse « se connaître » — une justification fausse pollue le profil et donne à l'utilisateur le sentiment qu'on lui met des mots dans la bouche.

- **Cinquième option « aucune de ces raisons »** : honnête, sans texte libre, mais produit un trou dans le profil et une frustration sans issue si elle est choisie souvent.
- **Bouton « proposer d'autres formulations »** qui régénère 4 alternatives : sauve la situation, mais crée un temps d'attente IA en plein milieu d'une session de 3 minutes et une boucle potentiellement infinie.
- **Autoriser la sélection de plusieurs justifications, y compris contradictoires** : reflète mieux la réalité mentale, mais complique radicalement le calcul du profil et l'affichage.
- **Rendre l'étape sautable sans pénalité** : préserve le flux, mais si 40 % sautent, l'étape ne sert à rien et doit disparaître du produit.

**Recommandation :** « aucune ne correspond » + une seule régénération possible, et instrumenter le taux d'usage : au-delà de 25 %, c'est la génération IA qui est à revoir, pas l'écran.

### B4. Comment explique-t-on à un utilisateur qui n'a jamais entendu parler du jugement majoritaire pourquoi c'est la proposition C qui gagne ?

**Enjeu :** si le résultat paraît arbitraire ou « faux », toute la crédibilité de l'app s'effondre au premier écran de résultats — et le jugement majoritaire produit régulièrement des vainqueurs contre-intuitifs pour qui raisonne en pourcentages.

- **Ne pas l'expliquer, afficher seulement la mention médiane de chaque proposition** : simple et visuellement calme, mais l'utilisateur qui a mis « Excellent » à une proposition perdante ne comprendra pas.
- **Afficher le profil de mérite complet (barres empilées des 5 mentions)** : c'est la représentation canonique et honnête, mais elle demande un temps d'apprentissage et casse le rythme de 3 minutes.
- **Expliquer une seule fois lors de l'onboarding, puis afficher la forme compacte** : bon compromis, mais l'explication sera oubliée et il faut un accès permanent au rappel.
- **Abandonner le jugement majoritaire et ne garder que l'échelle de mentions comme instrument d'introspection** : supprime tout le problème pédagogique, au prix de la dimension « décision collective » (voir C5).

**Recommandation :** profil de mérite empilé + une phrase générée en langage naturel (« la majorité juge C au moins Bien ») — l'explication doit être dans le résultat, pas dans un tutoriel.

### B5. Par quelle mécanique un vote sur une question d'actualité devient-il un point sur les 10 valeurs de Schwartz, et qu'en montre-t-on à l'utilisateur ?

**Enjeu :** c'est le cœur de la promesse ; si le lien vote → valeur est opaque ou perçu comme arbitraire, l'app devient un test de personnalité de magazine.

- **Étiquetage a priori de chaque proposition par la rédaction/l'IA** : simple, auditable, mais fait porter à un éditeur invisible la définition de ce qu'est « la sécurité » ou « l'autonomie ».
- **Le profil vient de la justification choisie, pas du vote** : plus défendable psychologiquement (la raison porte la valeur, pas la position), et cela donne enfin un rôle indispensable à l'étape de justification.
- **Le profil vient de la combinaison vote × justification × source de conviction** : le plus riche, mais devient inexplicable à l'utilisateur, donc incontestable, donc non crédible.
- **Modèle mixte avec affichage systématique de la contribution du jour** (« ce vote a nourri : Autonomie +, Sécurité − ») : coûte un écran, mais rend le profil vérifiable pas à pas.

**Recommandation :** faire porter la valeur par la justification et afficher la contribution après chaque vote — un profil qu'on ne peut pas tracer est un profil qu'on ne peut pas croire.

### B6. À partir de combien de votes affiche-t-on un profil, et que voit l'utilisateur avant ce seuil ?

**Enjeu :** l'état « profil vide » est l'écran le plus vu des 10 premiers jours ; s'il est vide ou faux, l'utilisateur part avant que le moteur de rétention n'existe.

- **Profil visible dès le premier vote, avec incertitude affichée** : gratification immédiate, mais un radar à 1 vote est un radar faux et donne des impressions durables et erronées.
- **Profil verrouillé jusqu'aux 10 dilemmes d'amorçage, puis révélé d'un coup** : crée un vrai moment, justifie l'onboarding, mais tout utilisateur qui abandonne au dilemme 6 repart sans rien.
- **Révélation valeur par valeur, chaque valeur se « déverrouillant » quand elle a assez de signal** : transforme le vide en progression lisible et donne une raison de revenir demain ; coût de conception élevé.
- **Pas de profil du tout au début, mais un journal chronologique de ses votes et justifications** : toujours plein dès le vote 1, honnête, mais ne tient pas la promesse annoncée.

**Recommandation :** révélation progressive valeur par valeur — c'est le seul design où l'état vide est lui-même le moteur de rétention.

### B7. Quelle est exactement la granularité du profil visible par les autres membres d'un groupe ?

**Enjeu :** « votes anonymes, profils visibles » n'est tenable que si le profil partagé ne permet pas de reconstituer les votes ; cette granularité est le paramètre qui décide si l'anonymat existe ou non.

- **Radar complet des 10 valeurs, identique au profil personnel** : la promesse la plus riche, mais dans un groupe de 5 personnes sur un sujet clivant, croiser les radars et le résultat collectif désigne le votant minoritaire (voir C4).
- **Top 3 des valeurs dominantes uniquement, sans intensité** : réduit fortement la ré-identification, reste conversationnel, mais appauvrit l'intérêt de la comparaison.
- **Pas de profils individuels, uniquement un profil agrégé du groupe + son écart au sien** : anonymat solide, mais supprime purement et simplement la fonctionnalité annoncée comme différenciante.
- **Profil complet mais partagé sur décision individuelle, membre par membre** : respecte le consentement, mais crée une pression sociale à partager dans une famille ou une classe, où refuser est un signal.

**Recommandation :** top 3 sans intensité par défaut, radar complet uniquement en partage réciproque explicite — l'anonymat des votes est une promesse, pas un réglage.

### B8. Que se passe-t-il dans un groupe de 2 ou 3 membres, où l'anonymat des votes est mathématiquement impossible ?

**Enjeu :** la cible prioritaire contient énormément de groupes de 2 à 4 personnes ; sans réponse, la promesse d'anonymat est un mensonge dès le premier cas d'usage réel.

- **Seuil dur : pas de résultat collectif en dessous de 5 votants** : protège l'anonymat, mais rend l'app inutilisable pour un couple ou une fratrie — soit une part majeure de la cible.
- **En dessous du seuil, mode « ouvert » assumé : les votes sont nominatifs et l'app le dit clairement** : honnête, et sans doute plus intéressant à 2 (c'est une conversation) ; mais crée deux produits différents à concevoir et à expliquer.
- **Résultat agrégé seulement, jamais individuel, quel que soit l'effectif** : cohérent, mais à 2 personnes l'agrégat révèle trivialement l'autre vote dès qu'on connaît le sien.
- **Fusionner les petits groupes dans un pool anonyme plus large** : préserve l'anonymat, mais détruit l'intérêt du groupe réel.

**Recommandation :** mode « ouvert » explicite en dessous de 5 membres, choisi à la création du groupe et affiché en permanence — mieux vaut un anonymat absent qu'un anonymat faux.

### B9. Comment traite-t-on les mineurs et les groupes à rapport de pouvoir asymétrique ?

**Enjeu :** une classe signifie des mineurs, un enseignant qui voit les profils de valeurs de ses élèves, et des données d'opinion — risque juridique et humain qui conditionne l'existence même du cas d'usage scolaire.

- **Interdire les moins de 15 ans et renoncer au cas d'usage scolaire** : simple, sûr, mais supprime le segment le plus évident et le plus enthousiaste.
- **Autoriser avec consentement parental et un mode « classe » où l'enseignant ne voit que l'agrégat** : préserve le cas d'usage, mais impose un rôle asymétrique à concevoir entièrement, et l'agrégat d'une classe de 25 reste identifiant sur les valeurs rares.
- **Autoriser sans distinction, en misant sur la responsabilité de l'adulte** : coût de conception nul, exposition juridique maximale.
- **Créer une version institutionnelle distincte, profils désactivés** : produit propre pour l'école, mais c'est un second produit.

**Recommandation :** v1 réservée aux 15 ans et plus, mode classe repoussé et conçu comme un produit séparé — la classe n'est pas un groupe, c'est une institution.

### B10. Rejouer une question passée modifie-t-il le profil ?

**Enjeu :** « rejouable à volonté » plus « le profil se construit vote après vote » permet, tel quel, de fabriquer artificiellement son profil.

- **Les rejeux nourrissent le profil comme les votes du jour** : cohérent, mais permet le farming et surpondère les utilisateurs disponibles.
- **Les rejeux ne nourrissent pas le profil, mode « exploration »** : protège l'intégrité, mais l'utilisateur ne comprendra pas que son effort ne compte pas.
- **Un seul vote comptabilisé par question, le rejeu écrase le précédent et le profil se recalcule** : intellectuellement le plus juste, et rend visible l'évolution ; coût technique du recalcul.
- **Le rejeu compte, mais plafonné** : limite le farming sans frustrer, au prix d'une règle arbitraire.

**Recommandation :** un vote par question, le rejeu écrase et recalcule — c'est aussi la seule implémentation crédible de « changer d'avis est valorisé ».

### B11. Peut-on voter sans avoir ouvert le dossier factuel ?

**Enjeu :** le dossier est ce qui distingue Endoxa d'un sondage d'humeur ; optionnel il sera ignoré, obligatoire il fait exploser la session de 3 minutes.

- **Optionnel, replié par défaut** : session courte, mais on mesure des réflexes et non des jugements — le profil mesure alors des biais, pas des valeurs.
- **Obligatoire, écran plein avant le vote** : garantit l'exposition aux faits, mais ajoute 60 à 90 secondes et crée un mur au moment le plus fragile.
- **Vote déverrouillé après consultation, dossier court par contrat (3 chiffres, 2 arguments, 40 secondes)** : impose une discipline éditoriale forte et rend la contrainte tenable.
- **Voter d'abord, dossier ensuite, avec correction possible** : très puissant pédagogiquement, mais double les étapes et complique l'agrégation.

**Recommandation :** dossier obligatoire mais borné à 40 secondes de lecture par contrat éditorial — mesurer un vote non informé serait mesurer autre chose que ce qu'on prétend mesurer.

### B12. Que propose-t-on à l'utilisateur qui, à 8h05, a tout fait et n'a plus rien à faire ?

**Enjeu :** l'app est structurellement vide 23h57 par jour ; sans réponse, la rétention dépend entièrement d'une notification.

- **Rien : écran de clôture assumé, « à demain »** : cohérent avec le ton calme et anti-addictif.
- **Proposer une question passée à rejouer** : remplit le vide, mais entre en collision directe avec B10.
- **Ouvrir la lecture du profil en profondeur** : contenu infini, aligné sur la promesse, et ne pollue pas les données.
- **Montrer l'état d'avancement du groupe** : donne une raison de revenir dans la journée, mais crée une pression sociale.

**Recommandation :** clôture nette + une seule porte ouverte, l'exploration de son propre profil — l'app doit avoir une fin quotidienne, c'est sa signature.

### B13. Quel modèle d'interaction accessible pour l'échelle à 5 mentions + « sans avis » ?

**Enjeu :** ce composant est utilisé 4 fois par jour par 100 % des utilisateurs ; s'il n'est pas utilisable au lecteur d'écran et au clavier, l'app est inaccessible, point.

- **Groupe de boutons radio** : sémantique native, parfait au lecteur d'écran et au clavier, mais visuellement banal et large sur mobile.
- **Slider à 5 crans** : compact et élégant, mais notoirement difficile au lecteur d'écran et à la motricité fine.
- **Liste déroulante** : accessible et minuscule, mais masque l'échelle donc empêche toute calibration.
- **Radios stylisés en échelle continue, « sans avis » détaché physiquement** : le meilleur des deux, au prix d'un travail CSS/ARIA sérieux et d'un test réel.

**Recommandation :** radios stylisés en échelle, « sans avis » détaché — l'accessibilité de ce composant n'est pas négociable puisqu'il est le produit.

---

## STRUCTURANT

### S1. Les 10 dilemmes d'amorçage se font-ils en une seule séance ?

- **Les 10 en une séance (10 à 15 min)** : profil crédible immédiatement, mais l'onboarding le plus long du marché sur une app dont l'argument est la brièveté.
- **3 d'entrée puis 1 par jour pendant 7 jours** : onboarding indolore, mais aucun profil digne de ce nom avant J7.
- **10 en une séance, interruptible avec reprise exacte** : compromis raisonnable, exige un état de reprise robuste.
- **Aucun onboarding** : friction zéro, mais le profil vide devient un problème (B6).

**Recommandation :** 3 puis 1 par jour — la première séance doit démontrer la promesse de brièveté, pas la contredire.

### S2. Que voit-on au tout premier lancement, avant tout compte ?

- **Trois écrans explicatifs puis inscription** : la promesse est posée, mais c'est le format le plus ignoré qui existe.
- **La question du jour immédiatement, jouable sans compte, compte demandé au moment de voir le profil** : démonstration au lieu d'explication ; complexité technique du compte différé.
- **Un dilemme d'amorçage unique et très personnel avant toute explication** : entre immédiatement dans le registre introspectif ; risque de désorienter.
- **Inscription d'abord** : données propres, taux d'abandon maximal.

**Recommandation :** jouer d'abord, s'inscrire au moment de voir son profil.

### S3. Qu'est-ce qui accueille quelqu'un revenant après trois semaines ?

- **Rien de spécial** : sans culpabilisation, mais gaspille un moment où l'on pourrait re-démontrer la valeur.
- **Un rattrapage proposé** : donne du contenu, mais crée une dette visible.
- **Un bilan de l'absence : « voici ce qui a occupé le pays », en 4 questions** : transforme l'absence en contenu ; coût éditorial réel.
- **Une relecture de son profil et de son évolution** : recentre sur la promesse, mais suppose un profil déjà fourni.

**Recommandation :** un bilan d'absence en 3 questions marquantes, sans notion de retard.

### S4. Y a-t-il une notion de série ?

- **Streak visible classique** : rétention prouvée, mécanique anxiogène incompatible avec la promesse.
- **Aucun compteur** : cohérent avec le ton, retire le seul levier gratuit.
- **Compteur cumulatif non cassable** : progression sans punition ; moins puissant.
- **Progression du profil comme unique compteur** : l'indicateur est la promesse elle-même.

**Recommandation :** compteur cumulatif non cassable, aucune notion de série.

### S5. À quelle heure paraît la question, dans quel fuseau, et que voit-on avant ?

- **6h heure de Paris, écran d'attente ailleurs** : simple, mais écran mort pour les ultramarins et expatriés.
- **Journée glissante par fuseau** : confortable, mais désynchronise les groupes.
- **Heure fixe avec compte à rebours explicite** : honnête et prévisible ; nécessite un écran d'attente non vide.
- **Contexte et dossier publiés la veille au soir** : remplit l'attente et permet de venir préparé.

**Recommandation :** heure fixe unique avec dossier consultable la veille.

### S6. Que fait l'app hors connexion ?

- **Blocage** : garantit un abandon total en mobilité.
- **Question préchargée, vote stocké localement, synchronisation différée** : sauve la session ; il faut concevoir l'écran « résultats en attente ».
- **Tout en local avec rattrapage notifié** : très robuste, crée une seconde visite (aussi une opportunité).
- **Précharger plusieurs jours** : contredit le rythme quotidien.

**Recommandation :** préchargement du jour + écriture locale + écran « résultats à venir ».

### S7. Où est le point de non-retour, et peut-on revenir en arrière ?

- **Modifiable jusqu'aux résultats, rien après** : lisible, mais fige le vote au moment où l'argument d'en face pourrait le faire bouger.
- **Modifiable jusqu'à minuit, résultats recalculés** : donne un sens réel au changement d'avis, mais résultats instables.
- **Vote figé + « second vote » distinct après l'argument d'en face, les deux conservés** : exploite le mieux la promesse.
- **Retour libre avant validation, rien ensuite** : classique, mais réduit « changer d'avis » à un slogan.

**Recommandation :** vote figé + second vote optionnel, les deux conservés.

### S8. L'argument d'en face est-il obligatoire ?

- **Obligatoire, écran plein entre résultat et profil** : garantit l'exposition, vécu comme un péage.
- **Optionnel, bouton discret** : taux de consultation sous 20 %, donc l'étape n'existe pas.
- **Obligatoire mais très court, une phrase personnalisée contre sa propre position** : tenable en 10 secondes et réellement décentrant.
- **Placé avant les résultats** : maximise l'effet, rallonge l'effort avant toute récompense.

**Recommandation :** obligatoire, une seule phrase personnalisée — c'est le seul écran qui justifie le mot *endoxa*.

### S9. Le profil est-il cumulatif à vie ou sur fenêtre glissante ?

- **Cumulatif à vie** : stable, mais à 400 votes plus rien ne bouge et la rétention s'éteint.
- **Fenêtre glissante (90 jours)** : reste vivant, mais l'utilisateur voit son profil « perdre » des acquis.
- **Deux vues : profil actuel glissant + trajectoire historique** : la plus fidèle à la promesse, objet à concevoir en double.
- **Cumulatif à pondération décroissante** : élégant et invisible, donc inexplicable à qui conteste.

**Recommandation :** profil actuel glissant + trajectoire consultable, avec bande d'incertitude visible.

### S10. Que peut faire l'utilisateur qui n'est pas d'accord avec ce que l'app dit de lui ?

- **Rien** : chaque désaccord devient une sortie.
- **Explication à la demande : les 5 votes qui ont le plus contribué** : transforme le désaccord en compréhension.
- **Correction manuelle du profil** : apaisant, mais détruit la validité — on mesurerait l'image de soi souhaitée.
- **Un dilemme d'arbitrage supplémentaire sur la valeur contestée** : l'app répond par une question plutôt que par une justification.

**Recommandation :** explication traçable + dilemme d'arbitrage ; jamais de correction manuelle.

### S11. Quel vocabulaire pour les 10 valeurs de Schwartz ?

- **Vocabulaire académique fidèle** : rigueur, mais « vous êtes fort en Pouvoir » se lit comme une insulte.
- **Reformulation en verbes ou en phrases** : neutralise la connotation, mais s'éloigne du modèle.
- **Étiquette académique + définition toujours visible** : honnête, mais alourdit tous les écrans.
- **Étiquettes maison entièrement repensées** : contrôle du ton, au prix de la crédibilité scientifique.

**Recommandation :** reformulation en phrase, étiquette académique en second plan.

### S12. Comment orchestre-t-on la première apparition du profil ?

- **Apparition silencieuse dans un onglet** : l'événement passe inaperçu.
- **Moment dédié plein écran avec lecture guidée** : impact maximal ; risque de sur-promesse si le profil est faible.
- **Révélation en deux temps** : ménage l'effet, peut frustrer.
- **Déclenchée par l'utilisateur** : très juste émotionnellement ; certains ne cliqueront jamais.

**Recommandation :** moment plein écran déclenché par l'utilisateur — se voir doit être un acte choisi.

### S13. Comment présente-t-on les résultats à quelqu'un de très minoritaire ?

- **Pourcentages bruts** : honnête, mais le chiffre isolé est une gifle et l'écran suivant est l'argument d'en face.
- **Ne jamais afficher la position relative** : protège, mais prive de l'information la plus intéressante.
- **Contextualiser (« majoritaire chez les 18-24 ans »)** : redonne de la dignité ; exige un volume de données que la v1 n'aura pas.
- **Afficher d'abord ce qui rassemble** : cohérent avec « ni débat ni militantisme », mais peut sembler éluder.

**Recommandation :** point d'accord d'abord, puis position relative sans emphase — une minorité doit être informée, pas exposée.

### S14. Que voit une personne invitée dans un groupe avant d'avoir accepté ?

- **Rien** : protège les membres, mais on n'accepte pas ce qu'on ne voit pas.
- **Nom, nombre de membres, invitant** : minimum viable, sans envie particulière.
- **Aperçu du profil agrégé du groupe** : très engageant, mais expose une donnée sensible à un non-membre.
- **La question du jour, jouable avant d'accepter** : convertit par l'expérience sans rien exposer.

**Recommandation :** nom, membres, et question du jour jouable ; jamais de données de valeurs avant acceptation réciproque.

### S15. Que se passe-t-il dans un groupe où personne d'autre ne vote ?

- **Attente indéfinie** : un seul membre inactif bloque le groupe à vie.
- **Clôture automatique à minuit** : garantit un résultat, mais publie parfois un résultat à un seul votant.
- **Résultat de groupe au-delà du seuil, repli sur le national sinon** : jamais d'écran vide.
- **Relance douce du groupe** : réamorce la vie, mais installe une pression sociale.

**Recommandation :** clôture à minuit, résultat de groupe si le seuil est atteint, repli sur le national — aucun écran ne doit dépendre du comportement d'un tiers.

### S16. Que se passe-t-il quand quelqu'un quitte un groupe ?

- **Départ silencieux, votes conservés** : préserve les résultats, mais disparition inexpliquée.
- **Départ notifié, votes anonymisés et conservés en agrégat** : socialement honnête ; la notification peut humilier.
- **Retrait complet et recalcul** : respect maximal, mais les résultats déjà vus changent rétroactivement.
- **Sortie en sourdine** : évite le drame, au prix d'une ambiguïté.

**Recommandation :** départ notifié sobrement, contributions conservées en agrégat anonymisé, profil retiré immédiatement.

### S17. Avec plusieurs groupes, ai-je un profil ou plusieurs visages ?

- **Un profil unique visible partout** : simple, mais personne ne veut montrer les mêmes valeurs à ses parents et à ses collègues.
- **Visibilité réglable par groupe** : respecte les contextes, mais « masqué » devient socialement suspect.
- **Granularité imposée par le type de groupe** : réglage par défaut intelligent, typologie rigide.
- **Un profil par groupe, calculé sur les seules questions du groupe** : propre, mais fragmente les données.

**Recommandation :** granularité par défaut selon le type de groupe, ajustable.

### S18. Que se passe-t-il quand un utilisateur signale un manque de neutralité ?

- **Signalement simple avec compteur public** : transparent, mais transforme chaque question clivante en champ de bataille.
- **Signalement typé, traité humainement sous 24 h** : exploitable, mais suppose une équipe.
- **Au-delà d'un seuil, retrait et annulation des effets sur les profils** : protège l'intégrité, mais permet le brigading.
- **Régénération d'une cinquième proposition en cours de journée** : réactif, mais rend le scrutin incomparable entre le matin et le soir.

**Recommandation :** signalement typé, traitement humain sous 24 h, correction jamais rétroactive et toujours publiée.

---

## DÉTAIL

### D1. L'ordre des 4 propositions est-il fixe, aléatoire, ou éditorialisé ?

L'effet de position est massif et biaisera systématiquement les profils.
**Recommandation :** aléatoire, stable par groupe, propositions nommées par leur intitulé et jamais par un numéro.

### D2. « Sans avis » s'applique-t-il à une proposition ou à la question entière ?

**Recommandation :** par proposition, plus un « passer cette question » explicite en amont ; l'abstention est une information, pas un vide.

### D3. Que fait-on d'un utilisateur qui attribue la même mention aux 4 propositions ?

**Recommandation :** l'accepter, le compter dans le résultat collectif, l'exclure du profil, et le dire.

### D4. Comment le radar est-il lisible en daltonisme et en niveaux de gris ?

Aucune palette de 10 teintes n'est distinguable de façon fiable.
**Recommandation :** deux couleurs maximum ; l'identité des valeurs passe par la position et l'étiquette, pas par la teinte.

### D5. Quelle taille de cible tactile pour les mentions sur 360 px ?

**Recommandation :** échelle horizontale avec hauteur de zone tactile généreuse et correction possible avant validation.

### D6. Quel niveau de langue et quelle longueur pour le dossier factuel ?

**Recommandation :** version courte contrainte (niveau B1, 90 mots) par défaut, version longue accessible en un geste.

### D7. L'utilisateur peut-il exporter ou supprimer son profil ?

**Recommandation :** export lisible et suppression du profil indépendante du compte, accessibles en deux gestes.

### D8. Le pseudo est-il unique, modifiable, modéré ?

**Recommandation :** pseudo global modifiable et modéré, surnom propre à chaque groupe.

### D9. Peut-on partager son profil hors de l'app ?

**Recommandation :** image de partage conçue mais jamais suggérée par l'app, avec l'incertitude du profil visible dessus.

### D10. Quels sont les mots exacts des 5 mentions ?

- **Canonique** (À rejeter / Insuffisant / Passable / Bien / Très bien) : éprouvé, mais scolaire.
- **Adhésion** (Non / Plutôt non / Mitigé / Plutôt oui / Oui) : immédiat, mais retombe dans le binaire que le jugement majoritaire veut dépasser.
- **Appréciation** (Inacceptable / Faible / Acceptable / Convaincant / Nécessaire) : cohérent avec l'introspection, à tester car « nécessaire » n'est pas une intensité de « convaincant ».

**Recommandation :** registre de l'appréciation, testé sur 20 personnes avant tout développement.

---

## Contradictions repérées

### C1. La cible prioritaire est le groupe, mais la v1 est solo

La cible est « des groupes qui existent déjà », et les moteurs de rétention annoncés sont *le profil* **et** *la vie du groupe*. La v1 supprime la moitié des moteurs et la totalité de la cible. Elle ne teste donc pas le produit, elle teste une app d'introspection solo auprès d'un public qui n'est pas la cible. Les enseignements seront non transférables — un utilisateur solo qui reste ne prouve rien sur une famille, et un utilisateur solo qui part ne prouve rien non plus.

**À trancher :** soit la cible devient l'individu et le groupe est une extension, soit la v1 inclut un groupe minimal.

### C2. La session de 3 minutes est arithmétiquement impossible

Contexte 15 s + dossier factuel 60–90 s + 4 jugements sur 5 mentions 60–90 s + justification 20 s + source 10 s + résultats 30 s + argument d'en face 20 s + profil 30 s = **4 à 6 minutes**, sans compter l'hésitation.

**À trancher :** ce qu'on supprime — le dossier factuel long, l'étape « source de conviction », ou la consultation quotidienne du profil.

### C3. « Aucune saisie de texte libre » contredit « contrôle de neutralité par les utilisateurs »

Un signalement de non-neutralité utile est nécessairement argumenté : *quelle* proposition manque, *quel* chiffre est faux, *quelle* formulation est orientée. Un signalement sans texte n'est qu'un compteur d'insatisfaction, corrélé au désaccord politique et non à la partialité réelle.

**À trancher :** formulaire à choix fermés très fin, ou texte libre dans ce seul canal privé.

### C4. Le profil visible en groupe annule l'anonymat des votes

Groupe de 4 : A, B, C, D. Question sur l'accueil des demandeurs d'asile. Résultat : 3 « Bien » et 1 « À rejeter » sur la proposition d'accueil élargi. Les profils sont visibles : A, B et C ont Universalisme et Bienveillance en tête, D a Sécurité et Tradition. Personne n'a besoin d'information supplémentaire pour savoir qui a voté quoi.

L'anonymat n'est pas affaibli, il est **nul** — et il est nul précisément sur les questions où il compte. Ce n'est pas un cas limite : c'est le comportement normal du système dès que l'effectif est faible et le sujet clivant, c'est-à-dire le cas d'usage central.

**À trancher :** B7 et B8 ensemble ; « votes anonymes + profils visibles » ne peut pas être annoncé tel quel.

### C5. Le jugement majoritaire sert une finalité qui n'est pas celle du produit

Le jugement majoritaire est un mode de scrutin : sa raison d'être est de désigner un vainqueur collectif légitime. L'objectif affiché est que l'utilisateur *se connaisse mieux*. Ce qui sert l'introspection, c'est l'échelle graduée (juger chaque option indépendamment plutôt que choisir un camp) et la justification — pas l'agrégation par mention médiane, qui ajoute une lourde charge pédagogique au service d'une finalité collective que le produit dit ne pas poursuivre.

**À trancher :** garder l'échelle de mentions, et décider si le mécanisme d'agrégation est mis en avant ou reste en arrière-plan.

### C6. « Ni débat ni militantisme » contredit l'argument d'en face et le résultat collectif

Le produit affiche une question clivante, les arguments de chaque camp, le résultat de son groupe et l'argument opposé au sien — puis annonce qu'il ne s'agit pas d'une app de débat. Dans une famille, le débat n'aura pas lieu dans l'app : il aura lieu à table, avec les données de l'app comme munitions. Le produit **produit du débat sans fournir aucun outil pour le conduire**.

**À trancher :** assumer que le débat est hors app et concevoir ce qui aide à le tenir, ou fournir un espace cadré.

### C7. La rejouabilité illimitée contredit l'intégrité du profil

Si toutes les questions sont rejouables et que chaque vote nourrit le profil, le profil ne mesure plus des valeurs mais du temps disponible.

**À trancher :** B10, avec une règle explicite et affichée.

### C8. Le profil est une donnée sensible, mais la collecte est présentée comme minimale

« Pseudo, tranche d'âge, région » est décrit comme léger. Or l'objet central du produit relève des catégories particulières du RGPD, et le croisement pseudo × âge × région ré-identifie trivialement une personne dans une commune moyenne.

**À trancher :** l'usage réel de la région (si rien ne l'exploite en v1, ne pas la collecter), et un consentement explicite distinct de l'inscription.

### C9. Le ton calme et la rétention quotidienne tirent en sens opposés

Deux notifications par jour, un rendez-vous quotidien, un profil qui se complète et un bilan hebdomadaire constituent une architecture de rétention classique. Or le succès affiché est que l'utilisateur *se connaisse mieux* — un état atteint, après lequel il n'a plus besoin de revenir. Un produit d'introspection réussi devrait générer du départ.

**À trancher :** l'usage quotidien indéfini (et renoncer au discours anti-addictif), ou l'atteinte d'un état avec un design de sortie honorable, qui n'existe nulle part dans le cadrage.
