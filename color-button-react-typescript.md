# 🎯 Exercice React + TypeScript : Bouton coloré avec union de types

## Objectif

Créer un composant React `ColorButton` en TypeScript qui respecte les contraintes suivantes :

- Il accepte uniquement **trois couleurs** : `"red"`, `"green"`, `"blue"` via une **union de types**.
- Il déclenche une action `onClick` passée en prop.
- Il affiche un libellé facultatif, sinon une version par défaut.

---

## 💡 Exigences techniques

- TypeScript doit **refuser toute autre couleur non listée** (`"yellow"`, `"black"`, etc.).
- Le bouton utilise une **coloration de fond** (`backgroundColor`) correspondant à la couleur choisie.
- La couleur du texte est blanche par défaut.
- Le bouton est stylisé simplement avec du `style` en ligne.

---

## 🔧 Exemple d'utilisation

```tsx
<ColorButton color="red" onClick={() => alert("clicked!")} />
```

##  🔄 Bonus (facultatifs)

- Ajouter une propriété variant: 'solid' | 'outline'
- Ajouter une bordure dynamique ou un effet hover
