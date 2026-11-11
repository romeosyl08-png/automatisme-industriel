# 01 – Démarrage direct d'un moteur asynchrone triphasé

Schéma de puissance et de commande d'un démarrage direct avec auto-maintien, conçu et simulé dans **CADe_SIMU**.

*10 novembre 2026 – Roméo Sylvain Ouédraogo*

## Objectif

Démarrer et arrêter un moteur triphasé à l'aide de deux boutons-poussoirs, avec :
- un **auto-maintien** (le moteur continue de tourner après le relâchement du bouton Marche) ;
- une **protection** contre les courts-circuits (fusibles) et contre les surcharges (relais thermique).

## Outils

- CADe_SIMU (conception et simulation de schémas électriques)
- Windows 11 en machine virtuelle sous Ubuntu

## Composants

| Repère | Composant | Rôle |
| --- | --- | --- |
| F1 | Fusibles tripolaires | Protection du circuit de puissance contre les courts-circuits |
| KM | Contacteur | Établit et coupe l'alimentation du moteur |
| F2 | Relais thermique | Protection du moteur contre les surcharges |
| M | Moteur asynchrone triphasé | Récepteur |
| F3 | Fusible | Protection du circuit de commande |
| S1 | Bouton-poussoir NF | Arrêt |
| S2 | Bouton-poussoir NO | Marche |

## Fonctionnement

**Circuit de puissance** : L1, L2, L3 → F1 → contacts de puissance KM → F2 → moteur (U1, V1, W1), avec mise à la terre (PE).

**Circuit de commande** (L → N) : F3 → contact F2 (95-96, NF) → S1 (NF) → S2 (NO) en parallèle avec le contact auxiliaire KM (13-14) → bobine KM (A1-A2).

- Un appui sur **S2** alimente la bobine KM : le contacteur se ferme et le moteur démarre.
- Le contact **KM 13-14**, placé en parallèle de S2, maintient la bobine alimentée après le relâchement de S2 : c'est l'**auto-maintien**.
- Un appui sur **S1** coupe la bobine : le moteur s'arrête et ne redémarre pas sans nouvel appui sur S2.
- En cas de surcharge, **F2** ouvre son contact 95-96 : la bobine est coupée et le moteur s'arrête.

## Tests réalisés en simulation

| Test | Résultat attendu | Résultat |
| --- | --- | --- |
| Appui sur S2 | Le moteur démarre | ✅ |
| Relâchement de S2 | Le moteur continue de tourner (auto-maintien) | ✅ |
| Appui sur S1 | Le moteur s'arrête et ne redémarre pas seul | ✅ |
| Déclenchement de F2 | Le moteur s'arrête ; S2 ne permet pas de redémarrer avant réarmement | ✅ |

## Captures

| À l'arrêt | En marche | Défaut thermique |
| --- | --- | --- |
| ![Arrêt](captures/arret.png) | ![Marche](captures/marche.png) | ![Défaut thermique](captures/defaut-thermique.png) |

## Ce que j'ai appris

- **Puissance et commande sont deux circuits séparés** : ils ne sont reliés que par les repères des appareils (la bobine KM pilote les contacts KM, le relais F2 commande son contact 95-96).
- **NF ou NO** : l'arrêt utilise un contact normalement fermé, la marche un contact normalement ouvert.
- **Le relais thermique a ses propres contacts auxiliaires** (95-96 et 97-98) : un contact générique ne réagit pas à son déclenchement.
- **On ne shunte jamais une protection** : un fil qui contourne un fusible ou un contact de sécurité supprime la protection de toute l'installation.

## Fichiers

- `schema.cad` : schéma CADe_SIMU
- `captures/` : captures de la simulation