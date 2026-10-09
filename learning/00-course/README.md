# Parcours STM32 — Notes de cours (en français)

Notes personnelles d'étude, basées sur le MOOC officiel ST
**« Moving from 8 to 32 bits workshop – first steps in STM32 »**, puis prolongées par d'autres modules.

> Les READMEs du reste du dépôt sont en anglais. Ce dossier est en français car c'est un carnet d'étude.

## Profil et objectif

- **Apprenant** : ingénieur en électronique et systèmes embarqués.
- **Objectif** : maîtriser l'écosystème STM32 en profondeur (registres, démarrage, HAL/LL, périphériques, débogage), pas seulement faire clignoter une LED.
- **Méthode** : analogies, théorie courte, pratique d'abord, défis, progression étape par étape.

## Matériel et outils (confirmés)

| Élément | Détail |
|---|---|
| Carte | Blue Pill, STM32F103C8T6 (Cortex-M3) |
| Programmateur | ST-Link V2 USB, en SWD |
| IDE | STM32CubeIDE |
| Autre outil | STM32 ST-LINK Utility (STM32CubeProgrammer est son successeur) |
| Composants | Résistances, condensateurs, potentiomètres, écrans |

## Écarts entre le cours ST et notre matériel

1. **Carte** : le cours utilise la NUCLEO-F072RB (Cortex-M0, STM32F0). Nous utilisons le **STM32F103** (Cortex-M3, STM32F1). Les concepts sont identiques, mais **adresses de registres, noms de bits et options CubeMX diffèrent** : chaque leçon donne l'équivalent F103.
2. **Chaîne d'outils** : le cours utilise Keil uVision. Nous utilisons **STM32CubeIDE**.
3. **Durée** : la page ST annonce environ 4 h sur la fiche du cours et 8 h sur la liste des MOOC. Compte plutôt 8 h, plus la pratique.

## Où on en est : le dépôt contient déjà de la pratique

Les dossiers `learning/` et `projects/` du dépôt montrent un niveau déjà avancé en **HAL** (timers, UART, EXTI, I²C, ADC+DMA, FreeRTOS). Ce cours sert donc surtout à **combler ce que HAL cache** : démarrage, carte mémoire, registres, assembleur, API LL. Principe directeur : **refaire à plus bas niveau ce qui a déjà été fait en HAL**, et comparer.

## Contenu du dossier

| Fichier | Rôle |
|---|---|
| `README.md` | Cette page |
| `PLAN_DE_COURS.md` | Feuille de route module par module |
| `JOURNAL.md` | Carnet de bord : progression, questions, acquis |
| `LECON_01.md` | Leçon 1 : carte mentale, chaîne d'outils, lecture du code généré |

## Règles

1. Une leçon = courte théorie imagée, pratique guidée, défis, contrôle de compréhension.
2. Les erreurs font partie du cours : on les montre et on les débogue.
3. Les acquis et les doutes se notent dans `JOURNAL.md`.
4. Aucune ligne de code sans explication de sa raison d'être.
5. Les exercices terminés vont dans `learning/`, les réalisations documentées dans `projects/`.

## Sources

- Page du cours : https://www.st.com/content/st_com/en/support/learning/stm32-moocs/Moving_from_8_to_32_bits_workshop_MOOC.html
- Playlist : https://www.youtube.com/playlist?list=PLnMKNibPkDnHXgWV0h36LQDGrEuT2r5I4
