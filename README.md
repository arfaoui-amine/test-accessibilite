# Site de test d'accessibilité

## Ce que j'ai fait

J'ai fabrique un faux site (une clinique, 2 pages) et j'ai cache **20 erreurs
d'accessibilite dedans**, expres.

Apres je l'ai passe a l'extension **axe DevTools** dans Chrome pour voir combien
elle en trouve toute seule.

C'est comme tester un correcteur d'orthographe : tu ecris un texte avec des
fautes que tu connais deja, et tu comptes combien il en attrape.

---

## Le resultat

Sur `index.html` :

- **3 erreurs** en mode WCAG seulement
- **5 erreurs** quand j'active aussi les "bonnes pratiques"

Sur `contact.html` : **4 erreurs**

Donc entre 7 et 9 erreurs trouvees, sur 20 que j'avais mises.


---


## Ce que l'outil trouve

Il trouve bien :
- pas de `lang` sur `<html>`
- texte gris trop pale
- image sans `alt`
- titre de page vide
- label pas relie au champ
- select sans label
- bouton illisible (blanc sur turquoise pale)

---

## Ce que l'outil rate

| Mon erreur | Pourquoi il la rate |
|---|---|
| lien avec juste 🔍 | pour lui, l'emoji compte comme du texte |
| bouton avec juste ☰ | pareil |
| "Cliquez ici" x2 | le texte existe et il est pas vide, donc c'est bon pour lui |
| `alt="image"` | l'attribut est la, donc c'est bon pour lui |
| faux bouton (`div onclick`) | il voit un div normal |
| placeholder au lieu du label | il accepte le placeholder comme nom |
| rouge tout seul pour le 911 | faut comprendre que l'info depend de la couleur |
| etoile rouge = obligatoire | faut comprendre que c'est explique nulle part |
| tableau sans `<th>` | il sait pas si c'est un tableau de donnees ou de mise en page |
| radio sans `<fieldset>` | mon `<p>` ressemble a une question mais c'en est pas une |
| message d'erreur tout seul | faut relier une phrase a un champ |
| `outline: none` | faut tester au clavier pour le voir |

---

## Ce que j'en retiens pour l'app

**1. L'outil automatique c'est la base, pas la solution.**
Il attrape moins de la moitie des erreurs, sur un site ou je les connais toutes.

**2. Les erreurs interessantes sont dans ce qu'il declare "correct".**
Le bouton ☰, le lien 🔍, le placeholder : ils sont pas dans la liste des
erreurs. Donc une app qui affiche juste les erreurs de l'outil les verra
jamais.

**3. Les erreurs ratees se ressemblent.**
Je vois 4 familles :

- le nom existe mais veut rien dire
- l'info passe seulement par la couleur
- faut tester pour voir (clavier)
- la structure est ambigue

Chaque famille demande un type de verification different. Ca peut servir a
organiser la partie IA.

---

## Pour refaire le test

1. Ouvrir `index.html` dans Chrome
2. F12 → onglet axe DevTools → "Scanner la page entiere"
