# Web Worker Meetup Saar (Juni 2020) - GraphQL und Prisma

Keynote und Infos zum WebWorker Meetup Saar Vortrag zu GraphQL und Prisma im Juni 2020.

- [Web Worker Meetup Saar (Juni 2020) - GraphQL und Prisma](#web-worker-meetup-saar-juni-2020---graphql-und-prisma)
  - [Info](#info)
  - [Demo](#demo)
    - [1 Service starten](#1-service-starten)
    - [2 React Client starten](#2-react-client-starten)
    - [3 GraphQL Playground](#3-graphql-playground)
    - [4 Feld zu `Item` hinzufügen](#4-feld-zu-item-hinzufügen)
    - [5 Header und Items via Mutation anlegen](#5-header-und-items-via-mutation-anlegen)
  - [Feedback](#feedback)


## Info

Alle Links und Hintergrund- Infos finden sich in den [Slides](wwsaar-graphql-prisma.pdf).

## Demo

Verwendet [sventropy/prisam2-boilerplate](https://github.com/sventropy/prisma2-boilerplate).

### 1 Service starten

```sh
npm i
docker-compose down
docker-compose up -d
npm run migrate
npm run generate
npm run seed
npm start
```


### 2 React Client starten

```sh
cd clients/react-client
npm i
npm start
```

### 3 GraphQL Playground

Läuft unter <http://localhost:9222>

```graphql
{
  headers {
    title
    items {
      title
      type
      createdAt
    }
  }
}
```

**Docs** und **Schema** zeigen.

### 4 Feld zu `Item` hinzufügen

1. Feld in `schema.prisma` hinzufuegen

```graphql
model Item {
    ...
    description    String? 
    ...
}
```

2. Deployment

```sh
npm run migrate
npm start
```

3. Playground prüfen <http://localhost:9222>. Feld noch nicht exponiert.

4. Add field to `src/types/Item.ts`

```graphql
...
t.model.description();
...
```

5. Nochmal playground prüfen <http://localhost:9222>. Feld ist da aber leer.

6. Seed script anpassen um Feld zu befüllen...

```ts
description: lorem.sentence(),
```

... und Testdaten neu generieren

```sh
npm run reset
npm run seed
```

6. Client query ändern

```graphql
  query HeaderItemQuery {
    headers {
      title
      items {
        title
        type
        createdAt
        description
      }
    }
  }
```

7. Client API neu generieren

```sh
npm run codegen
```

8. `App.tsx` anpassen um Feld anzuzeigen

```html
...
<td>Description</td>
...
<td>{i.description}</td>
...
```

### 5 Header und Items via Mutation anlegen

Auf branch `wwsaar-final` wechseln.

```sh
git checkout wwsaar-final
```

*Wird nach dem Talk in `master` gemerged.*

## Feedback

Feedback jederzeit gerne über meine [Website](https://hennessen.net) oder direkt auf [Twitter](https://twitter.com/svenhennessen). Cheers! 👋🏻