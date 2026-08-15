# Questions — Architecture technique

> Produit par le rôle correspondant de l'équipe (voir `docs/methode.md`).
> Questions, pas solutions. Niveaux : **BLOQUANT**, **STRUCTURANT**, **DÉTAIL**.

---

*45 questions à trancher avant la première ligne de code. Classement : **BLOQUANT** (rien de solide ne peut être écrit avant), **STRUCTURANT** (décidable en semaine 1-4, mais coûteux à changer ensuite), **DÉTAIL** (arbitrable au fil de l'eau).*

---

## 0. Le cadre avant la technique

#### Q1 — [BLOQUANT] Le projet est-il personnel, associatif ou entrepreneurial, et à quelle date cette question sera-t-elle tranchée ?
**Enjeu.** Tout choix d'architecture, de coût et de conformité découle de cette réponse ; la laisser ouverte revient à concevoir pour l'hypothèse la plus chère « au cas où ».
- **Projet personnel assumé, plafond 500 utilisateurs** — un serveur unique, une base PostgreSQL, aucune redondance, coût < 25 €/mois. Le RGPD reste intégralement applicable.
- **Associatif** — impose une personne morale, un responsable de traitement identifié, des statuts, et une continuité de service au-delà de vous. Rend la relecture éditoriale quotidienne mutualisable.
- **Produit à monter** — impose dès le départ la séparation propre du domaine métier, un modèle de coûts, et probablement un co-fondateur : la boucle éditoriale quotidienne 7j/7 est incompatible avec une personne seule sur 12 mois.
- **Non tranché** — vous paierez la sur-ingénierie sans obtenir la robustesse, parce qu'un système conçu pour « peut-être 500 000 » et opéré par une personne casse aux deux extrémités.

> **Reco.** Trancher « personnel avec option associative » et poser un plafond explicite de 5 000 utilisateurs pour la v1 : c'est le seul cadre où le projet existe réellement dans six mois.

#### Q2 — [BLOQUANT] Combien d'heures par semaine, sur combien de mois, et par combien de personnes ?
**Enjeu.** Le périmètre décrit (rituel + scoring psychométrique + archives rejouables + pipeline IA + groupes) représente un ordre de grandeur de 900 à 1 400 heures de développement ; si le budget réel est de 8 h/semaine en solo, la v1 telle que décrite arrive dans trois ans.
- **8 h/semaine solo** — il faut couper : pas d'archives rejouables, pas de bilan hebdo, pas de dossier factuel en v1. Boucle vote + résultats + profil uniquement.
- **20 h/semaine solo** — v1 réaliste en 8-10 mois, à condition de ne rien construire soi-même qui existe managé.
- **2 personnes à 15 h** — permet de séparer le pipeline éditorial du produit, ce qui est la vraie difficulté.

> **Reco.** Chiffrer le budget-heures avant tout, puis retirer du périmètre v1 jusqu'à ce que ça tienne en 400 heures : ce qui reste est le produit.

#### Q3 — [STRUCTURANT] Quel est le plafond mensuel acceptable, en euros, toutes charges comprises ?
**Enjeu.** Sans plafond, on choisit des services au confort de développement et on découvre la facture au premier pic d'usage.
- **20 €/mois** — VPS unique, PostgreSQL auto-hébergé, sauvegardes manuelles vers un stockage objet. Vous êtes l'astreinte.
- **80 €/mois** — base managée avec sauvegardes point-in-time, CDN, monitoring. Le bon rapport risque/effort.
- **300 €/mois** — permet la redondance et un environnement de préproduction réel, injustifié avant plusieurs milliers d'utilisateurs actifs.

> **Reco.** 60-80 €/mois : le surcoût par rapport à l'auto-hébergement achète les sauvegardes vérifiées et le sommeil, qui sont les deux vrais besoins.

---

## 1. Ce que « PWA d'abord » implique réellement

#### Q4 — [BLOQUANT] La notification « question du jour » est le cœur du rituel : accepte-t-on qu'elle ne fonctionne sur iPhone que si l'utilisateur a explicitement ajouté l'app à son écran d'accueil ?
**Enjeu.** Sur iOS, le Web Push n'existe que pour une PWA installée depuis Safari — un utilisateur iPhone qui reste dans l'onglet ne recevra jamais rien, et le rituel quotidien s'effondre pour environ la moitié du public français.
- **On accepte et on conçoit un parcours d'installation explicite** — il faut un écran d'onboarding dédié (« Partager → Sur l'écran d'accueil »), non déclenchable par bouton sur iOS, avec un taux de conversion réaliste de 15 à 35 %.
- **On renonce aux notifications et on mise sur l'habitude** — honnête, mais la rétention d'un produit à rituel sans rappel chute typiquement d'un facteur 2 à 4.
- **Natif dès le départ** — résout la notification, mais ajoute les stores, la revue Apple (une app d'opinion politique est un sujet de revue sensible), la signature, et un cycle de mise à jour de plusieurs jours.
- **PWA + wrapper natif minimal ultérieur uniquement pour les notifications** — complexité de double distribution, mais préserve une base de code.

> **Reco.** PWA avec parcours d'installation traité comme une fonctionnalité produit de premier plan et mesuré ; le natif ne se justifie que si le taux d'installation iOS mesuré passe sous 20 %.

#### Q5 — [BLOQUANT] La notification « à l'heure choisie par l'utilisateur » sera-t-elle locale ou déclenchée par le serveur ?
**Enjeu.** Il n'existe pas de notification locale programmée fiable en PWA : « à 8 h 15 pour Marie, 19 h 30 pour Paul » impose un ordonnanceur serveur qui émet un push individuel par utilisateur et par jour, c'est-à-dire un service à part entière et non une option de réglage.
- **Push serveur individualisé** — à 10 000 utilisateurs, 10 000 pushs/jour étalés sur 24 h : trivial. À 500 000, c'est un ordonnanceur avec file, reprise sur erreur et gestion des abonnements expirés (comptez 20-30 % d'invalidation par an).
- **Créneaux fixes (matin / midi / soir)** — trois lots de diffusion par jour, infiniment plus simple à opérer, perte de personnalisation faible.
- **Heure unique pour tous** — un seul job, mais crée un pic de trafic massif concentré sur 10 minutes qui dimensionne toute l'infrastructure.

> **Reco.** Créneaux fixes au quart d'heure près en v1 : la granularité à la minute coûte un ordonnanceur et n'apporte rien de perceptible.

#### Q6 — [STRUCTURANT] Comment un utilisateur passe-t-il d'une version à la suivante, et que se passe-t-il s'il reste bloqué sur une version d'il y a trois semaines ?
**Enjeu.** Un service worker mal conçu sert indéfiniment l'ancienne version, et un utilisateur peut voter avec un client dont le format de payload n'est plus accepté.
- **Mise à jour silencieuse au prochain lancement** — décalage d'une session, acceptable, mais deux versions coexistent toujours en production.
- **Bandeau « nouvelle version disponible » avec rechargement explicite** — l'utilisateur contrôle, mais certains ne cliqueront jamais.
- **Version minimale imposée par l'API** — le serveur refuse les clients trop anciens et l'app force le rechargement. Le seul mécanisme qui garantit la cohérence, à condition de le construire dès la v1.

> **Reco.** Numéro de version du client envoyé à chaque appel API + refus au-delà de N versions de retard : c'est cinq lignes en v1 et une refonte en v3.

#### Q7 — [BLOQUANT] Quelles données peut-on perdre sans dommage si le navigateur purge le stockage local ?
**Enjeu.** iOS purge le stockage à écriture par script après 7 jours sans visite pour les sites non installés ; si l'identité de l'utilisateur vit uniquement là, il perd son compte et son historique de valeurs pour être parti une semaine en vacances.
- **Rien de critique en local, tout côté serveur** — nécessite une identité récupérable (voir Q34), coûte un aller-retour réseau à chaque ouverture.
- **Cache local de confort + serveur source de vérité** — bon compromis, mais il faut décider explicitement quelle donnée est du cache.
- **Identité et historique en local d'abord** — architecture « local-first » élégante, incompatible avec la purge iOS pour les non-installés, et avec le changement d'appareil.

> **Reco.** Serveur source de vérité, local strictement cache, et traiter toute donnée locale comme jetable par construction.

#### Q8 — [STRUCTURANT] Qu'est-ce qu'on perd exactement en PWA, et laquelle de ces pertes est inacceptable ?
**Enjeu.** Décider par élimination raisonnée plutôt que découvrir la limite en cours de route.
Pertes réelles à arbitrer : pas de widget écran d'accueil ; pas de notification locale programmée ; pas de synchronisation en arrière-plan sur iOS (l'API Background Sync n'est pas implémentée par Safari) ; pas de présence dans les stores donc pas de découvrabilité par recherche d'app ; pas d'intégration Raccourcis/Siri ; pastille de badge limitée ; pas de partage natif riche sur certaines plateformes ; désinstallation invisible (impossible de savoir qu'un utilisateur a supprimé l'app).
- **Toutes acceptables** — position par défaut cohérente avec « web d'abord ».
- **Le widget est le rituel** — si la vision produit est « je vois la question sur mon écran d'accueil », alors c'est du natif et il faut l'assumer maintenant.
- **La découvrabilité store est le canal d'acquisition** — alors la PWA n'est pas une stratégie de croissance.

> **Reco.** Écrire la liste des pertes dans le document de cadrage et la faire valider explicitement : c'est la seule façon d'éviter le « on passera au natif plus tard » qui coûte une réécriture.

---

## 2. Hors ligne et synchronisation

#### Q9 — [BLOQUANT] Peut-on voter dans le métro, sans réseau ?
**Enjeu.** Un rituel matinal de trois minutes se consomme majoritairement dans les transports ; si l'app affiche une erreur réseau, la promesse produit est cassée là où elle devait être tenue.
- **Non, réseau requis** — implémentation triviale, expérience mauvaise dans le cas d'usage le plus probable.
- **Consultation hors ligne, vote en ligne uniquement** — le dossier factuel préchargé se lit, le vote attend le réseau. Compromis honnête, mais la frustration arrive au moment de l'engagement.
- **Vote hors ligne mis en file et envoyé au retour du réseau** — la bonne expérience, mais impose de résoudre Q10 et Q11 avant, et un rejeu à l'ouverture de l'app (pas d'arrière-plan sur iOS).

> **Reco.** Vote hors ligne en file locale, envoyé au premier retour de connectivité ou à la réouverture — mais uniquement si Q10 est tranchée d'abord.

#### Q10 — [BLOQUANT] Un vote émis hors ligne à 8 h 50 et synchronisé à 21 h, alors que le scrutin est clos à 20 h, compte-t-il ?
**Enjeu.** C'est le cœur de l'intégrité du produit : accepter des votes horodatés côté client ouvre la porte à la manipulation triviale des résultats, les refuser trahit un utilisateur de bonne foi.
- **Horodatage client fait foi** — juste pour l'utilisateur, indéfendable : n'importe qui peut changer l'heure de son téléphone et voter après avoir vu les résultats.
- **Horodatage serveur fait foi, vote tardif rejeté** — intègre, mais l'utilisateur perd son vote et sa série.
- **Vote tardif accepté mais marqué « hors scrutin »** — il alimente le profil de valeurs personnel et l'archive, sans entrer dans les agrégats publics. Techniquement propre, explicable en une phrase.
- **Fenêtre de tolérance serveur (ex. +2 h après clôture)** — atténue sans résoudre, et crée une zone grise difficile à expliquer.

> **Reco.** Horodatage serveur pour l'agrégat, horodatage client conservé pour information, et vote hors délai compté dans le profil personnel mais pas dans les résultats — avec un message explicite.

#### Q11 — [STRUCTURANT] Que fait-on si le même utilisateur a voté sur deux appareils pour la même question ?
**Enjeu.** Sans règle d'idempotence, un utilisateur multi-appareil produit deux votes et corrompt les agrégats.
- **Clé unique (utilisateur, question) en base, premier arrivé gagne** — simple, déterministe, éventuellement surprenant pour l'utilisateur.
- **Dernier arrivé écrase** — cohérent avec « l'avis le plus récent », mais impose de recalculer l'agrégat et pose la question de la modification post-résultats.
- **Identifiant de vote généré par le client (UUID) + upsert idempotent** — permet de rejouer une file hors ligne sans risque de doublon, indispensable si Q9 est « oui ».

> **Reco.** UUID côté client + contrainte d'unicité (utilisateur, question) + premier arrivé gagne : c'est le seul triplet qui rend le rejeu hors ligne sûr.

#### Q12 — [STRUCTURANT] La question du jour et le dossier factuel sont-ils préchargés la veille au soir ?
**Enjeu.** Précharger implique que le contenu du jour J existe et est figé la veille, ce qui contraint tout le pipeline de génération (Q19) et interdit toute correction de dernière minute silencieuse.
- **Préchargement à la dernière ouverture de l'app** — bon hors ligne, mais publie du contenu avant l'heure officielle : quelqu'un le lira dans le cache.
- **Pas de préchargement** — le vote hors ligne devient impossible pour qui n'a pas ouvert l'app avec du réseau.
- **Préchargement chiffré, clé livrée à l'heure de bascule** — techniquement possible, complexité disproportionnée pour un enjeu faible.

> **Reco.** Préchargement en clair au dernier lancement, en acceptant qu'un contenu soit techniquement lisible en avance : l'enjeu de confidentialité est nul, l'enjeu d'usage est majeur.

---

## 3. Où vit le profil de valeurs

#### Q13 — [BLOQUANT] Le calcul du profil Schwartz s'exécute-t-il côté serveur ou côté client ?
**Enjeu.** Ce choix détermine si vous pouvez corriger un scoring erroné, et s'il détermine aussi ce que le serveur sait des opinions de chacun.
- **Serveur** — recalcul global possible en une commande, cohérence garantie, mais le serveur détient un profil politique nominatif : c'est une donnée de l'article 9 du RGPD à part entière, avec les obligations correspondantes.
- **Client uniquement, profil jamais transmis** — argument de confidentialité fort et sincère, mais : impossible de corriger rétroactivement, impossible d'afficher le profil aux membres d'un groupe (fonctionnalité v2 annoncée), perte totale au changement d'appareil.
- **Client, avec sauvegarde chiffrée de bout en bout côté serveur** — le serveur stocke un blob opaque. Résout la portabilité, ne résout ni le recalcul ni le partage en groupe, et impose une gestion de clés dont la perte est définitive.
- **Serveur, mais dérivé à la demande depuis les votes, jamais stocké** — le profil n'existe que le temps d'une réponse HTTP ; les votes bruts restent la seule donnée persistée.

> **Reco.** Calcul serveur, profil **dérivé et mis en cache**, jamais considéré comme source de vérité : les votes bruts versionnés sont la seule chose qu'on persiste, tout le reste se recalcule.

#### Q14 — [BLOQUANT] Que stocke-t-on exactement pour un vote, et avec quel niveau de détail ?
**Enjeu.** Si on ne stocke que le profil agrégé, aucun recalcul n'est jamais possible ; c'est la décision la plus irréversible du projet.
- **Profil agrégé seul** — base minuscule, confidentialité maximale, et l'impossibilité définitive de corriger une erreur de scoring ou de rejouer une archive.
- **Mentions des 4 propositions + justification choisie + origine de la conviction + identifiant de question + horodatage serveur + version du modèle de scoring** — permet tout : recalcul, archives rejouables, correction. C'est le format complet.
- **Format complet mais purge après N mois** — compromis de rétention, à arbitrer avec Q30.

> **Reco.** Format complet avec `scoring_version` dès le premier enregistrement : ajouter cette colonne après coup sur un historique est douloureux, l'ajouter au jour 1 est gratuit.

#### Q15 — [STRUCTURANT] Le centrage par utilisateur (ipsatisation) porte sur quelle fenêtre, et que montre-t-on à quelqu'un qui a voté trois fois ?
**Enjeu.** Un score centré sur trois observations est statistiquement du bruit ; l'afficher comme un « profil de valeurs » est à la fois faux et potentiellement blessant.
- **Centrage sur tout l'historique, affiché dès le premier vote** — profil très instable les premières semaines, effet « mon profil change tout le temps » qui détruit la confiance.
- **Centrage sur tout l'historique, mais affichage bloqué sous un seuil (ex. 10 dilemmes d'onboarding + 10 votes)** — c'est ce que fait l'onboarding par 10 dilemmes ; il faut décider si ces 10 suffisent seuls.
- **Fenêtre glissante (ex. 90 derniers votes)** — montre l'évolution, mais rend le profil dépendant du calendrier de l'actualité plutôt que de la personne.
- **Double affichage : profil stable (tout l'historique) + tendance récente (fenêtre)** — plus riche, deux fois plus de calcul et d'explication à produire.

> **Reco.** Centrage sur tout l'historique, affichage à partir de 10 observations, indicateur de confiance visible tant qu'on est sous 30 — l'honnêteté statistique est ici un argument produit.

#### Q16 — [BLOQUANT] Si le modèle de scoring change dans six mois, recalcule-t-on l'historique de tout le monde ?
**Enjeu.** Un profil qui bouge sans que l'utilisateur ait voté détruit la confiance dans l'objet central du produit, mais figer les anciens votes crée un historique incohérent, moitié v1 moitié v2.
- **Recalcul intégral rétroactif** — cohérence parfaite, profils qui bougent du jour au lendemain, et une opération de traitement par lot sur toute la base (à 500 000 utilisateurs × 300 votes = 150 millions de lignes, comptez plusieurs heures et une fenêtre de maintenance).
- **Aucun recalcul, chaque vote garde son scoring d'époque** — l'historique est stable, mais le profil global mélange des échelles incomparables : c'est faux psychométriquement.
- **Recalcul rétroactif + conservation de l'ancien profil consultable** — l'utilisateur peut voir « avant / après », coûte un stockage de snapshots et un écran de plus.
- **Nouveau modèle appliqué uniquement aux nouveaux comptes** — deux populations non comparables, poison à long terme pour toute analyse et pour les groupes.

> **Reco.** Recalcul intégral rétroactif, systématiquement, avec conservation d'un instantané daté du profil avant migration : le profil est une *dérivation* des votes, jamais un fait historique.

#### Q17 — [STRUCTURANT] Comment prévient-on l'utilisateur que son profil a changé sans qu'il ait voté ?
**Enjeu.** Sans annonce, un recalcul est indistinguable d'un bug, et le premier réflexe de l'utilisateur est de douter de tout le reste.
- **Rien** — perte de confiance silencieuse, et signalements de bug impossibles à traiter.
- **Note de version dans l'app** — minimum acceptable, peu lue.
- **Écran dédié au prochain lancement : « nous avons amélioré le calcul, voici ce qui a changé pour vous »** — transforme un incident de confiance en preuve de sérieux, coûte un écran et une comparaison avant/après.
- **Notification push** — intrusif pour un sujet froid, à réserver aux changements majeurs.

> **Reco.** Écran de changement au prochain lancement avec comparaison chiffrée avant/après et lien vers la méthodologie : c'est un différenciateur, pas une corvée.

#### Q18 — [STRUCTURANT] Un vote est-il modifiable ou supprimable après coup, et qu'arrive-t-il aux résultats agrégés déjà publiés ?
**Enjeu.** Le RGPD donne un droit d'effacement, mais retirer un vote d'un scrutin déjà clos et affiché change un résultat public.
- **Vote immuable, suppression du compte = anonymisation du vote qui reste dans l'agrégat** — défendable si annoncé clairement dès l'onboarding (le vote agrégé et anonymisé n'est plus une donnée personnelle).
- **Suppression réelle avec recalcul des agrégats historiques** — juridiquement le plus sûr, impose des agrégats recalculables donc non figés, et fait bouger des résultats archivés.
- **Modification possible avant clôture uniquement** — bon compromis d'usage, à combiner avec l'une des deux options ci-dessus.

> **Reco.** Modification libre avant clôture ; après clôture, immuabilité du vote et anonymisation irréversible à la suppression du compte, annoncée explicitement dans la politique de confidentialité.

---

## 4. Le temps, le fuseau, la définition d'un « jour »

#### Q19 — [BLOQUANT] À quelle heure et dans quel fuseau change la question du jour ?
**Enjeu.** Toute la logique du produit (série de votes, clôture, bilan hebdo, archives) dépend d'une définition canonique du « jour » qu'il sera impossible de changer rétroactivement sans réécrire l'historique.
- **Europe/Paris, minuit** — simple, mais un utilisateur qui vote à 23 h 50 puis 00 h 10 voit deux questions en vingt minutes et casse sa perception du rituel.
- **Europe/Paris, 5 h du matin** — la « journée » correspond à l'expérience vécue, personne n'est réveillé à la bascule. Recommandé par presque tous les produits à rituel quotidien.
- **Fuseau local de l'utilisateur** — chaque utilisateur a sa propre journée : le scrutin n'a plus de clôture commune, le jugement majoritaire n'a plus de sens, et les groupes deviennent incohérents.
- **UTC** — invisible pour l'utilisateur français, décale la bascule d'une heure selon la saison à cause du passage à l'heure d'été.

> **Reco.** `Europe/Paris`, bascule à 05 h 00 locale, stockage de tout en UTC, fuseau canonique inscrit en dur dans le schéma de données — jamais le fuseau du client.

#### Q20 — [STRUCTURANT] À quelle heure ferme le scrutin, et voit-on les résultats avant ou après ?
**Enjeu.** Si les résultats sont visibles avant la clôture, les votants tardifs sont influencés et le jugement majoritaire mesure autre chose que ce qu'on croit.
- **Résultats immédiats après son propre vote** — gratification instantanée, biais de conformité massif, et les résultats affichés à 7 h sont ceux de 200 personnes.
- **Résultats après clôture du jour** — méthodologiquement propre, mais casse la boucle de 3 minutes décrite (« résultats » fait partie de la séance).
- **Résultats immédiats mais calculés sur le scrutin de la veille clos** — l'utilisateur voit un résultat complet et stable, au prix d'un décalage d'un jour à expliquer.
- **Résultats immédiats du jour en cours, avec effectif affiché et mention « provisoire »** — honnête, préserve la boucle, laisse le biais de conformité.

> **Reco.** Résultats immédiats du jour en cours avec effectif visible et libellé « provisoire », consolidés après clôture à 05 h le lendemain — le biais est réel mais l'alternative détruit la boucle produit.

#### Q21 — [DÉTAIL] Que voit un utilisateur à Montréal, à La Réunion, ou en Nouvelle-Calédonie ?
**Enjeu.** Un public « français » inclut des expatriés et des DOM-TOM, pour qui la bascule à 5 h Paris tombe au milieu de la nuit ou en fin de journée.
- **Rien de spécial, on assume Paris** — cohérent, à condition de l'écrire dans l'app plutôt que de laisser l'utilisateur croire à un bug.
- **Affichage de l'heure de bascule dans le fuseau local** — coût quasi nul, supprime 90 % de l'incompréhension.
- **Fuseaux multiples pour les notifications uniquement** — le contenu reste synchronisé sur Paris, seul le rappel s'adapte. Bon compromis.

> **Reco.** Contenu ancré sur Paris, notification dans le fuseau local de l'appareil, et compte à rebours affiché en heure locale.

---

## 5. Le pipeline de génération de contenu

#### Q22 — [BLOQUANT] Le contenu du jour J est généré à quel moment, et avec quelle avance ?
**Enjeu.** L'avance détermine à la fois la fraîcheur de l'actualité et le temps disponible pour rattraper un échec ; sans marge, un incident à 4 h du matin est une journée sans question.
- **Génération à J-1 dans la soirée** — bonne fraîcheur, ~8 h de marge, mais rate l'actualité de la nuit.
- **Génération à J-3, relecture étalée** — grande sécurité, actualité potentiellement périmée pour un produit qui promet l'actualité.
- **Génération à J-0 au petit matin** — fraîcheur maximale, aucune marge : un échec est visible par tous.
- **Génération à J-1 le soir + régénération optionnelle à J-0 6 h si l'actualité a basculé** — le meilleur des deux, deux fois plus de pipeline à opérer.

> **Reco.** J-1 en soirée avec fenêtre de relecture jusqu'à 04 h 30, et bascule automatique sur la réserve si le contenu n'est pas validé.

#### Q23 — [BLOQUANT] Que se passe-t-il exactement à 05 h 00 si la génération a échoué ou produit un contenu inacceptable ?
**Enjeu.** C'est le seul incident qui casse le produit entièrement, et il arrivera : modèle indisponible, flux RSS en panne, quota dépassé, sortie hors format.
- **Réserve de questions pré-générées, tirée automatiquement** — mentionnée dans le cadrage, mais il faut décider de sa taille (30 jours minimum), de qui la réapprovisionne, et du fait qu'elle soit intemporelle (une question d'actualité en réserve est périmée en trois semaines).
- **Report : « pas de question aujourd'hui »** — casse la série et l'habitude, effet dévastateur sur un produit à rituel.
- **Rejouer une question d'archive** — gratuit, mais pénalise les anciens utilisateurs qui l'ont déjà vue.
- **Alerte et intervention humaine** — repose sur votre disponibilité à 5 h du matin, 365 jours par an. Ce n'est pas un plan.

> **Reco.** Réserve de 40 questions intemporelles (éthique, arbitrages de société non datés), tirage automatique sans intervention, alerte informative seulement.

#### Q24 — [BLOQUANT] Y a-t-il une relecture humaine obligatoire avant publication, et qui la fait les dimanches d'août ?
**Enjeu.** Un dossier « factuel » avec chiffres sourcés engage votre responsabilité éditoriale : une erreur factuelle ou un cadrage biaisé sur un sujet clivant est un risque juridique et réputationnel que la seconde passe IA ne couvre pas.
- **Double passe IA seule** — 20 à 40 min/jour économisées, mais vous publiez des affirmations factuelles non vérifiées sur des sujets politiques. Comptez au moins un incident sérieux par trimestre.
- **Relecture humaine obligatoire, publication bloquée sinon** — sécurité maximale, et un engagement de 20-40 min par jour, 7j/7, soit **120 à 240 heures par an** : c'est le coût caché numéro un du projet.
- **Relecture humaine sur les sujets classés « sensibles » par un filtre, automatique sinon** — réduit la charge de 60-70 %, nécessite un classifieur et accepte le risque résiduel.
- **Publication automatique + bouton de retrait immédiat + signalement utilisateur** — modèle « post-modération », le plus soutenable en solo, à condition que le retrait soit réellement instantané.

> **Reco.** Post-modération avec kill-switch en un clic et signalement utilisateur en v1, relecture a priori uniquement pour une liste de thèmes sensibles — le modèle « relecture systématique » n'est pas tenable pour une personne seule et échouera silencieusement.

#### Q25 — [STRUCTURANT] Comment sait-on, six mois plus tard, quel prompt et quel modèle ont produit une question donnée ?
**Enjeu.** Sans traçabilité, un biais systématique découvert dans les questions est indétectable et incorrigeable, et les archives rejouables deviennent inexplicables.
- **Rien de tracé** — impossible de diagnostiquer, impossible de répondre à « pourquoi vos questions sont toujours formulées comme ça ».
- **Identifiant de version de prompt + identifiant de modèle stockés avec chaque contenu** — coût nul, valeur immense.
- **Ci-dessus + archivage du prompt complet et des articles sources** — permet la reproduction et l'audit ; quelques Ko par jour, soit moins de 5 Mo par an.

> **Reco.** Prompts versionnés en fichiers dans le dépôt (jamais en base éditable à la volée), hash du prompt + identifiant de modèle + URLs sources stockés avec chaque contenu généré.

#### Q26 — [BLOQUANT] Sur quelle base juridique reprend-on le contenu de flux RSS de presse, et qu'affiche-t-on comme source ?
**Enjeu.** Le droit voisin des éditeurs de presse (transposé en droit français depuis 2019) encadre la reprise d'extraits, et le dossier factuel généré à partir d'articles de presse tombe potentiellement dedans.
- **Reformulation intégrale + lien vers la source, jamais d'extrait** — position la plus défendable, coûte une contrainte forte dans le prompt et une vérification.
- **Citation courte avec attribution** — l'exception de courte citation existe mais son périmètre est étroit et son appréciation est judiciaire.
- **Uniquement des sources publiques non-presse (INSEE, Eurostat, rapports parlementaires, data.gouv.fr)** — élimine le risque, réduit la couverture de l'actualité chaude, améliore paradoxalement la qualité factuelle.

> **Reco.** RSS uniquement pour *détecter* le sujet, chiffres tirés exclusivement de sources publiques ouvertes, aucune reprise d'extrait de presse — cela supprime un risque juridique entier pour un coût produit faible.

---

## 6. Tests, données de test, environnements

#### Q27 — [BLOQUANT] Quelles parties du code doivent être couvertes à 100 %, et lesquelles ne seront pas testées du tout ?
**Enjeu.** Un bug dans le jugement majoritaire ou dans le scoring Schwartz produit des résultats faux mais plausibles, que personne ne détectera jamais à l'œil.
- **Tout tester** — irréaliste avec le budget-heures de Q2, et dilue l'effort sur des composants d'interface qui changeront.
- **Cœur métier pur à 100 % (calcul du jugement majoritaire y compris tous les cas d'égalité, scoring Schwartz, centrage, définition du jour et bascule de fuseau), interface non testée unitairement** — concentre l'effort là où l'erreur est invisible et grave.
- **Tests de bout en bout uniquement** — attrapent les régressions de parcours, ratent les erreurs numériques.

> **Reco.** Domaine métier extrait dans un module pur sans dépendance au navigateur ni à la base, couvert à 100 % avec des cas de référence écrits à la main ; interface couverte par 3-4 parcours de bout en bout seulement.

#### Q28 — [STRUCTURANT] Comment teste-t-on un jugement majoritaire ? Avec quels cas limites de référence ?
**Enjeu.** L'algorithme a des cas de départage subtils (égalité de mention majoritaire) que personne ne découvre en production avant d'avoir publié un classement faux.
Cas de référence obligatoires à écrire avant le code : zéro votant ; un seul votant ; toutes les propositions à la même mention ; égalité parfaite entre deux propositions départagée par les proportions supérieure/inférieure ; nombre pair vs impair de votants ; effet des « sans avis » (comptent-ils dans le dénominateur — c'est une décision produit, pas technique) ; une proposition notée par 3 personnes contre une notée par 3 000.
- **Cas écrits à la main, résultats calculés indépendamment sur papier** — lent mais fiable, environ 20 cas suffisent.
- **Comparaison contre une implémentation de référence tierce** — rapide si une existe, dépendance externe.
- **Tests basés sur des propriétés (l'ordre ne dépend pas de la permutation des votants, l'ajout d'un votant unanime ne dégrade pas une proposition)** — attrape les bugs auxquels on n'a pas pensé.

> **Reco.** 20 cas de référence à la main + 3 tests de propriété : c'est deux jours de travail qui protègent la crédibilité entière du produit.

#### Q29 — [BLOQUANT] Comment teste-t-on un pipeline dont la sortie n'est pas déterministe ?
**Enjeu.** Comparer une sortie de modèle à une chaîne attendue produit une suite de tests qui échoue au hasard, qu'on finit par désactiver — et le pipeline n'est alors plus testé du tout.
- **Tester le contrat, pas le contenu** — schéma de sortie valide, 4 propositions exactement, 4 justifications, longueurs dans les bornes, sources présentes et URLs joignables, aucun champ vide. Déterministe, rapide, en intégration continue.
- **Réponses de modèle enregistrées et rejouées** — le pipeline est testé de bout en bout sans appel réseau ni coût ; il faut rafraîchir les enregistrements périodiquement.
- **Évaluation par un second modèle (« LLM juge ») sur un jeu de 20 sujets** — mesure la qualité (neutralité du cadrage, équilibre des camps), à lancer manuellement à chaque changement de prompt, pas en intégration continue.
- **Revue humaine d'un échantillon hebdomadaire** — le seul moyen de détecter une dérive de cadrage politique ; 15 min/semaine.

> **Reco.** Contrat de sortie validé en CI + enregistrements rejoués + évaluation par juge sur un jeu figé de 20 sujets lancée à chaque modification de prompt, avec seuil de régression explicite.

#### Q30 — [BLOQUANT] Avec quelles données développe-t-on, sachant qu'on ne peut pas copier la production ?
**Enjeu.** Les votes sont des opinions politiques nominatives : les copier en environnement de développement est une violation caractérisée, et développer sans données réalistes rend le profil de valeurs et les résultats invérifiables.
- **Générateur de données synthétiques avec profils simulés** — des « personas » aux valeurs Schwartz définies a priori qui votent de façon cohérente. Permet de vérifier que le scoring retrouve bien les profils injectés : c'est aussi le meilleur test du scoring.
- **Production anonymisée** — l'anonymisation d'opinions politiques corrélées est extrêmement difficile à faire correctement ; à éviter.
- **Base vide et saisie manuelle** — impraticable au-delà de trois jours de développement.
- **Volontaires explicitement consentants sur un environnement de recette dédié** — utile pour la recette finale, pas pour le développement quotidien.

> **Reco.** Générateur de personas synthétiques versionné dans le dépôt, avec une graine fixe, servant à la fois de jeu de développement et de test de validation du scoring — aucune donnée de production ne quitte jamais la production.

---

## 7. RGPD et données sensibles

#### Q31 — [BLOQUANT] Quelle est la base légale du traitement d'opinions politiques ?
**Enjeu.** Les opinions politiques relèvent de l'article 9 du RGPD : sans base valable, le traitement est illicite dans son intégralité, quelle que soit la qualité technique.
- **Consentement explicite (art. 9.2.a)** — la seule base réaliste ici. Impose un consentement distinct, actif, non pré-coché, spécifique, révocable à tout moment, et une preuve horodatée du consentement.
- **Intérêt légitime** — inapplicable aux données de l'article 9. Écarter.
- **Données rendues manifestement publiques par la personne (art. 9.2.e)** — ne couvre pas des votes privés dans une app.

> **Reco.** Consentement explicite, recueilli séparément du reste de l'onboarding, avec horodatage et version des conditions stockés — et un parcours de retrait qui supprime réellement.

#### Q32 — [BLOQUANT] Une analyse d'impact (AIPD) est-elle requise, et qui la rédige ?
**Enjeu.** Le traitement à grande échelle de données de l'article 9, avec profilage et évaluation d'aspects personnels, coche plusieurs critères qui rendent l'AIPD obligatoire ; son absence est un manquement autonome, sanctionnable indépendamment de toute fuite.
- **Pas d'AIPD, on verra plus tard** — le « plus tard » arrive au premier signalement CNIL, et l'AIPD doit être antérieure au traitement.
- **AIPD rédigée en interne avec le modèle et le logiciel gratuits de la CNIL** — 30 à 60 heures de travail réel, gratuit, tout à fait faisable et formateur pour le cadrage.
- **AIPD externalisée** — 4 000 à 12 000 € selon le cabinet.

> **Reco.** AIPD en interne avec l'outil PIA de la CNIL, commencée maintenant : elle vous forcera à répondre à la moitié des questions de ce document.

#### Q33 — [BLOQUANT] Comment les votes restent-ils anonymes dans un groupe de 4 personnes où les profils de valeurs sont visibles ?
**Enjeu.** « Votes anonymes, profils visibles » est mathématiquement contradictoire dans les petits groupes : avec 4 membres, deux jours d'observation suffisent à réattribuer chaque vote.
- **Seuil minimum de membres pour afficher les résultats du groupe (ex. 8)** — simple, efficace, frustre les petits groupes qui sont pourtant le cas d'usage naturel.
- **Seuil minimum de votants sur la question (ex. 5) avant tout affichage** — plus fin, et évite le cas du groupe de 20 où seules 3 personnes ont voté.
- **Renoncer à la promesse d'anonymat en groupe et l'assumer** — cohérent avec « profils visibles », mais il faut le dire explicitement avant l'invitation.
- **Résultats agrégés uniquement, jamais de granularité individuelle, plus bruit différentiel** — techniquement le plus solide, complexité forte pour une v2.

> **Reco.** Seuil de 5 votants minimum par question avant affichage, formulation honnête dans l'app (« non attribué » plutôt que « anonyme »), et décision du schéma de données dès la v1 même si les groupes arrivent en v2.

#### Q34 — [STRUCTURANT] Quelles métriques produit collecte-t-on, et laquelle abandonne-t-on au nom de la minimisation ?
**Enjeu.** Le conflit est direct : « quelles valeurs corrèlent avec l'abandon en semaine 3 » est la question produit la plus utile et la plus intrusive du projet.
- **Analytics tiers classique** — riche, et vous transmettez des parcours corrélés à des opinions politiques à un tiers ; incompatible avec la promesse et probablement avec la base légale.
- **Compteurs agrégés serveur uniquement (nombre de votes/jour, taux de complétion de la boucle, taux d'abandon par étape), sans identifiant utilisateur** — couvre 80 % du besoin produit, coût nul, aucun risque.
- **Analytics auto-hébergé sans cookie ni identifiant persistant** — un cran au-dessus, quelques dizaines d'euros par mois, à documenter dans l'AIPD.
- **Cohortes avec identifiant pseudonyme** — permet l'analyse de rétention fine ; toute jointure entre cette table et les votes recrée une donnée de l'article 9.

> **Reco.** Compteurs agrégés serveur, séparés physiquement de la base de votes, avec interdiction architecturale de jointure — et renoncement assumé à l'analyse de rétention par profil de valeurs.

---

## 8. Authentification sans email ni mot de passe

#### Q35 — [BLOQUANT] Sans email ni mot de passe, comment un utilisateur récupère-t-il son compte après avoir cassé son téléphone ?
**Enjeu.** Si la réponse est « il ne peut pas », vous perdez définitivement l'historique de valeurs qui est la valeur accumulée du produit, et l'utilisateur avec.
- **Compte lié à l'appareil uniquement** — zéro friction, zéro donnée personnelle, et perte totale au changement d'appareil ou à la purge du stockage iOS. Inacceptable pour un produit dont la promesse est l'accumulation.
- **Passkeys (WebAuthn) synchronisées via le trousseau iCloud / Google** — pas de mot de passe, récupération assurée par la plateforme, aucune donnée personnelle chez vous. Support navigateur bon depuis 2023. La meilleure option technique, mais le concept reste opaque pour le grand public.
- **Phrase de récupération à noter (12 mots ou code alphanumérique)** — fonctionne partout, et 80 % des utilisateurs ne la noteront pas ; ceux qui la perdent perdent tout, sans recours possible pour vous.
- **Email optionnel proposé après 10 jours d'usage** — l'utilisateur a alors quelque chose à protéger et comprend l'intérêt. Réintroduit une donnée personnelle, mais une seule, non sensible, et facultative.

> **Reco.** Passkey en méthode principale, code de récupération affiché une fois en secours, et proposition d'email facultatif après la première semaine — le triptyque couvre les trois profils d'utilisateurs.

#### Q36 — [BLOQUANT] Dit-on à l'utilisateur, avant qu'il investisse trois mois de votes, ce qu'il perdra s'il perd l'accès ?
**Enjeu.** Un utilisateur qui perd un an d'historique sans avoir été prévenu écrira publiquement que l'app lui a « volé » ses données.
- **Rien** — engendre une colère justifiée et des avis destructeurs.
- **Mention dans les CGU** — juridiquement suffisant, moralement insuffisant, pratiquement inutile.
- **Rappel actif après 10, 30 et 90 votes tant qu'aucun moyen de récupération n'est configuré** — le seul dispositif qui fonctionne réellement.

> **Reco.** Relance contextualisée avec le compte de votes accumulés (« vous avez 47 votes qui ne survivront pas à un changement de téléphone »), non ignorable au-delà de 30 votes.

#### Q37 — [STRUCTURANT] Un même compte peut-il être actif sur deux appareils simultanément ?
**Enjeu.** Le multi-appareil implique une résolution de conflits (Q11) et interdit certaines simplifications d'implémentation prises « parce que c'est un seul appareil ».
- **Non, un appareil actif à la fois** — simple, frustrant pour l'utilisateur téléphone + ordinateur.
- **Oui, sans restriction, serveur source de vérité** — coût faible si Q13 est déjà « serveur », impose l'idempotence de Q11.
- **Oui, avec limite de N appareils** — complexité de gestion des sessions pour un bénéfice marginal.

> **Reco.** Multi-appareil libre : c'est gratuit si le serveur est source de vérité, et coûteux à rétrofitter sinon.

---

## 9. Hébergement, coûts, paliers d'échelle

#### Q38 — [STRUCTURANT] Quel hébergeur, et accepte-t-on des services managés d'éditeurs américains même avec des serveurs en Europe ?
**Enjeu.** Pour des données de l'article 9, la localisation des serveurs ne suffit pas : un éditeur soumis au CLOUD Act reste un point de friction dans l'AIPD et un argument de défiance pour un public sensible à la souveraineté.
- **Éditeur français ou européen intégral (Scaleway, OVHcloud, Clever Cloud, Exoscale, Hetzner)** — cohérent avec le positionnement, écosystème et confort de développement en retrait par rapport aux plateformes américaines.
- **Plateforme américaine avec région UE** — meilleur confort de développement, à documenter explicitement dans l'AIPD, et fragilise le discours si le projet en fait un argument.
- **Mixte : données sur infrastructure européenne, contenu statique sur CDN américain** — pragmatique et défendable si aucune donnée personnelle ne transite par le CDN.

> **Reco.** Base de données et application chez un éditeur européen, CDN au choix pour les seuls actifs statiques, et l'appel au modèle de langage documenté comme sous-traitant distinct.

#### Q39 — [STRUCTURANT] Quel est le coût mensuel réaliste à 100, 10 000 et 500 000 utilisateurs ?
**Enjeu.** Sans ces ordres de grandeur, on optimise des choses gratuites et on ignore ce qui coûte.

| Poste | 100 utilisateurs | 10 000 | 500 000 |
|---|---|---|---|
| Calcul + base | 15-25 € | 60-120 € | 800-2 500 € |
| CDN / bande passante | ~0 € | 5-20 € | 150-600 € |
| Génération IA du contenu | **3-8 €** | **3-8 €** | **3-8 €** |
| Push (Web Push) | 0 € | 0 € | 0 € (mais ~5,8 M pushs/mois à ordonnancer) |
| Sauvegardes / stockage | ~2 € | 10 € | 100-300 € |
| Observabilité | 0 € | 0-30 € | 100-400 € |
| **Total infrastructure** | **~25 €** | **~100-180 €** | **~1 200-3 800 €** |

Le point contre-intuitif : **le coût de l'IA est constant et négligeable** (une génération par jour, partagée par tous), alors que le réflexe est de le craindre. Ce qui coûte, c'est le pic de trafic et le travail humain.

- **Base gérée manuellement sur un VPS** — divise l'infrastructure par 2 à 3, multiplie par 5 le temps d'exploitation.
- **Tout managé** — le tableau ci-dessus.
- **Sans serveur / à la demande** — quasi gratuit à 100, potentiellement plus cher à 500 000 avec un pic concentré.

> **Reco.** Budgéter 80 €/mois pour les 18 premiers mois et considérer que le coût de l'IA n'est pas un sujet — le sujet est le pic.

#### Q40 — [STRUCTURANT] À quel palier précis l'architecture doit-elle changer, et quel indicateur le déclenche ?
**Enjeu.** Ce qui casse n'est pas le volume quotidien mais le pic : si tout le monde est notifié dans la même tranche horaire, 500 000 utilisateurs produisent des dizaines de milliers de requêtes par minute sur dix minutes, avec une base à 6 écritures/seconde en moyenne.
- **Jusqu'à ~5 000 utilisateurs** — un serveur, une base, la question du jour servie dynamiquement. Rien à faire.
- **~5 000 à ~50 000** — la question du jour et le dossier factuel doivent devenir des fichiers statiques servis par CDN (le contenu est identique pour tous : c'est le levier le plus rentable du projet). Les résultats agrégés passent en table matérialisée rafraîchie toutes les N minutes plutôt que calculés à la volée.
- **Au-delà de ~50 000** — écritures de votes en file d'attente avec agrégation asynchrone, notifications étalées sur plusieurs minutes avec jitter, réplique en lecture.
- **Indicateur déclencheur** — latence au 95e centile de l'endpoint de résultats, et pic de connexions simultanées à la base.

> **Reco.** Servir le contenu du jour en statique depuis le CDN **dès la v1** : c'est le seul choix d'échelle qui ne coûte rien maintenant et évite une refonte à 20 000 utilisateurs.

---

## 10. Accessibilité technique

#### Q41 — [BLOQUANT] Comment une échelle à 5 mentions plus « sans avis » se présente-t-elle à un lecteur d'écran ?
**Enjeu.** C'est le geste central de l'app, répété 4 fois par jour ; s'il est inutilisable au clavier ou avec VoiceOver, l'application entière est inaccessible, et le composant sera trop enraciné pour être refait.
- **Curseur (slider)** — visuellement séduisant, désastreux en lecteur d'écran (valeurs numériques sans libellé) et au doigt tremblant ; à écarter.
- **Groupe de 6 boutons radio natifs avec libellés textuels, regroupés par `fieldset`/`legend` portant l'intitulé de la proposition** — navigation aux flèches native, annonce correcte de « 3 sur 6 », gratuit. Contrainte : la mise en page doit tenir 6 cibles de 44 px sur un écran de 360 px, donc probablement en colonne.
- **Boutons personnalisés avec `role="radiogroup"`** — liberté visuelle totale, et l'obligation de réimplémenter à la main la gestion du focus, des flèches et de l'index de tabulation.

> **Reco.** Boutons radio natifs stylés en CSS, disposition verticale sur mobile, libellé textuel de la mention toujours visible — la couleur ne doit jamais être le seul porteur de sens.

#### Q42 — [STRUCTURANT] Quel niveau d'accessibilité vise-t-on, qui le vérifie, et le graphe de valeurs a-t-il une alternative textuelle ?
**Enjeu.** Le profil Schwartz sera probablement un radar à 10 axes : c'est l'écran le plus important du produit et le moins accessible qui soit.
- **RGAA/WCAG AA visé, vérification par outil automatique seul** — attrape 30 % des problèmes réels, laisse passer tous ceux qui comptent ici.
- **AA visé, avec tableau de données alternatif systématique pour toute visualisation** — un tableau « valeur / score / évolution » consultable, qui sert aussi les utilisateurs voyants. Coût : un composant.
- **Audit externe** — 3 000 à 6 000 €, pertinent avant une communication publique large, pas avant.

> **Reco.** AA visé, tableau alternatif obligatoire pour toute visualisation dès la conception, vérification manuelle avec VoiceOver sur les 5 écrans de la boucle quotidienne — les autres écrans peuvent attendre.

---

## 11. Performance

#### Q43 — [STRUCTURANT] Sur quel appareil et quel réseau de référence mesure-t-on, et quel est le budget chiffré ?
**Enjeu.** Une promesse de « rituel de 3 minutes » où 15 secondes partent en chargement perd 8 % du rituel et casse l'habitude ; sans budget chiffré, la dérive est invisible car vous développez sur un appareil rapide.
- **Pas de budget** — l'app deviendra lente par accumulation, sans qu'aucun commit ne soit coupable.
- **Budget chiffré et mesuré en intégration continue** — appareil de référence type milieu de gamme Android d'il y a 4 ans, réseau 4G dégradé (~1,5 Mbit/s, 150 ms de latence). Cibles : premier affichage utile < 2,5 s, JavaScript initial < 150 Ko compressé, interaction possible < 3,5 s, aucune image du dossier factuel au-dessus de 100 Ko.
- **Mesure manuelle occasionnelle** — mieux que rien, dérive quand même.

> **Reco.** Budget chiffré vérifié automatiquement à chaque intégration avec échec du build au dépassement — c'est le seul mécanisme qui tient dans la durée.

#### Q44 — [DÉTAIL] Le dossier factuel contient-il des images, et sous quelle contrainte ?
**Enjeu.** Un graphique en image générée par le pipeline peut à lui seul dépasser tout le budget de performance et être inaccessible.
- **Aucune image, chiffres en texte structuré** — le plus rapide, le plus accessible, le plus sobre, et suffisant pour « chiffres sourcés ».
- **Graphiques en SVG généré côté serveur** — léger, net sur tout écran, stylable selon le thème, accessible avec une description textuelle.
- **Images bitmap** — poids, coût de stockage, alternative textuelle à produire par le pipeline.

> **Reco.** Aucune image bitmap ; chiffres en texte et, si besoin, SVG minimal accompagné d'un tableau.

---

## 12. Ouverture du code

#### Q45 — [STRUCTURANT] Ouvre-t-on le code, et publie-t-on les prompts et la formule de scoring ?
**Enjeu.** L'ouverture est un argument de crédibilité majeur pour un produit qui prétend mesurer objectivement des valeurs, mais elle est irréversible et expose la mécanique du jeu.
- **Tout ouvert, prompts et scoring inclus** — crédibilité maximale, cohérente avec le positionnement. Conséquences concrètes : aucun secret dans le dépôt **ni dans l'historique git** (une clé d'API publiée une fois est compromise pour toujours, même après suppression du commit) ; le scoring devient optimisable par l'utilisateur, mais c'est un faux problème puisqu'il n'y a pas de classement à gagner ; les prompts publiés permettent à quiconque de reproduire votre pipeline, ce qui est le prix de la confiance.
- **Code ouvert, prompts fermés** — position bâtarde : la partie que les gens veulent auditer est justement le cadrage éditorial.
- **Scoring publié en documentation, code fermé** — donne 80 % de la crédibilité pour 0 % du risque, et n'engage pas à maintenir un projet public.
- **Fermé** — aucun coût, et vous perdez le principal argument de confiance sur un sujet où la confiance est tout.

> **Reco.** Méthodologie de scoring et prompts publiés en documentation dès la v1, ouverture du code après stabilisation — avec, dès le premier commit, une hygiène de secrets de projet ouvert (variables d'environnement, aucun secret versionné, détection automatique de secrets à chaque commit).

---

## Portes à sens unique

Décisions sur lesquelles on ne peut pas revenir sans réécrire, et le moment le plus tardif où on peut encore les prendre.

| # | Décision | Pourquoi c'est irréversible | Dernier moment |
|---|---|---|---|
| 1 | **Granularité de ce qu'on stocke pour un vote** (Q14) | Ce qui n'a pas été enregistré n'existe pas : aucun recalcul, aucune archive rejouable, aucune correction ne sera jamais possible sur les votes déjà passés. | **Avant le premier vote réel.** |
| 2 | **Présence d'un `scoring_version` sur chaque vote** (Q14, Q16) | Ajouter la colonne est facile, deviner rétroactivement quelle version a produit quel score est impossible. | **Avant le premier vote réel.** |
| 3 | **Fuseau et heure canoniques du « jour »** (Q19) | Changer la définition du jour redécoupe tout l'historique, les séries, les scrutins et les bilans hebdomadaires. | **Avant la première question publiée.** |
| 4 | **Le domaine métier (jugement majoritaire, scoring, centrage) est-il un module pur sans dépendance au navigateur ni à la base ?** (Q27) | C'est ce qui rend possible un passage au natif, un changement de framework, et les tests. Rétrofitter la séparation sur du code couplé, c'est réécrire. | **Premier jour de développement.** |
| 5 | **Base légale et périmètre du consentement** (Q31) | Élargir un traitement au-delà de ce qui a été consenti impose de recontacter et re-consentir toute la base — impossible sans email (Q35). | **Avant la première collecte.** |
| 6 | **Le lien utilisateur ↔ vote est-il conservé, et sous quelle forme ?** (Q18, Q33) | Si le lien est stocké en clair, promettre l'anonymat plus tard est un mensonge ; s'il n'est pas stocké, le profil personnel et les archives rejouables sont impossibles. Le schéma doit être posé maintenant, même si les groupes arrivent en v2. | **Avant le premier vote réel.** |
| 7 | **Modèle d'identité et de récupération** (Q35) | Des comptes créés sans facteur de récupération ne peuvent pas en recevoir un rétroactivement : on ne sait pas à qui ils appartiennent. Chaque jour d'attente crée des comptes définitivement irrécupérables. | **Avant l'ouverture publique.** |
| 8 | **Premier commit public / historique git** (Q45) | On peut ouvrir un code fermé ; on ne peut pas refermer un code ouvert, ni dé-publier un secret entré dans l'historique. | **Avant le premier dépôt public.** |
| 9 | **Contenu du jour servi en statique depuis un CDN** (Q40) | Gratuit à mettre en place au départ, c'est une refonte de l'architecture de livraison une fois le trafic là. | **Avant 5 000 utilisateurs.** |
| 10 | **Composant d'échelle de mentions** (Q41) | Répété 4 fois par jour et enraciné dans tous les écrans (vote, archives, onboarding, groupes) : le refaire, c'est refaire l'app. | **Avant le premier écran de vote livré.** |

---

## Ce qui coûtera beaucoup plus cher que prévu

Par ordre décroissant de sous-estimation.

**1. La relecture éditoriale quotidienne — 120 à 240 h/an.** 20 à 40 minutes par jour, 7 jours sur 7, y compris les dimanches d'août et les jours de grippe. C'est de loin le poste le plus lourd du projet et il n'apparaît nulle part dans un plan technique. Une personne seule tiendra environ 6 à 10 semaines. Corollaire : la conception doit rendre la publication **automatique par défaut** avec retrait a posteriori (Q24), pas l'inverse.

**2. Les « archives entièrement rejouables ».** Cette ligne du cadrage est présentée comme une fonctionnalité et c'est en réalité une contrainte d'architecture globale : elle impose de figer et versionner le contenu, le modèle de scoring, la formule d'agrégation, les libellés de mentions et les résultats d'époque, puis de faire cohabiter plusieurs versions dans le même écran. Comptez un facteur **2 à 3 sur le coût de développement de la boucle de vote**, soit 150 à 250 heures supplémentaires. C'est la fonctionnalité à repousser en premier.

**3. La conformité RGPD article 9 — 40 à 80 h en interne, ou 5 000 à 15 000 € externalisée.** AIPD, registre des traitements, politique de confidentialité rédigée pour de vrai, procédures d'exercice des droits (accès, portabilité, effacement) qui doivent être *implémentées*, pas seulement promises, mentions de consentement versionnées. Non négociable et souvent découvert trois semaines avant le lancement.

**4. Les notifications à heure choisie (Q5).** Ce qui ressemble à une case de réglage est un ordonnanceur distribué : file, reprise sur échec, désabonnements expirés (20 à 30 % par an), pushs individualisés, étalement du pic. Comptez **60 à 100 heures** et un service à surveiller en permanence. Les créneaux fixes coûtent 8 heures.

**5. L'accessibilité faite correctement — 60 à 120 h.** L'échelle de mentions accessible, le radar Schwartz avec alternative tabulaire, la navigation clavier complète, le respect des tailles de police système sur un écran dense, le mode mouvement réduit. Un audit externe ajoute 3 000 à 6 000 €. Fait après coup, c'est deux à trois fois plus cher.

**6. Le support et la modération dès l'apparition des groupes.** À 10 000 utilisateurs, 1 à 3 % écrivent, soit 100 à 300 messages à traiter, majoritairement « j'ai perdu mon compte » (Q35). Aucun outil ne les fait disparaître ; il faut du temps humain ou une politique explicite de non-réponse.

**7. Ce qui coûtera *moins* cher que prévu — la génération IA.** Un contenu par jour partagé par tout le monde : 3 à 8 €/mois, constants quelle que soit l'audience. Ne pas y consacrer d'effort d'optimisation. Le budget d'attention doit aller au pic de trafic et au temps humain.
