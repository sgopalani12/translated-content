---
title: indexedDB
slug: Web/API/indexedDB
tags:
  - API
  - IndexedDB
  - Propriété
  - Reference
  - WindowOrWorkerGlobalScope
translation_of: Web/API/indexedDB
original_slug: Web/API/WindowOrWorkerGlobalScope/indexedDB
---
{{APIRef}}

La propriété globale en lecture seule **`indexedDB`** fournit un mécanisme qui permet aux applications d'accéder aux bases de données indexées de façon asynchrone.

## Syntaxe

```js
var IDBFactory = self.indexedDB;
```

### Valeur de retour

Un objet {{domxref("IDBFactory")}}.

## Exemples

```js
var db;
function openDB() {
 var DBOpenRequest = window.indexedDB.open('toDoList');
 DBOpenRequest.onsuccess = function(e) {
   db = DBOpenRequest.result;
 }
}
```

## Spécifications

{{Specifications}}

## Compatibilité des navigateurs

{{Compat}}

## Voir aussi

- [Utiliser IndexedDB](/fr/docs/Web/API/API_IndexedDB/Using_IndexedDB)
- Initier une connexion : {{domxref("IDBDatabase")}}
- Utiliser les transactions : {{domxref("IDBTransaction")}}
- Définir un intervalle de clés : {{domxref("IDBKeyRange")}}
- Récupérer et modifier les données : {{domxref("IDBObjectStore")}}
- Utiliser les curseurs {{domxref("IDBCursor")}}
- Exemple de référence : [To-do Notifications](https://github.com/mdn/to-do-notifications/tree/gh-pages) ([exemple _live_](https://mdn.github.io/to-do-notifications/)).
