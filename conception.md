# Conception de l'app

Ce que j'ai decide apres avoir teste axe DevTools sur mon site de test.

---

## Pour qui

**Un developpeur web qui teste son propre site** avant de le mettre en ligne.

Il connait deja HTML et CSS. Pas besoin de lui expliquer ce qu'est un `<label>`.
Ce qu'il veut c'est : ou est le probleme, et quoi ecrire a la place.

J'ai choisi lui parce que je peux me mettre a sa place. Un auditeur qui facture
des rapports ou un gestionnaire qui veut un tableau de bord, je devrais les
inventer.

---

## Le probleme que je veux regler

Ce developpeur a deja axe DevTools. Gratuit, dans son navigateur, deux clics.

Donc si mon app affiche juste une liste de problemes, elle sert a rien.

Mais mon test montre qu'**axe rate plus de la moitie des problemes**. Sur mes
20 defauts caches, il en a trouve 9.

Les 11 qu'il rate ont un point commun : le code est **formellement correct**,
c'est le **sens** qui manque.

Exemple : `<button>☰</button>`. Pour axe le bouton a un nom, donc c'est bon.
Mais un lecteur d'ecran lit "trigramme pour le ciel".

C'est ce trou que mon app veut combler.

---

## Ce que l'app fait

Deux choses :

**1. Elle trouve ce qu'axe rate.**
Une IA relit la page, y compris ce qu'axe a declare correct, et repere ce qui
n'a pas de sens.

**2. Elle donne le correctif.**
axe dit "ce champ n'a pas de label". Mon app donne le code corrige, pret a
copier.

Les deux vont ensemble. Trouver un probleme sans donner la solution, c'est
frustrant. Donner des solutions a ce qu'axe trouve deja, c'est pas suffisant
pour justifier une app.

---

## L'ecran principal

Une seule liste de problemes, avec une etiquette qui dit d'ou vient
l'information :

```
⚠️  Image sans texte alternatif            [axe]
⚠️  Le bouton ☰ n'a pas de nom lisible     [IA]
⚠️  Contraste insuffisant                   [axe]
⚠️  Le lien "Cliquez ici" ne dit pas ou il mene   [IA]
```

J'ai hesite entre une liste melangee et deux sections separees.

J'ai choisi la liste melangee avec etiquettes parce que c'est plus court a lire,
et l'etiquette suffit a savoir d'ou vient l'info. Je peux ajouter un filtre
"voir seulement l'IA" si besoin.

---

## Une ligne de resultat

Quand le developpeur clique sur un probleme, il voit :

**Tout de suite**
1. Le code fautif
2. Une phrase : c'est quoi le probleme
3. Le code corrige + bouton copier

**Replie derriere "Details"**
4. Pourquoi c'est un probleme
5. La regle WCAG concernee
6. L'emplacement exact dans la page

**Dans un coin**
- La gravite (pastille de couleur)
- Un bouton "marquer comme corrige"

L'idee : celui qui sait deja ce qu'il fait corrige en 10 secondes. Celui qui
veut comprendre deplie.

---

## Truc pas encore regle : est-ce qu'on croit l'IA ?

Quand axe dit qu'il y a un probleme, c'est sur. C'est une regle, point.

Quand l'IA le dit, je sais pas. Elle peut se tromper.

Donc il faut que le dev puisse verifier lui-meme. Soit je mets un niveau de
confiance, soit j'explique pourquoi l'IA pense ca.

J'ai pas encore choisi.

---

## Architecture

### Comment on donne la page a analyser

Deux facons :

- coller une URL
- ou coller directement le HTML et le CSS

J'ai mis la deuxieme parce que mon utilisateur teste son site **avant** de le
publier. Souvent il est sur localhost, donc mon serveur peut pas l'atteindre.

Au depart je voulais juste le HTML, mais sans le CSS je perds tous les defauts
de contraste. Avec les deux, le navigateur calcule les vraies couleurs.

Ce qui reste perdu avec le collage : le contenu genere par JavaScript.

### Le chemin complet

```
URL collee  ─┐
             ├─→  navigateur sans fenetre  →  axe  →  IA  →  resultats
HTML + CSS  ─┘
```

Un seul moteur d'analyse, deux facons de le nourrir. Je code la partie
"navigateur + axe" une seule fois.

### Ce que l'IA recoit

Pour la v1 : **la page entiere**.

C'est pas optimal, ca coute plus cher. Mais mon site de test fait 2 petites
pages, donc le probleme existe pas encore. Je prefere commencer simple.

Quand ca deviendra trop lent ou trop cher, je passerai a : page entiere mais
nettoyee. J'enleve le contenu des `<script>`, les commentaires, et je coupe les
longs paragraphes. Je garde tous les elements, donc je rate rien.

### Stockage

Rien du tout. Je garde juste l'analyse precedente dans le navigateur, le temps
de la session.

Ca suffit pour comparer avant/apres : le dev analyse, corrige, relance, et voit
ce qui a ete regle.

Pas de base de donnees. Mon utilisateur corrige son site en une fois, il revient
pas le lendemain. Et s'il revient, il relance l'analyse, ca prend 10 secondes.

### Technologies

**Node partout.**

axe-core et Playwright sont ecrits en JavaScript. Les lancer depuis Java voudrait
dire demarrer un deuxieme service juste pour ca. Trop de complication pour rien.

Je connais pas Node. Mais j'ai deja vu un script qui fait navigateur + axe, donc
j'ai une idee de a quoi ca ressemble. Je vais apprendre en construisant.

---

## A faire

- [ ] Choisir comment gerer la confiance dans les resultats IA
- [ ] Dessiner les ecrans (accueil, resultats, detail)
- [ ] Choisir quel modele d'IA utiliser
