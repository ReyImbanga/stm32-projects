# Guide — Lire une documentation technique

> Un bon ingénieur ne retient pas tout. Il sait **où chercher** et **quoi croire**.
> Ce guide s'applique au STM32 comme à n'importe quel composant ou outil : résistance, LED, transistor, capteur, afficheur, régulateur, oscilloscope.

## 0. L'idée en une image

La documentation est **la bibliothèque d'une ville**. Personne ne lit toute la bibliothèque : on connaît les rayons, on ouvre l'index, on va droit à la bonne page. Ton objectif est de passer de « je cherche au hasard » à « je sais dans quel rayon aller ».

### Règle du cours : je te donne l'adresse, pas la réponse

Pour chaque exercice, je te dis **où** chercher. Tu trouves, tu me montres, et on vérifie ensemble. Si tu bloques, je te donne les indices dans cet ordre :

| Indice | Ce que je te donne |
|---|---|
| 1 | Le bon document |
| 2 | Le chapitre ou un mot-clé à chercher |
| 3 | La table, la figure ou la page |
| 4 | La réponse, avec la façon de la retrouver seul la prochaine fois |

Cherche **10 minutes** seul avant de demander l'indice suivant. C'est inconfortable, et c'est exactement là que la compétence se construit.

---

## 1. Les types de documents

Imagine que la puce est une voiture.

| Document | Analogie | Il répond à… | Pour le STM32F103C8 |
|---|---|---|---|
| **Datasheet** | La fiche technique du constructeur | Qu'est-ce que c'est, et jusqu'où ça va : brochage, tensions, courants, timings | **DS5319** |
| **Reference manual** | Le manuel d'atelier | Comment chaque organe fonctionne et se commande : registres, bits, schémas internes | **RM0008** |
| **Programming manual** | Le manuel du moteur | Le cœur lui-même : instructions, exceptions, NVIC, SysTick | **PM0056** |
| **Errata** | L'avis de rappel | Les défauts connus de la puce et leurs contournements | **ES096** |
| **Application note (AN)** | La recette du chef | Une tâche précise, pas à pas | Selon le besoin |
| **Manuel de carte et schéma** | Le plan de la maison | Quel fil va où sur la carte | Voir la note Blue Pill ci-dessous |
| **Documentation logicielle** | Le mode d'emploi du logiciel | Comment utiliser HAL, LL, CubeIDE | Commentaires « How to use this driver » dans `Drivers/` |

**Note Blue Pill.** La Blue Pill n'est pas une carte ST. Aucun manuel ST ne décrit ce que la carte ajoute autour de la puce (régulateur, quartz, LED, résistances de BOOT). Pour cela, il faut le schéma de la carte (communautaire) et un multimètre. La datasheet décrit la **puce**, jamais la **carte**.

### Les documents de ton microcontrôleur (vérifiés sur les PDF officiels)

| ID | Titre | Révision lue | Lien officiel |
|---|---|---|---|
| DS5319 | Medium-density performance line Arm-based 32-bit MCU with 64 or 128 KB Flash, USB, CAN, 7 timers, 2 ADCs, 9 com. interfaces (STM32F103x8, STM32F103xB) | Rev 20, juillet 2025 | [stm32f103c8.pdf](https://www.st.com/resource/en/datasheet/stm32f103c8.pdf) |
| RM0008 | STM32F101xx, STM32F102xx, STM32F103xx, STM32F105xx and STM32F107xx advanced Arm-based 32-bit MCUs | Rev 21, février 2021 | [RM0008](https://www.st.com/resource/en/reference_manual/rm0008-stm32f101xx-stm32f102xx-stm32f103xx-stm32f105xx-and-stm32f107xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf) |
| PM0056 | Cortex-M3 programming manual (STM32F10xxx/20xxx/21xxx/L1xxxx) | Rev 7, décembre 2024 | [PM0056](https://www.st.com/resource/en/programming_manual/pm0056-stm32f10xxx20xxx21xxl1xxxx-cortexm3-programming-manual-stmicroelectronics.pdf) |
| ES096 | STM32F101x8/B, STM32F102x8/B and STM32F103x8/B medium-density device errata | Rev 15, mars 2022 | [ES096](https://www.st.com/resource/en/errata_sheet/es096-stm32f103x8b-device-errata-stmicroelectronics.pdf) |

ST peut publier de nouvelles révisions : note toujours la révision que **tu** lis dans ta fiche de lecture (section 6).

**Télécharge depuis le site du constructeur.** Les sites miroirs peuvent héberger des versions anciennes ou modifiées.

---

## 2. Quelle question, quel document ?

C'est la table à connaître par cœur. Les numéros de chapitre sont ceux des révisions ci-dessus.

| Ta question | Document | Où chercher |
|---|---|---|
| Quelle taille de Flash, de RAM, quelle fréquence max ? | DS5319 | Première page, rubrique Features |
| Quelle broche porte `USART1_TX`, et quelle est sa tolérance en tension ? | DS5319 | Section 3 « Pinouts and pin description », Table 5 « Medium-density STM32F103xx pin definitions » |
| Quel courant maximal sur une broche ? | DS5319 | Section 5.2 « Absolute maximum ratings », Table 7 « Current characteristics » |
| Quelle tension et quel courant garantis en sortie, en fonctionnement normal ? | DS5319 | Chapitre « Electrical characteristics », tableaux de niveaux d'E/S |
| Comment configurer une broche GPIO ? | RM0008 | Chapitre 9 « General-purpose and alternate-function I/Os (GPIOs and AFIOs) » |
| Comment activer l'horloge d'un périphérique ? | RM0008 | Chapitre 7 « Reset and clock control (RCC) », registres d'activation d'horloge |
| À quelle adresse se trouve un périphérique ? | RM0008 | Chapitre 3 « Memory and bus architecture », carte mémoire |
| Comment fonctionne un timer, un ADC, une UART ? | RM0008 | Le chapitre du périphérique (table des matières) |
| Comment fonctionnent les interruptions, SysTick, les exceptions ? | PM0056 | Sections exceptions, NVIC, SysTick |
| Quelle instruction assembleur utiliser ? | PM0056 | Chapitre du jeu d'instructions |
| Mon périphérique se comporte étrangement, est-ce moi ou la puce ? | ES096 | Tableau récapitulatif des limitations |

---

## 3. La méthode en 6 étapes

1. **Formule ta question avec précision.** « Combien de courant peut sortir d'une broche ? » est vague. « Quel courant maximal, dans quelles conditions, pour une LED sur PB9 ? » est exploitable.
2. **Choisis le bon document** avec la table de la section 2.
3. **Navigue, ne lis pas.** Ouvre le panneau des **signets** du PDF (la table des matières cliquable), puis `Ctrl+F` avec les mots du constructeur, en anglais : `Absolute maximum ratings`, `pin definitions`, `register map`, le nom du registre.
4. **Lis le tableau, pas le paragraphe autour.** Puis lis les **notes de bas de tableau** et la colonne **Conditions**. L'information décisive s'y cache souvent.
5. **Vérifie le contexte** : bonne famille (densité), bon boîtier, bonne révision, bonnes conditions (température, tension d'alimentation).
6. **Recoupe** avec une seconde source : les errata, le code généré par CubeMX, un multimètre ou un oscilloscope. Puis écris la réponse dans ta fiche de lecture avec sa page.

---

## 4. Les pièges classiques

| Piège | Analogie | Ce qu'il faut faire |
|---|---|---|
| Confondre **Absolute maximum ratings** et conditions de fonctionnement | La limite de rupture du pont n'est pas le panneau de limitation de vitesse | Ne dimensionne jamais sur les valeurs absolues maximales. Elles servent à ne rien détruire, pas à faire fonctionner le circuit |
| Se fier à la valeur **typ** | « En général il fait beau » n'est pas une prévision | Seules les valeurs **min** et **max** sont des engagements, dans les conditions indiquées. Dimensionne sur elles |
| Ignorer la colonne **Conditions** | Une vitesse de pointe mesurée en descente | Vérifie que tes conditions (alimentation, température, charge) sont celles du tableau |
| Mélanger les variantes d'une famille | Même modèle de voiture, moteurs différents | Un document couvre souvent plusieurs puces (ici 64 ou 128 KB de Flash). Retrouve **ta** variante |
| Écrire dans les bits **Reserved** d'un registre | Toucher un interrupteur marqué « ne pas toucher » | Laisse-les à leur valeur de reset. Modifie un registre avec lecture, masque, écriture |
| Ignorer la légende des registres (`rw`, `r`, `w`…) | Ne pas lire la légende d'un plan | Lis la légende au début du chapitre des registres : elle dit si un bit se lit, s'écrit, ou s'efface en écrivant |
| Oublier les **errata** | Rouler avec un rappel constructeur non traité | Avant de déboguer pendant des heures un périphérique, lis ES096 |
| Croire que la datasheet décrit la **carte** | Lire la fiche du moteur pour connaître la couleur de la carrosserie | La puce et la carte sont deux documents différents |
| Utiliser un clone sans le savoir | Une pièce « compatible » n'a pas forcément les mêmes tolérances | Si ta puce est un clone (CKS32, GD32…), sa propre documentation prime. Note le marquage dans ton journal |

---

## 5. Tout composant : les 7 questions

Cette liste marche pour une LED comme pour un capteur I²C.

| N° | Question | Où la trouver en général |
|---|---|---|
| 1 | **Que fait-il ?** | Description, Features, schéma fonctionnel |
| 2 | **Comment l'alimenter ?** Tension min/typ/max, courant consommé | Recommended operating conditions |
| 3 | **Quel est son brochage ?** Boîtier, broches, repère de la broche 1 | Pinout, package |
| 4 | **Quelles sont ses limites absolues ?** | Absolute maximum ratings |
| 5 | **Que garantit-il dans mon cas ?** Niveaux logiques, timings, précision | Electrical characteristics, AC/DC characteristics |
| 6 | **Comment le piloter ?** Interface, protocole, adresse, séquence d'initialisation | Chapitre fonctionnel, registres, diagrammes de timing |
| 7 | **Comment le câbler ?** Composants externes recommandés, découplage | Typical application circuit |

### Appliqué à ton matériel

| Composant | Questions-clés | Ce que tu y trouveras |
|---|---|---|
| **LED** | 2, 4, 5 | Tension directe Vf à un courant donné, courant maximal : de quoi calculer la résistance série |
| **Transistor NPN** (étage de commande de LED) | 4, 5, 7 | Gain hFE, courant collecteur maximal, saturation VCE(sat) : de quoi calculer la résistance de base |
| **Photorésistance (LDR)** | 5, 6 | Résistance sous éclairement et dans le noir, temps de réponse : de quoi choisir la résistance du pont diviseur |
| **Potentiomètre** | 1, 5 | Valeur, tolérance, loi (linéaire ou logarithmique), puissance |
| **Afficheur OLED SSD1306** | 2, 5, 6, 7 | Adresse I²C, séquence d'initialisation, commandes |
| **Capteur de pression** (futur altimètre) | 5, 6, 7 | Registres, formules de compensation, circuit type |
| **Instrument** (oscilloscope, analyseur logique, ST-Link) | 5 | Bande passante, fréquence d'échantillonnage, niveaux de tension acceptés |

---

## 6. La fiche de lecture

Chaque information trouvée s'écrit dans le tableau du `JOURNAL.md`. C'est ce qui te rend rapide : la prochaine fois, tu ouvres ta fiche au lieu du PDF.

| Colonne | Contenu |
|---|---|
| Date | |
| Composant | Exemple : STM32F103C8 |
| Document et révision | Exemple : DS5319 Rev 20 |
| Question | La question précise posée à l'étape 1 |
| Réponse | La valeur, avec son **unité** |
| Où | Section, table ou figure, et page |
| Conditions | Température, tension, charge |
| Vérifié comment | Seconde source (errata, test, calcul) |

---

## 7. Ta progression de lecteur

| Niveau | Tu sais… | Exemple |
|---|---|---|
| 1 — Trouver | Localiser une valeur | Le brochage d'une fonction, la tension d'alimentation |
| 2 — Comprendre | Expliquer un schéma ou un registre | Le rôle de chaque bit d'un registre de configuration |
| 3 — Concevoir | Calculer avec marge à partir de la documentation | Une résistance série, un temps d'échantillonnage |
| 4 — Critiquer | Croiser les sources et repérer les écarts | Comparer deux composants, repérer une errata, détecter une contradiction |

Les exercices démarrent au niveau 1 et montent progressivement. Chaque leçon contient une rubrique **📖 Réflexe documentation**.
