# 02 – Inversion du sens de rotation d'un moteur asynchrone triphasé

Schéma de puissance et de commande permettant de faire tourner un moteur dans les deux sens, avec verrouillage électrique, conçu et simulé dans **CADe_SIMU**.

*Réalisé en novembre 2026 – Roméo Sylvain Ouédraogo*

## Objectif

Commander un moteur triphasé en marche avant et en marche arrière à l'aide de trois boutons-poussoirs, avec :
- un **auto-maintien** dans chaque sens ;
- un **verrouillage électrique** qui empêche les deux contacteurs de se fermer en même temps ;
- une **protection** contre les courts-circuits (fusibles) et contre les surcharges (relais thermique).

## Principe

Pour inverser le sens de rotation d'un moteur asynchrone triphasé, on **permute deux de ses trois phases**.

- **KM1 (avant)** transmet les phases dans l'ordre normal : L1, L2, L3.
- **KM2 (arrière)** croise L1 et L3 : L3, L2, L1. La phase L2 ne change pas.

## Outils

- CADe_SIMU (conception et simulation de schémas électriques)
- Windows 11 en machine virtuelle sous Ubuntu

## Composants

| Repère | Composant | Rôle |
| --- | --- | --- |
| F1 | Fusibles tripolaires | Protection du circuit de puissance contre les courts-circuits |
| KM1 | Contacteur | Marche avant |
| KM2 | Contacteur | Marche arrière (permutation de L1 et L3) |
| F2 | Relais thermique | Protection du moteur contre les surcharges |
| M | Moteur asynchrone triphasé | Récepteur |
| F3 | Fusible | Protection du circuit de commande |
| S1 | Bouton-poussoir NF | Arrêt |
| S2 | Bouton-poussoir NO | Marche avant |
| S3 | Bouton-poussoir NO | Marche arrière |

## Fonctionnement

**Circuit de puissance** : après F1, les contacteurs KM1 et KM2 sont branchés en parallèle, puis rejoignent F2 et le moteur. KM2 croise les phases L1 et L3.

**Circuit de commande** (L → N) : F3 → contact F2 (95-96, NF) → S1 (NF), puis deux branches en parallèle :
- **Branche avant** : S2 en parallèle avec KM1 (13-14) → contact NF de **KM2** → bobine KM1
- **Branche arrière** : S3 en parallèle avec KM2 (13-14) → contact NF de **KM1** → bobine KM2

**Le verrouillage électrique** : chaque branche est coupée par un contact NF de **l'autre** contacteur. Quand KM1 est fermé, son contact NF s'ouvre dans la branche arrière, et la bobine KM2 ne peut plus être alimentée. Sans ce verrouillage, la fermeture simultanée de KM1 et KM2 provoquerait un **court-circuit entre les phases L1 et L3**.

Pour changer de sens, il faut d'abord arrêter le moteur avec S1.

## Tests réalisés en simulation

| Test | Résultat attendu | Résultat |
| --- | --- | --- |
| Appui sur S2 | Le moteur tourne en marche avant | ✅ |
| Appui sur S1, puis sur S3 | Le moteur s'arrête, puis tourne en marche arrière | ✅ |
| Marche avant + appui sur S3 | Rien ne se passe (verrouillage) | ✅ |
| Marche arrière + appui sur S2 | Rien ne se passe (verrouillage) | ✅ |
| Déclenchement de F2 | Le moteur s'arrête, quel que soit le sens | ✅ |

## Captures

| Marche avant | Marche arrière | Verrouillage |
| --- | --- | --- |
| ![Marche avant](captures/avant.png) | ![Marche arrière](captures/arriere.png) | ![Verrouillage](captures/verrouillage.png) |

## Ce que j'ai appris

- **Le sens de rotation dépend de l'ordre des phases** : permuter deux phases suffit à l'inverser.
- **Le verrouillage se fait avec le contact de l'autre contacteur** : un contacteur verrouillé par son propre contact NF se met à « battre » (il s'ouvre et se ferme en boucle), un défaut qui l'use très vite.
- **Dans un schéma, deux fils qui se croisent ne sont pas reliés** : seuls les points de connexion créent une liaison, ce qui permet de réaliser le croisement des phases.
- **Dépanner, c'est suivre la tension** : en simulation comme en armoire, on cherche la coupure en suivant la tension point par point.

## Fichiers

- `schema.cad` : schéma CADe_SIMU
- `captures/` : captures de la simulation