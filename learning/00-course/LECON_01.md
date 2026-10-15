# Leçon 1 — L'écosystème STM32 et lecture du code généré

**Durée** : environ 1 h 30 pour la partie pratique, plus 30 à 45 minutes pour le réflexe documentation (exercices C, D et E possibles à une autre session) · **Prérequis** : début de la playlist ST (vidéo 3 vue), CubeIDE installé, Blue Pill + ST-Link V2, et la lecture de [`GUIDE_LIRE_UNE_DATASHEET.md`](GUIDE_LIRE_UNE_DATASHEET.md) (sections 0 à 4).

## Objectifs

À la fin de cette leçon, tu sauras :
1. Situer chaque partie d'un STM32 grâce à une carte mentale.
2. Vérifier ta chaîne d'outils (Blue Pill, ST-Link, SWD).
3. Lire un projet généré par CubeMX et comprendre le rôle de chaque fichier.
4. Observer un registre GPIO changer pendant l'exécution, avec le débogueur.
5. 📖 Retrouver une information dans une datasheet et dans un reference manual, et la noter avec sa source.

Ton dépôt contient déjà des projets HAL. Cette leçon ne te fait donc pas refaire un clignotement : elle t'apprend à **regarder sous le capot** de ce que tu as déjà écrit.

---

## 1. La carte mentale : le STM32 est une petite ville

| Dans la ville | Dans le STM32F103 | Rôle |
|---|---|---|
| Le maire | Le cœur **Cortex-M3** (jusqu'à 72 MHz) | Exécute les instructions |
| Les archives (permanentes) | La **Flash** (à partir de `0x08000000`) | Contient ton programme, survit à la coupure |
| Le bureau de travail (effacé le soir) | La **SRAM** (à partir de `0x20000000`) | Variables, pile |
| Les quartiers spécialisés | Les **périphériques** (GPIO, timers, ADC, UART…) à partir de `0x40000000` | Chacun fait un métier |
| Les autoroutes | Les **bus** (AHB, APB1, APB2) | Relient tout le monde |
| Le cœur qui bat | L'**horloge** (HSI 8 MHz par défaut au démarrage) | Rythme tout |
| Les interrupteurs des quartiers | Les **registres** | Tu configures et tu commandes avec eux |

**Retiens ceci** : au démarrage, **la plupart des quartiers sont éteints** (leur horloge est coupée). Pour utiliser un périphérique, il faut d'abord **allumer son horloge**. C'est l'erreur de débutant n°1 en accès direct aux registres, et elle sera au cœur de la leçon 3. HAL le fait pour toi dans `MX_GPIO_Init()` : tu vas le voir.

## 2. Vérification matérielle

La Blue Pill se programme en **SWD** avec le ST-Link V2 : 4 fils.

| ST-Link V2 | Blue Pill |
|---|---|
| SWCLK | SWCLK (PA14) |
| SWDIO | SWDIO (PA13) |
| GND | GND |
| 3.3V | 3.3V |

**Contrôle** :
1. Jumper `BOOT0` côté 0 (démarrage depuis la Flash).
2. Ouvre **ST-LINK Utility** → *Target → Connect*.
3. Note dans `JOURNAL.md` le **Device ID**, la taille de Flash et le marquage de la puce.

Attention : beaucoup de Blue Pill portent des clones (CKS32, GD32…). Ils fonctionnent généralement, mais l'identifiant ou la taille de Flash peuvent différer, et CubeIDE peut afficher un avertissement d'ID. Ce que tu lis est une information utile à noter.

## 3. Pratique : lire un vrai projet généré

Utilise ton projet `learning/01-gpio/blink-button-debounce` (LED sur PB9, bouton sur PB6).

1. Ouvre-le dans CubeIDE (`File → Open Projects from File System…`).
2. Compile, flashe, vérifie le comportement.
3. Ouvre `Core/Src/main.c` et trouve `MX_GPIO_Init()`.
4. Dans cette fonction, repère la ligne qui **allume l'horloge du port B** (`__HAL_RCC_GPIOB_CLK_ENABLE()`), puis celle qui configure la broche. Tu viens de voir, en HAL, la règle des « quartiers éteints ».
5. Ouvre le `.ioc` : retrouve les mêmes réglages dans l'interface graphique.

Rappel : tout code ajouté hors des zones `USER CODE BEGIN/END` est **écrasé** à la prochaine génération.

## 4. Les fichiers générés

| Fichier | Rôle (analogie) |
|---|---|
| `Core/Src/main.c` | Le programme principal, le « bureau du maire » |
| `Core/Startup/startup_stm32f103c8tx.s` (le nom peut varier) | La **procédure d'ouverture du matin** : pile, table des vecteurs, copie des données, appel de `main` |
| `STM32F103C8TX_FLASH.ld` | Le **plan cadastral** : où placer code et variables en mémoire |
| `Core/Src/system_stm32f1xx.c` | Réglages de base de l'horloge système |
| `Core/Src/stm32f1xx_hal_msp.c` | Réglages bas niveau appelés par HAL |
| `Drivers/` | HAL et CMSIS, le code fourni par ST |

Ouvre-les sans chercher à tout comprendre. La leçon 2 les décortique.

## 5. Défis

- ⭐ Dans `main.c`, change la période lente de la LED (500 ms) en 1000 ms. Recompile, reflashe, observe.
- ⭐⭐ Dans le fichier de démarrage, trouve le **nom de la fonction appelée au reset** et la ligne où `main` est appelé.
- ⭐⭐⭐ Lance le **débogueur**, place un point d'arrêt dans la boucle principale, ouvre la vue **Registers** et observe `GPIOB → ODR`. **Prédis d'abord** quel bit va changer quand la LED change d'état (indice : le numéro du bit dépend de la broche utilisée). Vérifie ensuite.

## 6. 📖 Réflexe documentation

Télécharge **DS5319** (datasheet) et **RM0008** (reference manual) depuis les liens du guide, et note la **révision** de chaque PDF dans le journal. Je ne te donne pas les réponses : tu cherches, tu me montres, nous vérifions. Si tu bloques après 10 minutes, demande « indice » pour recevoir l'indice suivant (voir l'échelle dans le guide).

Écris chaque réponse dans la **fiche de lecture** du `JOURNAL.md`, avec son unité, sa source (section, table, page) et ses conditions.

### Exercice A — L'identité de ta puce (DS5319) · niveau 1
1. Sur la première page, relève la quantité de Flash, la quantité de SRAM et la fréquence maximale de la famille.
2. Compare avec la taille de Flash lue par ST-LINK Utility sur **ta** carte, et avec le marquage de la puce. Ta puce correspond-elle ? Un écart est une information à noter, pas une panne.

### Exercice B — Tes broches (DS5319) · niveau 1 à 2
Ton projet utilise **PB9** (LED) et **PB6** (bouton).
1. Dans le tableau des définitions de broches, trouve les lignes de PB9 et PB6. Relève leur niveau d'E/S et leurs fonctions alternatives.
2. Une abréviation apparaît dans la colonne du niveau d'E/S. Retrouve sa signification dans la légende du tableau.
3. Explique pourquoi cette information compte si un jour tu connectes un signal en 5 V.

### Exercice C — Dimensionner une LED (DS5319 + documentation de ta LED) · niveau 3
1. Relève le courant maximal par broche dans les valeurs absolues maximales. Explique, avec le guide (section 4), pourquoi on ne dimensionne **pas** sur cette valeur.
2. Dans les caractéristiques électriques, trouve les tensions de sortie garanties (bas et haut) et les courants pour lesquels elles sont garanties.
3. Relève la tension directe Vf de ta LED pour un courant donné (datasheet, ou mesure au multimètre si tu n'as pas la référence). Calcule la résistance série pour une sortie en 3,3 V et un courant que tu choisis et **justifies** avec les deux documents.
4. Compare avec la résistance réellement câblée sur ton montage.

### Exercice D — Retrouver ce que fait HAL (RM0008) · niveau 2
1. Dans le chapitre sur le RCC, trouve le registre qui active l'horloge des périphériques de l'APB2, puis le bit qui active le port B.
2. Dans CubeIDE, place le curseur sur `__HAL_RCC_GPIOB_CLK_ENABLE()` et appuie sur `F3` (Open Declaration) jusqu'à la ligne qui écrit dans le registre. Compare avec ce que tu as lu dans le manuel.
3. Trouve l'adresse de base du GPIOB et l'offset du registre ODR, puis calcule l'adresse de `GPIOB->ODR`. Vérifie-la dans la vue **Registers** du débogueur (défi ⭐⭐⭐).

### Exercice E — Un autre composant · niveau 1 à 3
Choisis un composant de ton montage : LED, transistor NPN, photorésistance, potentiomètre ou écran. Télécharge sa documentation sur le site du fabricant et applique les **7 questions** du guide (section 5), avec une ligne de fiche de lecture par réponse. **Dis-moi la référence exacte** (le marquage), et je t'indique quel document ouvrir si tu ne le trouves pas.

### Bonus portfolio
Ajoute une section **References** au README de l'un de tes projets, en suivant [`docs/PROJECT_README_TEMPLATE.md`](../../docs/PROJECT_README_TEMPLATE.md) : identifiant du document, révision et section utilisée. Un recruteur y voit que tu documentes l'origine de tes valeurs.

## 7. Contrôle de compréhension

À répondre dans `JOURNAL.md`, avec tes propres mots :
1. Pourquoi faut-il activer l'horloge d'un port GPIO avant de l'utiliser ?
2. Pourquoi ne pas écrire son code en dehors des marqueurs `USER CODE` ?
3. Quelle est la différence entre Flash et SRAM, avec l'analogie de la ville ?
4. Sur la Blue Pill, la LED embarquée est sur **PC13** et s'allume à **niveau bas**. Qu'est-ce que cela change dans le code par rapport à une LED câblée à la masse ?
5. 📖 Quelle est la différence entre les valeurs absolues maximales et les conditions de fonctionnement recommandées ? Que risques-tu en dimensionnant sur les premières ?
6. 📖 Pour savoir quel bit active l'horloge d'un port, quel document ouvres-tu : la datasheet, le reference manual ou le programming manual ? Pourquoi ?
7. 📖 Pourquoi ne dimensionne-t-on pas un circuit sur la valeur « typ » d'une datasheet ?

## Prochaine leçon

**Leçon 2 — Mémoire et démarrage** : nous ouvrons la procédure d'ouverture du matin ligne par ligne et nous suivons ce qui se passe entre le reset et le premier `main()`. 📖 Documents : RM0008 (carte mémoire, configuration du boot) et PM0056 (exceptions, table des vecteurs).
