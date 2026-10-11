# pdf-bym

Livres d'édification de l'application **BYM**, convertis depuis leurs PDF au
format JSON que l'application télécharge à la volée.

Ce dépôt n'est pas embarqué dans l'APK : la Bibliothèque, onglet « Livres »,
affiche la pochette (asset local) puis récupère le texte ici au moment du
téléchargement. Un dépôt privé renverrait 404 sans jeton — d'où la
publication publique.

## Fichiers

Le nom du fichier est le **code** de l'ouvrage du catalogue, en minuscules.

| Fichier | Code | Titre |
| --- | --- | --- |
| `ministere.json` | `MINISTERE` | LE MINISTÈRE OU LE SEIGNEUR |
| `animalite.json` | `ANIMALITE` | L'ANIMALITÉ DE L'ÊTRE HUMAIN |

L'URL consommée par l'application est du type :

```
https://raw.githubusercontent.com/victordiaz1000/pdf-bym/main/<code>.json
```

## Format

Produit par `appCodebar/pdf_book_to_json.py` :

```bash
python pdf_book_to_json.py "LE-MINISTERE-OU-LE-SEIGNEUR.pdf" ministere.json \
    --code MINISTERE [--dry-run] [--cover ../bible_app/assets/covers/ministere.jpg]
```

```jsonc
{
  "code": "MINISTERE",
  "title": "LE MINISTÈRE OU LE SEIGNEUR",
  "subtitle": "…",
  "author": "…",
  "publisher": "…",
  "isbn": "…",
  "rights": "…",
  "toc":    [ { "title": "…", "chapter": 3, "level": 1 }, … ],
  "chapters": [ { "blocks": [ { "type": "p|quote|li|pair|h2|h3|h4", "text": "…" } ] } ]
}
```

`toc` porte le sommaire (titre, niveau, chapitre) ; `chapters` les chapitres,
dans l'ordre. Une citation y ajoute `reference`, une ligne du tableau `gloss`,
et un paragraphe touché par une note de bas de page `textWithNotes` + `notes`
(invariant : `text[position:position+len(word)] == word`).

## Ajouter un ouvrage

1. Convertir le PDF et relire le JSON à l'œil.
2. Pousser `<code>.json` ici.
3. Renseigner l'`url` dans `bible_app/lib/data/work_catalog.dart`.
4. Ajouter la pochette dans `bible_app/assets/covers/` (déjà déclarée dans
   `pubspec.yaml`).
