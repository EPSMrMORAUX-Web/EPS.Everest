# 🏔️ Everest – Demi-fond (EPS)

Application web (un seul fichier HTML) pour animer et évaluer un cycle de demi-fond sous forme d'ascension de l'Everest.

## Principe

- Élèves en **binômes** (ou trios / élève seul) : un coureur, un observateur qui compte les tours.
- **1 tour = 100 m = 1 pièce**. Les pièces servent à acheter un **ticket** pour franchir chaque camp : montée, sommet, puis descente.
- L'enseignant clique sur le numéro du groupe pour ajouter (+) ou retirer (−) une pièce.

## Barème et prix des tickets

| Étape | Prix | Cumul | Distance / allure | Note /20 |
|---|---|---|---|---|
| Camp de base | 16 | 16 | 1,6 km(/h) | 0 |
| Camp 1 (6 100 m) | 9 | 25 | 2,5 km(/h) | 4 |
| Camp 2 (6 500 m) | 9 | 34 | 3,4 km(/h) | 7 |
| Camp 3 (7 400 m) | 7 | 41 | 4,1 km(/h) | 10 |
| Camp 4 (8 000 m) | 7 | 48 | 4,8 km(/h) | 12 |
| Sommet (8 848 m) | 4 | 52 | 5,2 km(/h) | 13 |
| Camp 4 (retour) | 4 | 56 | 5,6 km(/h) | 14 |
| Camp 3 (retour) | 7 | 63 | 6,3 km(/h) | 15 |
| Camp 2 (retour) | 7 | 70 | 7 km(/h) | 17 |
| Camp 1 (retour) | 9 | 79 | 7,9 km(/h) | 19 |
| Camp de base (retour) | 9 | 88 | 8,8 km(/h) | 20 |

Les prix des retours sont déduits du barème (modifiables dans le tableau `S` du script, champ `c`).

## Classes (onglets)

- Chaque classe a son **onglet** avec **ses élèves et ses groupes uniquement**. `＋ Classe` crée un onglet, `✏️` le renomme, `🗑` le supprime.
- **Import Excel / CSV** (colonnes *Nom*, *Prénom*, éventuellement *Classe*) : crée **toujours un nouvel onglet** (ou remplit l'onglet courant s'il est vide). Si le fichier contient plusieurs valeurs dans la colonne *Classe*, un onglet par classe est créé.
- **Import JSON de groupes** : crée aussi un **nouvel onglet**, sans rien mélanger avec la classe affichée.
- Les données sont enregistrées dans le navigateur (`localStorage`) et propres à l'appareil.
- **Mode jour / nuit** : bouton « 🌙 Nuit / ☀️ Jour » en haut de page. Par défaut, le thème suit celui de l'appareil ; le choix est mémorisé. Le PDF est toujours imprimé en clair.

## Groupes

- **Binômes dans l'ordre / aléatoires** à partir des élèves libres. Case « Si impair : former un trio » : le dernier élève rejoint le dernier groupe (sinon il est seul).
- **Absent / dispensé** : clic sur un élève de la liste (barré = exclu des groupes automatiques).
- **Modifier un groupe** : sur la carte, le menu **↪** à côté d'un élève le déplace vers un autre groupe, vers un **nouveau groupe solo**, ou le retire (sans groupe). Le menu **＋ ajouter un élève** complète un groupe (3 maximum). Un groupe vidé est supprimé.
- Les groupes sont toujours triés par numéro croissant.
- Un élève déplacé **prend la progression du groupe d'arrivée** (pièces et tickets du groupe) ; un nouveau groupe solo démarre à 0.

## Export / import de groupes (JSON)

Bouton **⬇ Exporter les groupes (JSON)** : classe, élèves (avec absent) et groupes (numéro, membres, tours, étape).

```json
{ "app": "everest-demi-fond", "version": 2, "classe": "2nde 4",
  "eleves": [ { "nom": "Lisa MARTIN", "absent": false } ],
  "groupes": [ { "numero": 1, "membres": ["Lisa MARTIN", "Maxime DURAND"], "tours": 0, "etape": -1 } ] }
```

L'import accepte aussi des formats simples venant d'autres applications : une liste de listes de noms `[["Lisa","Maxime"],["Sam","Inès"]]`, ou une liste d'objets avec `membres` / `members` / `eleves` (ou `a`, `b`, `c`).

## Séance et évaluation

- Les cartes affichent pièces en poche, total de tours, distance, étape et **note /20** ; la montagne montre la position de chaque groupe (colonne gauche : montée, droite : descente).
- Un ticket vert = le groupe peut payer le prochain camp. `↩` annule le dernier ticket et rend les pièces.
- **Remise à zéro** : pièces et tickets de la classe affichée (élèves et groupes conservés).
- **PDF d'évaluation** : saisir la séance/date, cliquer sur « 📄 PDF évaluation », puis **Enregistrer au format PDF** dans la fenêtre d'impression. Chaque élève est listé avec son groupe, ses tours, sa distance, sa dernière étape et sa note.
- **CSV** : mêmes données pour un tableur.
- La note correspond au dernier ticket acheté : une pièce non échangée ne compte pas.
