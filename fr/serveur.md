# Serveur et datapacks

[Accueil](../README.md) · [Guide](guide.md) · [Gagner de l'XP](xp.md) · [Arbres de compétences](arbres.md) · [English](../en/server.md)

## Configuration

Chaque monde a son propre fichier : `serverconfig/legacyskills-server.toml` dans le dossier du monde (en solo : `saves/<monde>/serverconfig/`). Il est créé au premier démarrage, avec un commentaire sur chaque ligne. Valeurs par défaut :

| Section | Clé | Défaut | Ce qu'elle fait |
|---|---|---|---|
| `progression` | `max_level` | 50 | Niveau maximum d'une discipline. L'XP continue de s'accumuler au-delà |
| `progression` | `point_cap` | 24 | Plafond des points gagnés par les niveaux, avant Touche-à-tout |
| `budget` | `xp_per_hour` | 4000 | XP brute par heure de jeu, par joueur et par discipline. 0 coupe le budget |
| `budget` | `reserve` | 1000 | Réserve maximale |
| `diminishing_returns` | `full` | 20 | Charge sous laquelle un gain reste à plein tarif |
| `diminishing_returns` | `recovery_per_minute` | 2 | Charge perdue par minute |
| `diminishing_returns` | `step_per_unit` | 0.05 | Baisse par unité de charge au-delà du plein tarif |
| `diminishing_returns` | `floor` | 0.1 | Coefficient minimal |
| `kills` | `max_base` | 200 | XP la plus élevée d'une mise à mort, partagée entre les joueurs |
| `kills` | `credit_expiry_seconds` | 120 | Le crédit d'un joueur sur une créature se perd ce temps après son dernier coup |
| `multipliers` | `global`, `combat`, `crafting`, `adventure` | 1 | Multiplicateurs appliqués après le budget |
| `multipliers` | `others` | vide | Autres disciplines venues de datapacks : `"espace:nom=multiplicateur"` |
| `activity` | `enabled` | true | Filtre d'inactivité |
| `activity` | `half_after_seconds` | 120 | Silence avant ×0,5 |
| `activity` | `zero_after_seconds` | 300 | Silence avant ×0 |
| `activity` | `look_degrees` | 2 | Plus petite rotation qui compte en une seconde |
| `activity` | `move_blocks` | 1 | Plus petit déplacement qui compte en une seconde |
| `sources.kill` | `enabled`, `spawner_multiplier` | true, 0.1 | Mises à mort, et part des créatures de générateur de monstres |
| `sources.shield_block` | `enabled`, `xp_per_damage`, `damage_cap` | true, 0.5, 10 | Vrais blocages au bouclier |
| `sources.anvil` | `enabled`, `xp_per_full_repair` | true, 30 | Réparations à l'enclume |
| `sources.crafting` | `enabled`, `xp_per_item` | true, 6 | Fabrication d'objets durables |
| `sources.smithing` | `enabled`, `xp_per_item` | true, 25 | Table de forge |
| `sources.smelting` | `enabled`, `xp_per_item`, `returns_weight_per_item` | true, 0.5, 0.25 | Sortie de fourneau prise à la main |
| `sources.movement` | `enabled`, `xp_per_100m`, `cell_size` | true, 8, 64 | Course, nage, escalade |
| `sources.movement` | `returns_full`, `returns_recovery_per_minute` | 3, 0.3 | Rendement décroissant par zone de terrain |
| `sources.advancement` | `enabled` | true | Progrès déclarés en datapacks |
| `nodes` | `salvage_chance` | 0.15 | Chance de Chute récupérée |
| `respec` | `base_cost`, `cost_per_node`, `max_cost` | 5, 1, 30 | Coût de la réorientation en niveaux vanilla (la première est gratuite) |
| `respec` | `delay_hours` | 2 | Temps de jeu entre deux réorientations |
| `actives` | `rally_cry_cooldown`, `rally_cry_rampart_cooldown` | 120, 60 | Recharge du Cri de ralliement, en secondes |
| `actives` | `rally_cry_duration`, `rally_cry_radius` | 6, 8 | Durée et rayon de Résistance I |
| `actives` | `field_repair_cooldown`, `field_repair_share` | 90, 0.15 | Réparation de fortune |
| `actives` | `dash_cooldown`, `dash_scout_cooldown`, `dash_strength` | 12, 6, 1.1 | Élan |
| `journal` | `gains_csv` | false | Écrit chaque gain d'XP dans `legacyskills_gains.csv`, dans le dossier du monde |

### Aidez à équilibrer le mod

Les valeurs d'XP sont encore des estimations de conception. Si vous avez un serveur, mettez `gains_csv = true` : chaque ligne de `legacyskills_gains.csv` est une tentative de gain, avec la source, l'XP brute et ce qui reste après chaque étape. Partager ce fichier dans les commentaires CurseForge aide beaucoup.

## Commandes

Toutes les commandes demandent le niveau d'opérateur 2.

| Commande | Ce qu'elle fait |
|---|---|
| `/legacyskills info <joueur>` | XP, niveau, rang et réserve par discipline, points libres, nœuds, spécialisation, activité en cours |
| `/legacyskills xp <joueur> <discipline> get` | XP d'une discipline |
| `/legacyskills xp <joueur> <discipline> add <n>` ou `set <n>` | Ajoute ou règle l'XP, sans filtre. Marche aussi pour un joueur déconnecté |
| `/legacyskills xp <joueur> <discipline> gain <n> <clé>` | Un gain qui passe par le rendement décroissant et le budget. Joueur connecté seulement |
| `/legacyskills node <joueur> add <nœud>` ou `remove <nœud>` | Ajoute un nœud selon les règles d'un achat, ou le retire |
| `/legacyskills spec <joueur> set <id>` ou `clear` | Règle ou retire la spécialisation |
| `/legacyskills reset <joueur>` | Efface le profil |
| `/legacyskills definitions` | Disciplines, nœuds, spécialisations et sources d'XP acceptés, refus avec leur raison, et une empreinte |

## Sauvegardes

Les profils sont sauvegardés dans `data/legacyskills_players.dat`, dans le dossier du monde, par UUID de joueur. Un fichier illisible est renommé au lieu d'être écrasé, et un fichier écrit par une version plus récente du mod n'est jamais réécrit.

## Datapacks

Tout le contenu des arbres est en JSON, rechargé par `/reload`. Les conditions NeoForge (`neoforge:conditions`) fonctionnent. Une clé inconnue fait refuser le fichier. Un fichier refusé n'empêche jamais les autres de se charger, et chaque refus est écrit dans le journal et affiché par `/legacyskills definitions`. Si un nœud disparaît, les joueurs qui l'avaient récupèrent leurs points.

| Dossier | Contenu |
|---|---|
| `data/<espace>/legacyskills/discipline/` | `icon`, `order` |
| `data/<espace>/legacyskills/node/` | `discipline`, `icon`, `cost` (1 à 10), `required_level`, `requires` (8 nœuds au plus), `branch`, `x`, `y`, `exclusive_group`, `effects` |
| `data/<espace>/legacyskills/specialization/` | `icon`, `bonus_points` (0 à 10), `requires` (`levels`, `nodes`), `effects` |
| `data/<espace>/legacyskills/xp_source/` | Sources d'XP, voir plus bas |

Effets des nœuds et des spécialisations :

- `legacyskills:flag` : un drapeau lu par Legacy Skills ou par un autre mod par `legacyskills:unlocks`.
- `legacyskills:active` : une compétence active (`legacyskills:rally_cry`, `legacyskills:field_repair`, `legacyskills:dash`).
- `legacyskills:attribute` : un attribut, un montant et une opération. Dégâts, vitesse d'attaque, armure, vie et chance sont refusés.

Un nœud, par exemple :

```json
{
  "discipline": "legacyskills:adventure",
  "icon": "minecraft:feather",
  "cost": 1,
  "required_level": 3,
  "branch": "body",
  "x": 0,
  "y": 0,
  "effects": [
    {"type": "legacyskills:attribute", "attribute": "minecraft:generic.safe_fall_distance", "amount": 2.0, "operation": "add_value"}
  ]
}
```

Sources d'XP :

```json
{"type": "legacyskills:advancement", "discipline": "legacyskills:combat", "advancement": "minecraft:adventure/kill_a_mob", "xp": 50}
```

```json
{"type": "legacyskills:external", "discipline": "legacyskills:crafting", "max_per_call": 100}
```

`xp` va de 1 à 10000, `max_per_call` de 1 à 1000.

Tags : `legacyskills:durable_goods` (objets qui donnent de l'XP d'Artisanat), `legacyskills:recoverable` (ingrédients que Chute récupérée peut rendre), `legacyskills:no_kill_xp` (créatures sans XP de mise à mort, vide par défaut).

## Pour les moddeurs

Legacy Skills expose trois capacités d'entité, faites seulement de types du JDK : votre mod n'a pas besoin d'en dépendre. Si Legacy Skills n'est pas installé, vous recevez `null`.

```java
EntityCapability<Predicate, Void> UNLOCKS = EntityCapability.createVoid(ResourceLocation.parse("legacyskills:unlocks"), Predicate.class);
Predicate<ResourceLocation> unlocks = player.getCapability(UNLOCKS);
```

| Capacité | Type | Usage |
|---|---|---|
| `legacyskills:unlocks` | `Predicate<ResourceLocation>` | Ce drapeau, cette compétence active, ce nœud ou cette spécialisation est-il en vigueur pour le joueur ? |
| `legacyskills:levels` | `ToIntFunction<ResourceLocation>` | Niveau d'une discipline |
| `legacyskills:xp` | `ObjIntConsumer<ResourceLocation>` | Serveur seulement : donne de l'XP au nom d'une source `legacyskills:external`, plafonnée par appel, puis les étapes habituelles |

Côté client, seul le joueur local répond, d'après ce que le serveur lui a envoyé.
