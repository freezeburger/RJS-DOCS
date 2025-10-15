# 🎯 Exercice TypeScript / React — `useApi` Hook

## 🧠 Objectif pédagogique

Valides concepts suivants en TypeScript dans un contexte React :
- Typage d’objets via `interface`
- Utilisation de `union types`
- Encapsulation logique via un `hook personnalisé`
- Asynchrone et gestion d’état dans un hook

---

## 📝 Énoncé

### 🔧 Contexte

Vous travaillez sur une application React qui consomme une API externe (https://dummyjson.com/products). Vous devez créer un hook personnalisé `useApi` qui :
1. Permet de **récupérer la liste des produits** avec leurs titres et prix uniquement.
2. Permet de **récupérer les détails d’un produit** via la méthode `getDetail(id: number)`.

---

### 📐 Contraintes TypeScript

- Vous devez utiliser :
  - Une **interface** pour représenter la **réponse de détail** d’un produit
  - Une **union de type** pour restreindre les **catégories**
  - Une **interface** pour représenter le **produit simplifié** (title, price, id)

---

### 💡 Aide - Types à définir

```ts
export type ProductCategory = 'beauty' | 'smartphones' | 'fragrances' | 'skincare'; // etc.

export interface ProductPreview {
  id: number;
  title: string;
  price: number;
}

export interface ProductDetail extends ProductPreview {
  description: string;
  category: ProductCategory;
  rating: number;
  stock: number;
  thumbnail: string;
}

```

### Fecthing par défaut

> Utiliser : 'https://dummyjson.com/products?select=title,price'


### Fecthing de détail 

> > Utiliser : 'https://dummyjson.com/products/ID'


###  Spécification du Hook

```ts
const { data, loading, error, getDetail } = useApi();
```