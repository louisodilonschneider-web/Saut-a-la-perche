# PerchePro — Application de Saut à la Perche

Application web HTML autonome pour le suivi des entraînements et la gestion des concours de saut à la perche. Aucune installation requise, tout fonctionne directement dans le navigateur.

---

## Fonctionnalités

### Module Entraînements

- Enregistrement de chaque séance avec : date, perche utilisée, marque au sol, prise de mains, hauteur passée (avec et/ou sans contact), deux commentaires libres
- Historique complet de toutes les séances sous forme de tableau
- **4 graphiques d'évolution** :
  - Hauteur passée (avec/sans contact)
  - Marque au sol
  - Prise de mains
  - Perches utilisées (fréquence par modèle)
- Suppression individuelle d'une séance
- Données persistées dans le `localStorage` du navigateur (aucun serveur requis)

### Module Concours

- Création d'un concours avec : nom, date, lieu, hauteur de départ, incrément standard
- Saisie des athlètes : prénom (obligatoire), nom, numéro de licence, club
- **Gestion complète du déroulement** :
  - Feuille de concours avec ordre de passage
  - 3 tentatives par hauteur pour chaque athlète
  - Résultat de chaque tentative : Réussi (O) / Échec (X) / Impasse (—)
  - Élimination automatique après 3 échecs consécutifs
  - Mécanique d'**impasse** : un athlète peut renoncer à ses tentatives restantes sur une hauteur pour rejoindre les concurrents à la hauteur suivante, en conservant ses essais restants
  - Passage à la hauteur suivante avec hauteur personnalisable
- **Classement automatique** selon les règles officielles :
  1. Meilleure hauteur passée (desc)
  2. Nombre d'échecs total (tie-break)
  3. Ordre de passage initial
- **Export PDF** du classement incluant :
  - Tableau de classement complet avec parcours (O/X/—) pour chaque hauteur
  - Détail individuel du parcours de chaque athlète
  - Médailles 🥇🥈🥉 pour les trois premiers

---

## Règles officielles implémentées

| Règle | Implémentation |
|---|---|
| 3 tentatives par hauteur | Compteur par athlète et par hauteur |
| Élimination à 3 échecs | Statut `Éliminé` automatique |
| Impasse | Bouton dédié, conservation des tentatives restantes |
| Ordre de passage | Numérotation fixe par ordre d'inscription |
| Départ pour égalité | Tri par nombre d'échecs total |
| Notation O/X/— | Affichée dans feuille de concours et PDF |

---

## Utilisation

### Démarrage rapide

1. Ouvrir `poleVault.html` dans un navigateur (Chrome, Firefox, Edge, Safari)
2. Aucune connexion internet requise après ouverture
3. Les données sont sauvegardées automatiquement dans le navigateur

### Enregistrer une séance d'entraînement

1. Aller dans l'onglet **Entraînements**
2. Cliquer sur **+ Nouvelle séance**
3. Remplir les champs souhaités (seule la date est obligatoire)
4. Valider → la séance apparaît dans le tableau et les graphiques se mettent à jour

### Créer et gérer un concours

1. Aller dans l'onglet **Concours**
2. Cliquer sur **+ Nouveau concours**
3. Renseigner les informations du concours et les athlètes
4. Valider → le concours s'ouvre directement en mode **En cours**
5. Pour chaque athlète, cliquer sur **▶ Saisir** pour enregistrer le résultat d'une tentative
6. Cliquer sur **📏 Passer à la hauteur suivante** pour monter la barre
7. Consulter le **classement** en temps réel et exporter en PDF

---

## Architecture technique

| Composant | Technologie |
|---|---|
| Interface | HTML5 / CSS3 / JavaScript (vanilla) |
| Graphiques | Chart.js 4.4 (CDN Cloudflare) |
| Persistance | `localStorage` (navigateur) |
| Export PDF | Fenêtre d'impression native du navigateur |
| Dépendances serveur | Aucune |

### Structure de données

**Séance d'entraînement**
```json
{
  "date": "2025-06-15",
  "pole": "Skypole 4m20 / 70kg",
  "ground": "430",
  "grip": "390",
  "heightTouch": "340",
  "heightClean": "330",
  "c1": "Bonne impulsion, regarder le ciel",
  "c2": "Travailler l'enroulement"
}
```

**Athlète en concours**
```json
{
  "first": "Louis",
  "last": "Dupont",
  "license": "12345678",
  "club": "Athlé Paris",
  "attempts": {
    "300": ["fail", "success"],
    "310": ["success"],
    "320": ["fail", "fail", "fail"]
  },
  "bestHeight": 310,
  "eliminated": true,
  "remainingAttempts": 0,
  "order": 0
}
```

---

## Limites et évolutions possibles

- Les données sont locales au navigateur : si le cache est vidé, les données sont perdues. Prévoir une **sauvegarde manuelle** via export JSON (évolution possible).
- L'application ne gère pas encore les **barrage** (jump-off) pour la première place.
- Le décalage d'ordre de passage pour athlètes engagés simultanément dans une autre épreuve est géré manuellement par l'officiel (aucune automatisation).
- Pour une utilisation multi-appareils, une synchronisation via Firebase ou similaire peut être ajoutée (voir projet HoopStat).

---

## Compatibilité navigateurs

| Navigateur | Support |
|---|---|
| Chrome / Edge | ✅ Complet |
| Firefox | ✅ Complet |
| Safari (iOS/macOS) | ✅ Complet |
| Internet Explorer | ❌ Non supporté |

---

*PerchePro — Développé pour les perchistes organisateurs de compétitions. Version 1.0*
