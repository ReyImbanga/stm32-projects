# Plan de cours STM32

Le plan suit le **plan officiel du MOOC ST** (5 volets). Les titres exacts des vidéos ne figurent pas sur la page du cours : tu les notes dans `JOURNAL.md` au fil du visionnage et nous ajustons le découpage.

**Principe 1 — Sous HAL.** Le dépôt contient déjà des projets en HAL. Chaque leçon vise donc à comprendre ce qu'il y a **sous** HAL, puis à le refaire plus bas.

**Principe 2 — 📖 Réflexe documentation.** Chaque leçon et chaque module indique **quel document ouvrir** pour répondre aux questions du jour. Je te donne l'adresse (document, chapitre), tu trouves la réponse, nous vérifions ensemble. La méthode, valable pour tout composant et tout outil, est dans [`GUIDE_LIRE_UNE_DATASHEET.md`](GUIDE_LIRE_UNE_DATASHEET.md).

---

## Phase 0 — Préparation (fait / en cours)

- Carte : Blue Pill STM32F103C8T6. Programmateur : ST-Link V2. ✅
- Vérifier la connexion SWD (voir `docs/setup.md`).
- Noter dans le journal ce que renvoie l'outil (Device ID, taille Flash, éventuel clone).
- Télécharger **DS5319**, **RM0008**, **PM0056** et **ES096** depuis le site de ST (liens dans le guide), et noter leur révision dans le journal.
- Lire le guide de lecture de documentation (10 minutes, sections 0 à 4).

## Phase 1 — Le cours ST adapté au F103

| Leçon | Volet officiel | Idée-force (analogie) | Pratique principale | 📖 Doc à consulter | Lien avec le dépôt |
|---|---|---|---|---|---|
| **1** | Écosystème STM32 + démarrage avec cartes et logiciels | Le microcontrôleur est **une petite ville** | Lire le code généré d'un projet existant, déboguer, observer un registre | DS5319 (identité, brochage, courants) ; RM0008 chapitres RCC et GPIO | `learning/01-gpio/blink-button-debounce` |
| **2** | Accès aux registres (1/2) : mémoire et démarrage | La carte mémoire est **l'annuaire** ; le démarrage est **l'ouverture du magasin** | Étudier fichier de démarrage, linker script, table des vecteurs | RM0008 chapitre 3 (carte mémoire, configuration du boot) ; PM0056 (exceptions) | tout projet |
| **3** | Accès aux registres (2/2) : assembleur puis C | Un registre est **un tableau d'interrupteurs** | PC13 en assembleur, puis en C avec registres | RM0008 chapitres RCC et GPIO (registres) ; PM0056 (jeu d'instructions) | `learning/06-registers/baremetal-gpio` |
| **4** | CubeMX, HAL et bibliothèques bas niveau | HAL = **taxi**, LL = **ta voiture**, registres = **à pied** | Même LED en HAL, LL, registres : comparer taille et vitesse | Sections « How to use this driver » dans `Drivers/STM32F1xx_HAL_Driver/Src/` ; manuel « Description of STM32F1 HAL and low-layer drivers » | `learning/01-gpio` |
| **5** | Développer avec CubeMX et LL | **Plan d'architecte** avant de construire | Mini-projet combiné | Datasheets des composants du montage ; notes d'application utiles | `projects/` |

## Phase 2 — Approfondissement : refaire plus bas ce qui existe en HAL

| Module | Ce qui est déjà fait en HAL | Objectif d'approfondissement | 📖 Doc à consulter |
|---|---|---|---|
| A. Horloges | Réglages par défaut | Arbre d'horloge, HSE, PLL 72 MHz, sortie MCO, mesure réelle | RM0008 chapitre RCC (schéma de l'arbre d'horloge) ; DS5319 (caractéristiques des oscillateurs) |
| B. Interruptions | `learning/04-interrupts/exti-basics` (inachevé), `projects/pwm-led-mode-controller` | Finir EXTI, comprendre NVIC et priorités, écrire un handler sans HAL | PM0056 (NVIC, exceptions) ; RM0008 chapitre « Interrupts and events » |
| C. Timers | `learning/02-timers/timer-interrupts-pwm` | Calculer prescaler/période à la main, PWM par registres, capture d'entrée | RM0008 chapitre « General-purpose timers (TIMx) » |
| D. ADC | `projects/adc-dma-uart-monitor`, `projects/ldr-pwm-light-controller` | Conversion par registres, temps d'échantillonnage, bruit et filtrage | RM0008 chapitre ADC ; DS5319 caractéristiques ADC (cherche `RAIN`, l'impédance de source maximale : elle concerne ton pont diviseur LDR et ton potentiomètre) |
| E. Communication | `learning/03-uart`, `learning/05-i2c` | UART par registres, analyse I²C à l'analyseur logique ou à l'oscilloscope, SPI | RM0008 chapitres USART, I2C, SPI ; datasheet du SSD1306 ; spécification I²C (NXP UM10204) |
| F. DMA | Utilisé dans les projets ADC | Configurer un canal DMA par registres | RM0008 chapitre DMA (tableau des requêtes par canal) |
| G. Boot et démarrage | — | Modes BOOT0/BOOT1, bootloader système, MOOC « boot and startup tips » (20 min) | RM0008 chapitre 3 (configuration du boot) ; note d'application AN2606 sur le boot en mémoire système |
| H. Débogage avancé | — | Points d'arrêt, SWV, provoquer et analyser un HardFault | PM0056 (registres de statut de faute) ; ES096 (errata) |
| I. Temps réel | `projects/freertos-microweather-altimeter` (fondation) | Plusieurs tâches, files, mutex, finir le projet | Documentation FreeRTOS et API CMSIS-RTOS v2 ; datasheet du capteur choisi pour l'altimètre |

> Les identifiants et numéros de chapitre de la phase 2 sont confirmés sur les PDF au début de chaque module, comme nous l'avons fait pour la phase 1.

## Évolution du niveau de lecture

| Étape | Niveau visé (voir le guide) |
|---|---|
| Leçons 1 et 2 | 1 — Trouver une valeur, un chapitre, une table |
| Leçons 3 à 5 | 2 — Comprendre un registre, un schéma |
| Modules A à F | 3 — Concevoir : calculer avec marge à partir des documents |
| Modules G à I | 4 — Critiquer : croiser les sources, errata, écarts entre clone et puce d'origine |

## MOOC ST complémentaires (durées indiquées sur la page ST)

- STM32CubeIDE basics : 2 h 30
- STM32Cube basics MOOC with hands-on exercises : 8 h
- STM32 boot and startup tips : 20 min
- FreeRTOS on STM32 : 10 h

## Rythme conseillé

Une leçon par session de 1 h 30 à 2 h : 20 min de vidéo, 60 min de pratique, 10 min de bilan dans le journal. Le réflexe documentation ajoute environ 30 à 45 minutes par leçon ; les exercices de calcul peuvent se faire à une autre session.
