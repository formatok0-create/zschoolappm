# Z SCHOOL — Systeme d'exploitation personnel d'apprentissage

Application Android premium (Kotlin + Jetpack Compose) pour organiser, suivre et terminer vos formations en ligne. Interface entierement en francais, code source en anglais.

## Ouvrir le projet

1. Ouvrir le dossier dans **Android Studio** (Ladybug ou plus recent).
2. Laisser Gradle se synchroniser (AGP 8.6, Kotlin 2.0, JDK 17).
3. Lancer sur un appareil / emulateur **Android 8.0 (API 26)** minimum.

> Build optimise : pas de Hilt, pas de material-icons-extended, KSP limite a Room,
> caches Gradle actives (`parallel`, `caching`, `configuration-cache`).

Ou en ligne de commande :

```bash
./gradlew assembleDebug
```

## Architecture

- **Clean Architecture / MVVM** : UI (Compose) → ViewModel → Repository → Room.
- **Injection manuelle ultra-legere** : le graphe (Room, Repository, DataStore) vit dans `ZSchoolApp` en `lazy`, chaque ViewModel expose une `Factory`. Zero processeur d'annotations Dagger → builds beaucoup plus rapides.
- **Offline-first** : Room est la source de verite locale, tout fonctionne sans reseau.
- **Sauvegarde cloud Supabase** : compte email/mot de passe, snapshot automatique toutes les 24h (WorkManager) + a chaque lancement en ligne. Restauration automatique sur un nouvel appareil.
- **Room** : formations, modules, competences, notes, ressources, sessions de concentration (cles etrangeres en cascade).

## Fonctionnalites

- Navigation inferieure a 3 onglets : Accueil, Tableau de bord, Reglages (masquee hors ecrans principaux)
- Accueil : en-tete avec compteur, recherche integree, tri (date/nom/progression/duree), filtres par categorie
- Cartes de formation animees : accent par formation, bordure neon respirante, flottement, halo, ombre coloree
- Logo circulaire importe depuis la galerie (Photo Picker, copie en stockage interne)
- Balayer pour supprimer + dialogue de confirmation
- FAB qui se masque pendant le defilement
- CRUD complet des formations
- Ecran de detail avec onglets : Aperçu · Modules · Compétences · Notes · Ressources · Statistiques · Paramètres
- Modules : nom, duree, date prevue, case a cocher, minuteur de concentration
- Minuteur plein ecran avec halo respirant et enregistrement des sessions
- Competences synchronisees automatiquement vers la page globale
- Tableau de bord global avec graphiques (barres et courbe 7 jours) dessines au Canvas
- Export PDF de l'historique (PdfDocument natif + partage via FileProvider)
- Ecran d'edition dedie avec selecteur de couleur d'accent et logo galerie
- Reglages : theme Clair / Sombre / Systeme (DataStore), haptique, animations

## Structure

```
app/src/main/java/com/zschool/app
├── data/local (entities, DAOs, ZDatabase)
├── data/repository (LearningRepository)
├── ui/theme (couleurs, typo, glassmorphism)
├── ui/components (GlassCard, FormationCard, AccentBar, SwipeToDelete, ZIcons)
├── ui/screens (home, formation, edit, timer, skills, dashboard, settings, intro)
├── ui/navigation (ZNavGraph)
└── util (PdfExporter, ImageStore, formats francais)
```

## Credits

Icones emoji (medecine, matieres premieres, elevage) : [Twemoji](https://github.com/jdecked/twemoji) — graphiques sous licence CC-BY 4.0.
