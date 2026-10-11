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

L'URL consommée par l'application est du type :

```
https://raw.githubusercontent.com/victordiaz1000/pdf-bym/main/<code>.json
```

## Format

Produit par `appCodebar/pdf_book_to_json.py` :

```bash
python pdf_book_to_json.py "LE-MINISTERE-OU-LE-SEIGNEUR.pdf" -o ministere.json --cover
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
  "chapters": [ { "blocks": [ { "type": "p|quote|reference|li|pair|h2|h3|h4", "text": "…" } ] } ]
}
```

`chapters[0]` est le sommaire ; `chapters[1..N]` sont les chapitres. Les notes
de bas de page voyagent dans les blocs `note` rattachés à leur paragraphe.

## Ajouter un ouvrage

1. Convertir le PDF et relire le JSON à l'œil.
2. Pousser `<code>.json` ici.
3. Renseigner l'`url` dans `bible_app/lib/data/work_catalog.dart`.
4. Ajouter la pochette dans `bible_app/assets/covers/` (déjà déclarée dans
   `pubspec.yaml`).
