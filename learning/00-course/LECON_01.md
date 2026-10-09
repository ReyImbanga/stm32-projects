# Leçon 1 — L'écosystème STM32 et lecture du code généré

**Durée** : environ 1 h 30 · **Prérequis** : début de la playlist ST (vidéo 3 vue), CubeIDE installé, Blue Pill + ST-Link V2.

## Objectifs

À la fin de cette leçon, tu sauras :
1. Situer chaque partie d'un STM32 grâce à une carte mentale.
2. Vérifier ta chaîne d'outils (Blue Pill, ST-Link, SWD).
3. Lire un projet généré par CubeMX et comprendre le rôle de chaque fichier.
4. Observer un registre GPIO changer pendant l'exécution, avec le débogueur.

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

## 6. Contrôle de compréhension

À répondre dans `JOURNAL.md`, avec tes propres mots :
1. Pourquoi faut-il activer l'horloge d'un port GPIO avant de l'utiliser ?
2. Pourquoi ne pas écrire son code en dehors des marqueurs `USER CODE` ?
3. Quelle est la différence entre Flash et SRAM, avec l'analogie de la ville ?
4. Sur la Blue Pill, la LED embarquée est sur **PC13** et s'allume à **niveau bas**. Qu'est-ce que cela change dans le code par rapport à une LED câblée à la masse ?

## Prochaine leçon

**Leçon 2 — Mémoire et démarrage** : nous ouvrons la procédure d'ouverture du matin ligne par ligne et nous suivons ce qui se passe entre le reset et le premier `main()`.
