# Defigner

A semantic data modeling and visualization platform built in 2010-2011. Typed objects with URI-based identity, domain/range constraints, and content-addressed persistence — built on top of Alessandro Warth's [Worlds](http://www.vpri.org/pdf/tr2011001_final_worlds.pdf) research for transactional isolation.

## What's in here

- **Thing model** -- A typed object system where every entity has a URI identity, typed properties with domain/range constraints, subtype inheritance, and CouchDB persistence. Properties are themselves Things — the schema is self-describing.
- **Worlds** -- Alessandro Warth's transactional isolation system. `sprout()` a child context, modify in isolation, `commit()` back with automatic conflict detection. (Warth's code, used here as the transaction layer.)
- **OMeta compiler** -- Warth's pattern-matching DSL, used here to compile declarative model definitions into executable JavaScript.
- **Graph visualization** -- Interactive DAG rendering of type hierarchies using Protovis.

## The Thing model

```javascript
// Everything is a Thing with a URI
thing._uri = 'uri:furniture/table'

// Properties have typed constraints
// domain: what can have this property
// range: what values are allowed
property('uri:height', { domain: 'uri:furniture', range: 'uri:number' })

// Types form a DAG
table.ofType(furniture)  // furniture is a supertype
furniture.hasProperties(['uri:height', 'uri:material'])

// Persisted to CouchDB via URI-keyed documents
thing.store()  // content-addressed by URI
```

The URI is the identity — like content-addressable storage, the reference is derived from what the thing *is*, not an arbitrary key.

## 2026 context

This project predates the current AI agent era by 15 years, but the typed semantic object model is directly relevant to work being done today at [Daslab](https://daslab.dev):

- **Typed tool interfaces** -- The Thing model (typed objects with constraint validation and composability) is essentially what modern agent frameworks reinvent for tool definitions
- **Content-addressed identity** -- URI-based identity maps to content-addressable storage for workflow artifacts
- **Declarative-to-executable compilation** -- The OMeta pipeline (declarative model → AST → executable code) is the same architecture used in [autocompile](https://github.com/mirkokiefer/autocompile), which compiles AI agent execution traces into deterministic programs
- **Transactional isolation for evolving compilations** -- The Worlds concept (sprout, modify, merge with conflict detection) is the mechanism needed for compiled workflows that evolve as new traces are observed

## History

Built Dec 2010 — Jan 2011 by Mirko Kiefer and Graham McLeod. Transactional isolation and OMeta by [Alessandro Warth](http://www.tinlizzie.org/~awarth/) (VPRI/UCLA).
