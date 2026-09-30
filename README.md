# Fiche résumé — Jour 3 : Migration, cas pratique et bilan de faisabilité

> Formation Tailwind CSS · TechLearn Studio · Question centrale du jour : est-ce que Tailwind répond aux besoins de CSD, et comment migrer concrètement casd.eu ?

---

## 1. Les 3 stratégies de migration Bootstrap → Tailwind

| Stratégie | Principe | Avantage | Limite |
|---|---|---|---|
| **Page par page** | Bootstrap et Tailwind cohabitent, on migre les pages une à une | Chaque page migrée est un livrable fini et testable | Sur 1000+ pages, calendrier qui se compte en années ; deux design systems visibles en parallèle |
| **Par composant** ⭐ recommandé pour CSD | On migre d'abord header, footer, boutons, cartes — les composants transverses réutilisés partout | Gain visible instantanément sur toutes les pages, risque maîtrisé, un composant migré profite à toutes les pages qui l'utilisent | Demande de bien identifier les composants vraiment transverses en amont |
| **Refonte complète** | Nouveau site entièrement repensé en Tailwind | Base la plus "propre" | Projet à part entière, viable seulement si une refonte est de toute façon prévue |

📖 **Recommandation posée à Arnaud :** la migration par composant, vu le volume de pages, l'équipe de 4 devs et l'obligation RGAA de CSD.

---

## 2. Cohabitation technique Bootstrap + Tailwind

- Les deux frameworks peuvent tourner **sur la même page** sans conflit : Tailwind génère des classes utilitaires atomiques (`bg-brand-600`, `px-4`) qui n'entrent quasiment jamais en collision avec les noms de classes Bootstrap (`.btn`, `.card`)
- Seul point de vigilance réel : **l'ordre de chargement** des feuilles de style — si un même élément portait à la fois une classe Bootstrap et une classe Tailwind touchant la même propriété CSS, c'est la feuille chargée en dernier qui l'emporte
- Bonne pratique : un composant est **soit 100% Bootstrap, soit 100% Tailwind**, jamais un mélange des deux sur le même élément

---

## 3. L'argument performance

| | Bootstrap complet (non purgé) | Tailwind buildé et purgé |
|---|---|---|
| Poids CSS typique | ~150-200 Ko | ~10-20 Ko |
| Pourquoi | Livre TOUTES les classes, même celles jamais utilisées | Le mécanisme de purge ne génère que les classes réellement présentes dans le HTML/les templates |

📖 **Vérification faite en live :** build du CSS avec `npx tailwindcss ... --minify`, comparaison des tailles de fichiers (`ls -lh`), puis confirmation dans l'onglet **Network** des DevTools sur les deux versions du formulaire NovaRecrute (avant/après).

⚠️ Sur un site avec des milliers de pages et un public majoritairement mobile comme casd.eu, ce delta de poids a un impact direct sur les temps de chargement — donc sur la conformité aux critères de performance d'un audit RGAA complet.

---

## 4. Le cas pratique de l'après-midi (hors partie Twig)

Reprise d'une vraie page (idéalement casd.eu) refaite intégralement en Tailwind, en autonomie, en 3 étapes chronométrées :

| Étape | Contenu |
|---|---|
| **1 — Structure** | HTML sémantique complet (`header`/`nav`/`main`/`footer`), hiérarchie de titres cohérente, réutilisation de composants déjà vus |
| **2 — Responsive** | Mobile-first sur TOUTE la page, testé réellement en réduisant la fenêtre à chaque breakpoint, pas juste imaginé |
| **3 — Accessibilité** | Navigation clavier complète, focus visible, contraste réel vérifié, `alt` pertinents |

📖 Posture du formateur : check-in courts toutes les heures, intervention seulement sur blocage réel de plus de 5-10 min — l'objectif est de voir si Arnaud est capable de refaire ça seul.

---

## 5. Grille de revue du cas pratique (4 angles)

| Angle | Points vérifiés |
|---|---|
| **Structure & sémantique** | Balises sémantiques, un seul `<h1>`, hiérarchie de titres sans saut, listes réelles |
| **Cohérence des classes** | Couleurs du thème plutôt qu'arbitraires, espacements cohérents, composants répétés à l'identique |
| **Responsive** | Aucun débordement horizontal à aucune largeur, texte lisible sans zoom mobile |
| **Accessibilité** | Focus toujours visible, ordre de tabulation logique, contraste texte ≥ 4.5:1 / UI ≥ 3:1 |

---

## 6. Bilan et plan d'action

**4 questions du bilan structuré :**
1. Le responsive mobile-first a-t-il répondu au besoin ?
2. Que retenir de la personnalisation d'une base graphique réutilisable sur des milliers de pages ?
3. Où se sent Arnaud sur l'accessibilité pour l'audit gouvernemental, par rapport à avant la formation ?
4. Quelle stratégie de migration est la plus réaliste pour une équipe de 4 devs ?

**Plan d'action en 4 étapes co-construit :**
1. Choisir un **site pilote** à faible risque
2. Migrer les **composants transverses en premier** (header, footer, boutons, cartes)
3. **Mesurer le gain** (poids CSS, DevTools) sur le site pilote migré
4. **Décider d'étendre** la stratégie selon le gain mesuré et le retour de l'équipe

---

## Ressources laissées à Arnaud

- Doc officielle Tailwind v3 : [v3.tailwindcss.com/docs](https://v3.tailwindcss.com/docs)
- Plugin formulaires : [github.com/tailwindlabs/tailwindcss-forms](https://github.com/tailwindlabs/tailwindcss-forms)
- Ressources RGAA officielles : [accessibilite.numerique.gouv.fr](https://accessibilite.numerique.gouv.fr)
- Vérificateur de contraste : [webaim.org/resources/contrastchecker](https://webaim.org/resources/contrastchecker/)