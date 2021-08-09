---
path: "/create-data-relationships"
title: "Create Relationships Between Data Nodes"
order: "2B"
section: "Data Customization"
description: "TKTK"
---

TKTK

```diff
  exports.sourceNodes = ({ actions, createNodeId, createContentDigest }) => {
-   const { createNode } = actions;
+   const { createNode, createTypes } = actions;

    const authors = [
      {
        slug: 'n-k-jemisin',
        name: 'N. K. Jemisin',
      },
      {
        slug: 'blake-crouch',
        name: 'Blake Crouch',
      },
      {
        slug: 'fredrik-backman',
        name: 'Fredrik Backman',
      },
    ];

    const books = [
      {
        isbn: 9780316229296,
        name: 'The Fifth Season',
        author: 'n-k-jemisin',
        series: 'The Broken Earth Trilogy',
        seriesOrder: 1,
      },
      {
        isbn: 9780316229265,
        name: 'The Obelisk Gate',
        author: 'n-k-jemisin',
        series: 'The Broken Earth Trilogy',
        seriesOrder: 2,
      },
      {
        isbn: 9780316229241,
        name: 'The Stone Sky',
        author: 'n-k-jemisin',
        series: 'The Broken Earth Trilogy',
        seriesOrder: 3,
      },
      {
        isbn: 9781101904244,
        name: 'Dark Matter',
        author: 'blake-crouch',
        series: null,
        seriesOrder: null,
      },
      {
        isbn: 9781476738024,
        name: 'A Man Called Ove',
        author: 'fredrik-backman',
        series: null,
        seriesOrder: null,
      },
    ];

    authors.forEach((author) => {
      createNode({
        ...author,
        id: createNodeId(`author-${author.slug}`),
        parent: null,
        children: [],
        internal: {
          type: 'Author',
          content: JSON.stringify(author),
          contentDigest: createContentDigest(author),
        },
      });
    });

    books.forEach((book) => {
      createNode({
        ...book,
        id: createNodeId(`book-${book.isbn}`),
        parent: null,
        children: [],
        internal: {
          type: 'Book',
          content: JSON.stringify(book),
          contentDigest: createContentDigest(book),
        },
      });
    });
+
+   createTypes(`
+     type Author implements Node {
+       id: ID!
+       name: String!
+       slug: String!
+       books: [Book!]! @link(from: "slug" by: "author.slug")
+     }
+
+     type Book implements Node {
+       id: ID!
+       name: String!
+       isbn: ID!
+       series: String
+       seriesOrder: Int
+       author: Author @link(from: "author" by: "slug")
+     }
+   `);
  };
```