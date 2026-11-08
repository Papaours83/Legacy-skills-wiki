# Gagner de l'XP

[Accueil](../README.md) · [Guide](guide.md) · [Arbres de compétences](arbres.md) · [Serveur et datapacks](serveur.md) · [English](../en/xp.md)

Tous les chiffres de cette page sont des valeurs par défaut. Un serveur peut tous les changer, voir [Serveur et datapacks](serveur.md).

## Sources

### Combat

| Source | XP brute |
|---|---|
| Tuer une créature hostile | `min(PV max, 200) × votre part`. Votre part, ce sont vos dégâts divisés par le plus grand entre le total des dégâts des joueurs et les PV max de la créature. Un zombie (20 PV) tué seul donne 20 XP |
| Une créature sortie d'un générateur de monstres | Un dixième de la valeur ci-dessus. Les générateurs d'épreuve ne sont pas concernés |
| Un coup d'une créature hostile réellement bloqué au bouclier | `min(dégât bloqué, 10) × 0,5`, donc 5 XP au plus |

Un dégât ne compte que jusqu'aux PV qui restent à la créature. Flèches, tridents, TNT que vous avez allumée et potions jetables comptent comme vos dégâts. Le crédit d'un joueur sur une créature se perd 120 secondes après son dernier coup. Les créatures du tag `legacyskills:no_kill_xp` ne donnent rien (vide par défaut).

### Artisanat

| Source | XP brute |
|---|---|
| Réparer à l'enclume | `30 × part de durabilité rendue` : une réparation complète donne 30 |
| Fabriquer un objet du tag `legacyskills:durable_goods` | 6 par objet. Le tag contient les outils et armures en fer, en or et en diamant, les boucliers, arcs, arbalètes, cannes à pêche, etc. Les outils en bois et en pierre n'en font pas partie |
| Prendre un objet à la table de forge | 25 par objet |
| Retirer à la main des objets d'un fourneau | 0,5 par objet |

Le clic Maj donne exactement la même XP que les clics normaux.

### Aventure

| Source | XP brute |
|---|---|
| Courir, nager et grimper | 8 par 100 m |

Le rendement décroissant du déplacement se compte par zone de 64 sur 64 blocs : tourner en rond au même endroit cesse vite de rapporter. Chaque zone paie plein tarif 3 fois, puis se recharge lentement.

### Progrès

Quinze progrès vanilla donnent de l'XP, une fois par joueur, même si un opérateur retire le progrès puis le rend. Ils échappent au filtre d'inactivité, au rendement décroissant et au budget.

| Progrès | Discipline | XP |
|---|---|---|
| Chasseur de monstres (tuer un monstre) | Combat | 50 |
| Pas aujourd'hui, merci (dévier un projectile avec un bouclier) | Combat | 50 |
| Aux frontières de la mort (utiliser un totem d'immortalité) | Combat | 150 |
| Monstres chassés (tuer tous les monstres) | Combat | 500 |
| Libérez l'End ! (tuer l'Ender Dragon) | Combat | 500 |
| L'âge du fer (faire fondre un lingot de fer) | Artisanat | 50 |
| Bonne pioche ! (fabriquer une pioche en fer) | Artisanat | 50 |
| Enchanté ! (enchanter un objet) | Artisanat | 100 |
| Motif de jalousie (orner une armure) | Artisanat | 100 |
| Couvrez-moi de débris (armure complète en netherite) | Artisanat | 300 |
| Aller au fond des choses (entrer dans le Nether) | Aventure | 100 |
| À suivre... (suivre un œil de l'Ender) | Aventure | 150 |
| Fin ? (entrer dans l'End) | Aventure | 200 |
| Voyage au bout de l'enfer (explorer tous les biomes du Nether) | Aventure | 300 |
| Partons à l'aventure ! (découvrir tous les biomes) | Aventure | 500 |

Aucune XP en créatif ni en spectateur, ni pour un joueur factice (les machines d'autres mods).

## Ce qui arrive à un gain

Chaque gain passe par ces étapes, dans cet ordre :

1. **Filtre d'inactivité** (mises à mort, blocages au bouclier et déplacement seulement).
2. **Rendement décroissant** pour ce genre de gain précis.
3. **Budget horaire** de la discipline.
4. **Multiplicateurs** réglés par le serveur (tous à 1 par défaut).

### Inactivité

Le mod surveille quatre familles de signaux : le regard (au moins 2° en une seconde), le déplacement (au moins 1 bloc hors véhicule), l'inventaire (ouvrir ou fermer un menu, changer de case, jeter un objet) et le monde (casser ou poser un bloc, clic droit, saut).

| Silence | XP |
|---|---|
| Jusqu'à 120 s | ×1 |
| Plus de 120 s | ×0,5 |
| Plus de 300 s | ×0 |

Attaquer, écrire dans le chat, prendre des dégâts et ramasser des objets ne comptent pas comme de l'activité. Le déplacement ne peut pas remettre le silence à zéro pour l'XP de déplacement elle-même : un courant d'eau seul ne vous garde pas actif.

### Rendement décroissant

Chaque genre de gain a sa propre charge : les mises à mort d'un type de créature, les blocages contre un type de créature, les fabrications ou réparations d'un objet, le déplacement dans une zone de terrain. La charge monte de 1 par gain ou par objet (de votre part pour une mise à mort, de 0,25 par objet au fourneau) et baisse de 2 par minute.

| Charge | XP |
|---|---|
| Sous 20 | ×1 |
| Au-dessus de 20 | -5 % par unité de charge |
| Plancher | ×0,1 |

Changer d'activité vaut toujours mieux que répéter la même. La charge est sauvegardée avec votre profil : redémarrer le serveur ne la remet pas à zéro.

### Le budget horaire

Chaque discipline a son propre budget : 4 000 XP brute par heure de jeu, avec une réserve de 1 000 XP. La réserve se remplit pendant que vous jouez, et un gain ne peut pas prendre plus que ce qu'elle contient. Ce qui dépasse le budget est perdu. Seul le temps de jeu compte : le temps passé déconnecté ne remplit pas la réserve.

## Niveaux

Le coût du niveau L au niveau L+1 est de `50 + 15 L + 1,2 L²` XP.

| Niveau | XP totale | Coût du niveau suivant |
|---|---|---|
| 1 | 50 | 67 |
| 5 | 436 | 155 |
| 10 | 1 517 | 320 |
| 15 | 3 543 | 545 |
| 20 | 6 814 | 830 |
| 30 | 18 291 | 1 580 |
| 40 | 38 348 | 2 570 |
| 50 | 69 385 | maximum |

L'XP continue de s'accumuler après le niveau 50 : si le serveur relève le niveau maximum, elle compte tout de suite.

Les rangs sont seulement affichés et ne donnent rien : rang I dès le niveau 0, II dès 5, III dès 15, IV dès 30, V au 50.

## Points

Les points viennent de la somme de vos niveaux dans toutes les disciplines (niveaux cumulés). L'écart entre deux points augmente de 1 tous les 5 points.

| Points | Niveaux cumulés demandés |
|---|---|
| 1 à 5 | 2, 4, 6, 8, 10 |
| 6 à 10 | 13, 16, 19, 22, 25 |
| 11 à 15 | 29, 33, 37, 41, 45 |
| 16 à 20 | 50, 55, 60, 65, 70 |
| 21 à 24 | 76, 82, 88, 94 |

Les niveaux donnent 24 points au plus. Touche-à-tout en ajoute 3 au-dessus de ce plafond, donc 27 au plus, pour 35 points de nœuds dans les arbres.

Si vous perdez des points (le plafond baisse, les données changent), les derniers nœuds achetés sont rendus et mis en attente. Ils reviennent tout seuls, sans nouveau coût, dès qu'ils tiennent de nouveau.
