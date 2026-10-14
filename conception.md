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

## ce qui est pas encore regle : est-ce qu'on croit l'IA ?

Quand axe dit qu'il y a un probleme, c'est sur. C'est une regle, point.

Quand l'IA le dit, je sais pas. Elle peut se tromper.

Donc il faut que le dev puisse verifier lui-meme. Soit je mets un niveau de
confiance, soit j'explique pourquoi l'IA pense ca.

J'ai pas encore choisi.

---

## A faire

- [ ] Choisir comment gerer la confiance dans les resultats IA
- [ ] Dessiner les ecrans (accueil, resultats, detail)
- [ ] Proposer l'architecture technique
