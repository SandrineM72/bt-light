# bt-light

Petite application web (PWA) pour piloter la lumière de l'ampoule enceinte **Sengled Solo Color Plus**, dont l'application officielle (Sengled Pulse) n'est plus maintenue et ne détecte plus l'ampoule sur les versions récentes d'Android.

👉 **En ligne :** https://bt-light.culturecook.fr

## Fonctionnalités

- Allumer / éteindre la lumière
- Choisir la teinte (roue chromatique complète)
- Régler l'intensité
- Mémorisation des derniers réglages
- Installable sur l'écran d'accueil, en plein écran

Le **son** n'a pas besoin de l'application : l'ampoule s'appaire comme une enceinte Bluetooth classique (« Solo Color Plus »).

## Prérequis

| Élément | Détail |
|---|---|
| Navigateur | **Chrome sur Android** (Web Bluetooth n'est pas pris en charge par Firefox, Safari ni Samsung Internet) |
| Téléphone | Bluetooth **et** localisation activés |
| Ampoule | Allumée à l'interrupteur, module lumière visible sous le nom `S21…` |
| Hébergement | HTTPS obligatoire (exigence de Web Bluetooth) |

## Utilisation

1. Ouvrir la page dans Chrome Android.
2. Menu **⋮** → **Installer l'appli**.
3. Lancer l'appli → **Connecter l'ampoule** → choisir l'appareil `S21…`.

À savoir :

- Chrome demande de choisir l'ampoule à chaque ouverture (règle de sécurité de Web Bluetooth).
- Une seule connexion au module lumière est possible à la fois : fermer toute autre appli connectée à l'ampoule.
- Ne pas appairer le module `S21…` dans les réglages Bluetooth d'Android, cela bloque la connexion.

## Technique

- Page unique `index.html` (HTML, CSS, JavaScript), sans dépendance ni étape de build.
- Communication via l'API **Web Bluetooth** (Bluetooth basse consommation).
- `manifest.webmanifest` + icônes pour l'installation en PWA.
- Les commandes de couleur sont mises en file d'attente : une seule écriture Bluetooth à la fois, les couleurs intermédiaires sont écrasées pour garder un curseur fluide.

## Structure

```
index.html             Application (interface + logique Bluetooth)
manifest.webmanifest   Manifeste PWA
icon-192.png           Icône 192 × 192
icon-512.png           Icône 512 × 512 (maskable)
```

## Déploiement

Contenu statique servi par Caddy :

```
bt-light.culturecook.fr {
    root * /srv/bt-light
    file_server {
        hide .git
    }
}
```

Mise à jour : `git pull` dans `/srv/bt-light`.

## Pistes d'évolution

- Mode lumière blanche (température et luminosité)
- Service worker pour l'ouverture hors connexion
