# Grammar-Meta-Standard Integration

## Overview

This document describes how Sky Omega integrates with [grammar-meta-standard](https://github.com/canyala/grammar-meta-standard) to enable grammar-aware code reasoning. The integration provides mechanical code understanding and generation capabilities that complement LLM-based probabilistic reasoning.

## Architecture

### Component Relationship

```
┌─────────────────────────────────────────────────────────────┐
│                      Sky Omega                               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐              │
│  │   Sky    │◄──►│   Lucy   │◄──►│  James   │              │
│  │(Intelligence)  │ (Memory) │    │(Orchestration)          │
│  └──────────┘    └──────────┘    └──────────┘              │
│       ▲               ▲                ▲                     │
│       │               │                │                     │
│       └───────────────┴────────────────┘                     │
│                       │                                      │
│              ┌────────▼─────────┐                           │
│              │  Grammar Parser   │                           │
│              │   & Generator     │                           │
│              └────────┬─────────┘                           │
│                       │                                      │
└───────────────────────┼──────────────────────────────────────┘
                        │
                        ▼
        ┌───────────────────────────────────┐
        │    grammar-meta-standard          │
        │  (External Infrastructure)        │
        ├───────────────────────────────────┤
        │  • EBNF Grammar Specifications    │
        │  • Versioned per Language         │
        │  • Community Maintained           │
        └───────────────────────────────────┘
```

## Integration Points

### 1. Grammar Loading

**Process:**
- James orchestrates grammar retrieval for target language/version
- Grammar files (lexical.ebnf + syntax.ebnf) loaded from repository
- Grammars cached in Lucy for reuse across sessions

**Example:**
```
Request: "Refactor this Python 3.13 function"
→ Load grammars/python/v3.13/lexical.ebnf
→ Load grammars/python/v3.13/syntax.ebnf
→ Cache in Lucy with key: grammar:python:3.13
```

### 2. Code Parsing

**Process:**
- Input code tokenized according to lexical grammar
- Tokens parsed according to syntax grammar
- AST generated with semantic annotations
- AST stored in Lucy (RDF representation)

**Semantic Annotations:**
Each AST node can be enriched with:
- Type information
- Scope bindings
- Control flow metadata
- Semantic equivalence classes

### 3. Semantic Storage (Lucy/RDF)

**Lucy stores:**
```turtle
@prefix code: <http://sky-omega.se/ontology/code#> .
@prefix lang: <http://sky-omega.se/ontology/language#> .

code:Function_abc123
    a code:FunctionDefinition ;
    code:name "calculate_total" ;
    code:language lang:Python ;
    code:version "3.13" ;
    code:ast [ 
        a code:AST ;
        code:structure "..." ;
    ] ;
    code:semantics [
        code:inputs ( code:Param_price code:Param_quantity ) ;
        code:outputs code:Float ;
        code:sideEffects false ;
    ] .
```

This RDF representation enables:
- Semantic queries across codebase
- Pattern matching on code structure
- Retrieval of semantically similar code
- Verification of semantic equivalence

### 4. Grammar-Constrained Generation

**Process:**
- Sky understands user intent (LLM reasoning)
- James retrieves relevant patterns from Lucy
- Generator produces code constrained by grammar rules
- Output validated against grammar before presentation

**Example workflow:**
```
User: "Generate a function to compute moving average"

Sky → Intent: Function with numeric input/output, iteration pattern
Lucy → Retrieves: Similar functions, relevant patterns
James → Orchestrates: Grammar-constrained synthesis
Generator → Produces: Valid AST per grammar rules
Validator → Confirms: Syntactically valid output
```

## Mechanical Code Understanding

### Parsing Pipeline

```
Source Code
    ↓
Lexical Analysis (tokens)
    ↓
Syntactic Analysis (AST)
    ↓
Semantic Annotation
    ↓
RDF Storage (Lucy)
    ↓
Queryable Knowledge Graph
```

### Advantages Over Probabilistic Parsing

| Aspect | Probabilistic (LLM only) | Grammar-Aware (Sky Omega) |
|--------|-------------------------|---------------------------|
| **Validity** | Likely valid | Guaranteed valid (where grammar covers) |
| **Understanding** | Pattern-based | Structure-based |
| **Verification** | Test-driven | Formal verification possible |
| **Refactoring** | Best-effort | Semantic-preserving |
| **Confidence** | Statistical | Mechanical |

## Constrained Code Generation

### Production Rule Application

Grammar rules limit generation space:

```
# Grammar rule (simplified)
function_def ::= "def" IDENTIFIER "(" params? ")" ":" suite

# Valid generation
def calculate(x, y):
    return x + y

# Invalid generation (blocked by grammar)
def calculate x, y:     # Missing parentheses
    return x + y
```

Generator **cannot produce** syntactically invalid code when constrained by grammar.

## Semantic Reasoning

### Equivalence Checking

**Given two code snippets:**
```python
# Version A
def total(items):
    sum = 0
    for item in items:
        sum += item
    return sum

# Version B  
def total(items):
    return sum(items)
```

**Sky Omega can verify:**
1. Both parse to valid ASTs
2. Semantic annotations match (input/output types, side effects)
3. Behavioral equivalence (through test generation or formal methods)
4. Store equivalence class in Lucy

### Cross-Language Translation

**Grammar-to-grammar mapping:**

```
Python Grammar              C++ Grammar
─────────────────          ────────────────
list comprehension    →    range-based for loop
generator expression  →    iterator pattern
context manager      →    RAII wrapper
```

Mappings stored in Lucy enable reliable translation with semantic preservation guarantees.

## Current Limitations & Roadmap

### **Current State**

**Grammar Coverage:**
- C#: ~60% of language features
- Python: ~55% of language features
- JavaScript: ~50% of language features
- Other languages: Minimal coverage

**Implications:**
- Mechanical guarantees only for covered constructs
- Fallback to probabilistic parsing for gaps
- Hybrid approach: mechanical + probabilistic

### **Roadmap**

**Phase 1 (Current):**
- Core language features for C#, Python, JavaScript
- Basic semantic annotations
- Proof-of-concept integration

**Phase 2 (Near-term):**
- 80%+ coverage for priority languages
- Advanced semantic annotations (control flow, data flow)
- Semantic equivalence checking

**Phase 3 (Medium-term):**
- Full language coverage
- Cross-language translation
- Self-modification capabilities

**Phase 4 (Long-term):**
- Sky contributes to grammar improvements
- Automated grammar extraction from specs
- Community-driven grammar ecosystem

## Handling Gaps

When encountering uncovered constructs:

```python
# Example: Walrus operator (recent Python feature)
if (n := len(data)) > 10:  # May not be in grammar yet
    process(n)
```

**Sky Omega's approach:**
1. **Attempt grammar parse** → Fails on `:=` token
2. **Flag as uncovered** → Mark uncertainty
3. **Fallback to LLM** → Use probabilistic understanding
4. **Store pattern** → Learn for future grammar improvement
5. **Honest reporting** → User knows guarantee doesn't apply here

## Integration with E-Clean Principles

Grammar-meta-standard embodies **E-Clean** principles:

### **Epistemic Clean**
- Grammars are **explicit** (formal EBNF notation)
- Grammars are **grounded** (in language specifications)
- Grammars are **falsifiable** (test suites verify correctness)

### **Semantic Architecture**
- Structure preserves meaning (grammar rules ≈ semantic units)
- Platform constraints explicit (language-specific features documented)
- Versioning tracks semantic evolution (Python 3.12 vs 3.13)

The grammar repository is **infrastructure that enables E-Clean tooling**.

## Future Capabilities

As integration matures:

### **Self-Modification**
Sky can:
- Parse her own source code
- Understand architectural structure mechanically
- Propose improvements with formal verification
- Execute changes with semantic preservation guarantees

### **Automated Documentation**
Generate documentation from semantic annotations:
```
Function: calculate_total
Inputs: price (float), quantity (int)
Output: float
Side effects: None
Semantic property: Commutative in inputs
```

### **Intelligent Debugging**
- Parse error traces with grammar awareness
- Identify syntactic vs semantic errors mechanically
- Suggest fixes constrained by valid grammar productions

## Technical Implementation Notes

**Parser Generator:**
- EBNF grammars compiled to parser code
- Incremental parsing for large codebases
- Error recovery for partial/invalid code

**RDF Schema:**
- Custom ontology for code semantics
- Integrates with Lucy's RDF store
- SPARQL queries for code patterns

**Performance:**
- Grammar loading: O(1) amortized (cached)
- Parsing: O(n) in code length
- Semantic queries: O(log n) with indexing

---

*For user-facing capabilities, see [Grammar-Aware Reasoning](../capabilities/grammar-aware-reasoning.md). For the grammar repository itself, see [Grammar-Meta-Standard Infrastructure](../infrastructure/grammar-meta-standard.md).*
