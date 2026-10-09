# Plan de cours STM32

Le plan suit le **plan officiel du MOOC ST** (5 volets). Les titres exacts des vidéos ne figurent pas sur la page du cours : tu les notes dans `JOURNAL.md` au fil du visionnage et nous ajustons le découpage.

**Principe** : le dépôt contient déjà des projets en HAL. Chaque leçon vise donc à comprendre ce qu'il y a **sous** HAL, puis à le refaire plus bas.

---

## Phase 0 — Préparation (fait / en cours)

- Carte : Blue Pill STM32F103C8T6. Programmateur : ST-Link V2. ✅
- Vérifier la connexion SWD (voir `docs/setup.md`).
- Noter dans le journal ce que renvoie l'outil (Device ID, taille Flash, éventuel clone).

## Phase 1 — Le cours ST adapté au F103

| Leçon | Volet officiel | Idée-force (analogie) | Pratique principale | Lien avec le dépôt |
|---|---|---|---|---|
| **1** | Écosystème STM32 + démarrage avec cartes et logiciels | Le microcontrôleur est **une petite ville** | Lire le code généré d'un projet existant, déboguer, observer un registre | `learning/01-gpio/blink-button-debounce` |
| **2** | Accès aux registres (1/2) : mémoire et démarrage | La carte mémoire est **l'annuaire** ; le démarrage est **l'ouverture du magasin** | Étudier fichier de démarrage, linker script, table des vecteurs | tout projet |
| **3** | Accès aux registres (2/2) : assembleur puis C | Un registre est **un tableau d'interrupteurs** | PC13 en assembleur, puis en C avec registres | `learning/06-registers/baremetal-gpio` |
| **4** | CubeMX, HAL et bibliothèques bas niveau | HAL = **taxi**, LL = **ta voiture**, registres = **à pied** | Même LED en HAL, LL, registres : comparer taille et vitesse | `learning/01-gpio` |
| **5** | Développer avec CubeMX et LL | **Plan d'architecte** avant de construire | Mini-projet combiné | `projects/` |

## Phase 2 — Approfondissement : refaire plus bas ce qui existe en HAL

| Module | Ce qui est déjà fait en HAL | Objectif d'approfondissement |
|---|---|---|
| A. Horloges | Réglages par défaut | Arbre d'horloge, HSE, PLL 72 MHz, sortie MCO, mesure réelle |
| B. Interruptions | `learning/04-interrupts/exti-basics` (inachevé), `projects/pwm-led-mode-controller` | Finir EXTI, comprendre NVIC et priorités, écrire un handler sans HAL |
| C. Timers | `learning/02-timers/timer-interrupts-pwm` | Calculer prescaler/période à la main, PWM par registres, capture d'entrée |
| D. ADC | `projects/adc-dma-uart-monitor`, `projects/ldr-pwm-light-controller` | Conversion par registres, temps d'échantillonnage, bruit et filtrage |
| E. Communication | `learning/03-uart`, `learning/05-i2c` | UART par registres, analyse I²C à l'analyseur logique ou à l'oscilloscope, SPI |
| F. DMA | Utilisé dans les projets ADC | Configurer un canal DMA par registres |
| G. Boot et démarrage | — | Modes BOOT0/BOOT1, bootloader système, MOOC « boot and startup tips » (20 min) |
| H. Débogage avancé | — | Points d'arrêt, SWV, provoquer et analyser un HardFault |
| I. Temps réel | `projects/freertos-microweather-altimeter` (fondation) | Plusieurs tâches, files, mutex, finir le projet |

## MOOC ST complémentaires (durées indiquées sur la page ST)

- STM32CubeIDE basics : 2 h 30
- STM32Cube basics MOOC with hands-on exercises : 8 h
- STM32 boot and startup tips : 20 min
- FreeRTOS on STM32 : 10 h

## Rythme conseillé

Une leçon par session de 1 h 30 à 2 h : 20 min de vidéo, 60 min de pratique, 10 min de bilan dans le journal.
