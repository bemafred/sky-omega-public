# grammar-meta-standard Infrastructure

## Overview

[grammar-meta-standard](https://github.com/canyala/grammar-meta-standard) is a community-maintained repository of versioned EBNF grammars for modern programming languages. It provides authoritative, formal grammar specifications that enable tools like Sky Omega to perform mechanical code reasoning.

**Repository:** https://github.com/canyala/grammar-meta-standard  
**License:** MIT  
**Status:** Active development, community contributions welcome

## Purpose

Grammar-meta-standard provides formal grammar specifications for:

- **Static analysis tools** — Parse and analyze code mechanically
- **Code generators** — Produce syntactically valid code by construction
- **Transpilers** — Translate between languages with formal guarantees
- **LLM-powered development tools** — Constrain generation, verify outputs
- **Semantic tooling** — Enable RDF/SPARQL integration for code knowledge graphs

## Relationship to Sky Omega

**Sky Omega uses grammar-meta-standard, but is independent from it:**

- **grammar-meta-standard:** Language grammar specifications (infrastructure)
- **Sky Omega:** Intelligent system using those specifications (application)

**Analogy:**
- grammar-meta-standard : Dictionary
- Sky Omega : Writer using the dictionary

Sky Omega enhances LLM reasoning with mechanical guarantees from grammars, but can function without them (falling back to probabilistic parsing). The grammars provide **precision where available**, not requirement.

## Technical Details

### Format

**Extended Backus-Naur Form (EBNF)**

Standard notation for context-free grammars:
```ebnf
function_def ::= "def" IDENTIFIER "(" params? ")" ":" suite
params ::= param ("," param)*
param ::= IDENTIFIER (":" type_hint)?
```

### Structure

**Versioned per language:**
```
grammars/
├── csharp/
│   ├── v12/
│   │   ├── lexical.ebnf    # Token definitions
│   │   └── syntax.ebnf     # Grammar rules
│   ├── v13/
│   └── v14/
├── python/
│   ├── v3.12/
│   └── v3.13/
└── javascript/
    ├── es2022/
    ├── es2023/
    └── es2024/
```

**Two-file approach:**
- `lexical.ebnf` — Defines tokens (keywords, operators, literals)
- `syntax.ebnf` — Defines grammar rules (how tokens combine)

### Coverage Status

| Language | Versions | Coverage | Status |
|----------|----------|----------|--------|
| C# | 12, 13, 14 | ~60% | Partial |
| Python | 3.12, 3.13 | ~55% | Partial |
| JavaScript | ES2022-2024 | ~50% | Partial |
| Java | 17, 21 | ~30% | Minimal |
| Go | 1.21, 1.22 | ~25% | Minimal |
| Turtle | 1.1 | ~80% | Good |
| SPARQL | 1.1 | ~75% | Good |

**Note:** Percentages indicate proportion of language features with complete grammar rules.

## Contributing

grammar-meta-standard welcomes contributions:

### **What We Need:**

1. **Grammar accuracy improvements** — Fix incorrect rules
2. **Coverage expansion** — Add missing language features
3. **New language support** — Add new languages entirely
4. **Semantic annotations** — Enrich grammars with semantic metadata
5. **Test suites** — Verify grammars against real-world code

### **How to Contribute:**

1. Fork the repository
2. Add/improve grammar files following EBNF standard
3. Include test cases demonstrating correctness
4. Submit pull request with clear description
5. Community review and integration

See: [Contributing Guide](https://github.com/canyala/grammar-meta-standard/blob/main/CONTRIBUTING.md)

### **Why Contribute?**

Your contributions:
- Improve tools for entire ecosystem (not just Sky Omega)
- Enable better code analysis, generation, and verification
- Build open infrastructure for AI-powered development
- Get credited in a growing community resource

## Design Principles

### **1. Versioned Specifications**

Languages evolve. Grammar-meta-standard tracks language versions explicitly:

- Python 3.12 vs 3.13 (new syntax features)
- C# 12 vs 13 vs 14 (language additions)
- JavaScript ES2022 vs ES2023 vs ES2024

**Why this matters:**
- Precise parsing for specific language versions
- Migration tooling knows exact semantic differences
- No ambiguity about "which Python" or "which JavaScript"

### **2. Test-Driven Validation**

Every grammar includes test suite:
```
grammars/python/v3.13/
├── lexical.ebnf
├── syntax.ebnf
└── tests/
    ├── valid/          # Should parse successfully
    │   ├── functions.py
    │   ├── classes.py
    │   └── ...
    └── invalid/        # Should fail to parse
        ├── syntax_errors.py
        └── ...
```

**Falsifiability:** If a grammar passes all tests, we have evidence it's correct. If it fails, we know exactly what's wrong.

### **3. Semantic Annotations (Future)**

Beyond syntax, grammars can include semantic metadata:

```ebnf
# Syntax rule with semantic annotation
function_def ::= "def" IDENTIFIER "(" params? ")" ":" suite
    [semantic: creates_scope, binds_name, executable_block]
```

Enables richer reasoning about code meaning, not just structure.

## Use Cases Beyond Sky Omega

### **IDE Tooling**
- Syntax highlighting
- Code completion
- Error detection
- Refactoring assistance

### **Static Analysis**
- Linters (enforce style/best practices)
- Security scanners (detect vulnerabilities)
- Complexity metrics (cyclomatic complexity, etc.)

### **Code Generation**
- Template engines
- DSL compilers
- Metaprogramming tools

### **Education**
- Teaching language design
- Compiler construction courses
- Parsing algorithm demonstrations

## Future Vision

### **Near-Term (6-12 months)**
- 80%+ coverage for C#, Python, JavaScript
- 10+ total languages with good coverage
- Robust test suites for all grammars
- Community contributors from multiple organizations

### **Medium-Term (1-2 years)**
- Full coverage for priority languages
- Semantic annotation standard
- Cross-language mapping specifications
- Integration with major IDE platforms

### **Long-Term (2-5 years)**
- Sky Omega helps maintain grammars (AI-assisted grammar development)
- Automated grammar extraction from language specifications
- Real-time grammar updates as languages evolve
- Industry-standard resource for AI-powered tooling

## Sky Omega's Role in the Ecosystem

As Sky Omega matures, she could:

### **1. Help Maintain Grammars**
- Parse language specification documents
- Generate EBNF rules from formal specs
- Propose improvements based on code analysis
- Validate grammars against large codebases

### **2. Generate Test Suites**
- Create comprehensive test cases for grammar validation
- Identify edge cases in language features
- Ensure grammars handle real-world code

### **3. Extract Grammars from Specifications**
- Read official language docs (Python PEP, ECMAScript spec, etc.)
- Generate formal grammars automatically
- Accelerate coverage expansion

### **4. Suggest Improvements**
- Analyze how developers actually use languages
- Identify commonly misunderstood syntax
- Recommend grammar clarifications

This creates a **virtuous cycle:**

```
Better grammars → Better tools → Better code analysis → Better grammars
```

## Relationship to E-Clean & Semantic Architecture

grammar-meta-standard embodies **E-Clean** principles:

### **Epistemic Clean**
- Grammars are **explicit** (formal EBNF)
- Grammars are **grounded** (in language specifications)
- Grammars are **falsifiable** (test suites prove correctness)

### **Semantic Architecture**
- Structure preserves meaning (grammar rules = semantic units)
- Platform constraints explicit (language-specific features documented)
- Versioning tracks semantic evolution

The grammar repository is **infrastructure that enables E-Clean tooling**. It provides the mechanical foundation that Sky Omega's semantic reasoning builds upon.

## Getting Started

### **For Tool Builders:**

```bash
# Clone repository
git clone https://github.com/canyala/grammar-meta-standard.git

# Use grammars in your tool
# Example: Parse Python 3.13 code
parser = load_grammar("grammars/python/v3.13/")
ast = parser.parse(source_code)
```

### **For Contributors:**

```bash
# Fork and clone
git clone https://github.com/YOUR_USERNAME/grammar-meta-standard.git

# Add/improve grammars
cd grammars/python/v3.13/
# Edit lexical.ebnf or syntax.ebnf

# Add test cases
cd tests/valid/
# Add test_my_feature.py

# Submit PR
git push origin my-grammar-improvement
```

### **For Researchers:**

Explore formal language theory in practice:
- Compare grammar evolution across versions
- Analyze language complexity metrics
- Study parser performance characteristics
- Investigate semantic annotation frameworks

## Community & Governance

**Current Status:** Maintained by Canyala Innovation with community contributions

**Future Governance:** As the project grows, governance will evolve to include:
- Contributor recognition system
- Language maintainer roles
- Review process for contributions
- Community roadmap input

**Communication:**
- GitHub Issues for bugs/features
- Discussions for design questions
- Pull Requests for contributions

## Acknowledgments

grammar-meta-standard builds on decades of formal language theory and parser technology. Special thanks to:

- Language designers who publish formal specifications
- Parser generator communities (ANTLR, Bison, PEG, etc.)
- Contributors who add and improve grammars
- Tools that rely on these grammars, providing feedback and validation

## License

**MIT License** — Free for commercial and non-commercial use.

You can:
- Use grammars in any project (open or closed source)
- Modify grammars for your needs
- Distribute grammars with your tools
- Build commercial products using these grammars

You must:
- Include original license and copyright notice
- Not hold authors liable for issues

## Contact & Support

- **Issues:** [GitHub Issues](https://github.com/canyala/grammar-meta-standard/issues)
- **Discussions:** [GitHub Discussions](https://github.com/canyala/grammar-meta-standard/discussions)
- **Email:** Open an issue instead (public discussion benefits everyone)

---

*For how Sky Omega uses these grammars, see [Grammar-Aware Reasoning](../capabilities/grammar-aware-reasoning.md) and [Grammar-Meta-Standard Integration](../integration/grammar-meta-standard-integration.md). For foundational principles, see [E-Clean & Semantic Architecture](../e-clean-and-semantic-architecture.md).*
