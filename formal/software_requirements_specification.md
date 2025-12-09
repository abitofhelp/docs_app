# Software Requirements Specification (SRS)

**Version:** 2.0.0<br>
**Date:** 2025-12-08<br>
**SPDX-License-Identifier:** BSD-3-Clause<br>
**License File:** See the LICENSE file in the project root<br>
**Copyright:** © 2025 Michael Gardner, A Bit of Help, Inc.<br>
**Status:** Released

---

## 1. Introduction

### 1.1 Purpose

This Software Requirements Specification (SRS) describes the functional and non-functional requirements for Hybrid_App_Ada, a professional Ada 2022 application starter template demonstrating hexagonal architecture with functional programming principles.

### 1.2 Scope

Hybrid_App_Ada provides:
- Professional 5-layer hexagonal architecture implementation
- Static dependency injection via Ada generics
- Railway-oriented programming with Result monads
- Clean architecture boundary enforcement
- Comprehensive test suite with custom framework
- Production-ready code quality standards
- Educational documentation and examples

### 1.3 Definitions and Acronyms

| Term | Definition |
|------|------------|
| DDD | Domain-Driven Design - strategic and tactical patterns for complex software |
| Hexagonal Architecture | Ports & Adapters pattern isolating business logic from infrastructure |
| Result Monad | Functional pattern for error handling without exceptions |
| SPARK | Ada subset for formal verification |
| CLI | Command Line Interface |
| DI | Dependency Injection |
| DTO | Data Transfer Object |

### 1.4 References

- Ada 2022 Reference Manual (ISO/IEC 8652:2023)
- SPARK 2014 Reference Manual
- Clean Architecture (Robert C. Martin)
- Domain-Driven Design (Eric Evans)
- Railway-Oriented Programming (Scott Wlaschin)
- Hexagonal Architecture (Alistair Cockburn)
- functional crate v3.0.0 documentation

---

## 2. Overall Description

### 2.1 Product Perspective

Hybrid_App_Ada is a standalone application implementing hexagonal (ports and adapters) architecture with clean separation between domain logic, application use cases, infrastructure adapters, presentation layer, and bootstrap.

**Architecture Layers:**

| Layer | Purpose |
|-------|---------|
| Domain | Pure business logic, value objects, error types (zero dependencies) |
| Application | Use cases, commands, ports (interfaces) |
| Infrastructure | Driven adapters implementing outbound ports |
| Presentation | Driving adapters (CLI, API) calling inbound ports |
| Bootstrap | Composition root wiring all dependencies |

```text
┌──────────────────────────────────────────────────────┐
│                    Bootstrap                          │
│          (Composition Root - wires everything)        │
└────────────────────────┬─────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────┐
│                   Presentation                        │
│      CLI Adapter (driving - calls Application)        │
└────────────────────────┬─────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────┐
│                   Application                         │
│  Use Cases + Inbound/Outbound Ports + Commands       │
└────────────────────────┬─────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────┐
│                  Infrastructure                       │
│    Driven Adapters (implements outbound ports)       │
└────────────────────────┬─────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────┐
│                     Domain                            │
│  Entities + Value Objects + Business Rules (PURE)    │
└──────────────────────────────────────────────────────┘
```

### 2.2 Product Features

1. **Hexagonal Architecture**: 5-layer clean architecture
2. **Static Dependency Injection**: Compile-time wiring via generics
3. **Railway-Oriented Programming**: Result monad error handling
4. **Architecture Enforcement**: Automated boundary validation
5. **Test Infrastructure**: Custom test framework
6. **Build Automation**: Comprehensive Makefile
7. **Formal Verification**: SPARK-compatible domain layer
8. **Result Combinators**: v2.0.0 adds Bimap, Ensure, With_Context, Fallback, Recover, Tap

### 2.3 User Classes

| User Class | Description |
|------------|-------------|
| Application Developers | Learn hexagonal architecture patterns |
| Team Leads | Adopt architectural standards |
| Educators | Teach clean architecture principles |
| Ada Developers | Start new projects with best practices |

### 2.4 Operating Environment

| Requirement | Specification |
|-------------|---------------|
| Platforms | POSIX (Linux, macOS, BSD), Windows 11, Embedded |
| Ada Compiler | GNAT FSF 13+ or GNAT Pro |
| Ada Version | Ada 2022 |
| Build System | Alire 2.0+, GNU Make |
| Dependencies | functional ^3.0.0 |

---

## 3. Functional Requirements

### 3.1 Domain Layer (FR-01)

**Priority**: Critical
**Description**: Pure business logic with zero external dependencies.

| ID | Requirement |
|----|-------------|
| FR-01.1 | Value objects must be immutable (validated at construction) |
| FR-01.2 | Validation must occur in value object creation (Create function) |
| FR-01.3 | Domain must have zero infrastructure dependencies |
| FR-01.4 | Business rules must be pure functions (no side effects) |
| FR-01.5 | Result monads must handle all errors (no exceptions) |
| FR-01.6 | Domain provides raw data (Get_Name), not formatted output |

### 3.2 Application Layer (FR-02)

**Priority**: Critical
**Description**: Use case orchestration, port definitions, output formatting.

| ID | Requirement |
|----|-------------|
| FR-02.1 | Define inbound ports (use case interfaces) |
| FR-02.2 | Define outbound ports (infrastructure interfaces) |
| FR-02.3 | Implement use cases using Domain logic |
| FR-02.4 | Commands must be DTOs with larger bounds than Domain validation |
| FR-02.5 | Re-export Domain.Error for Presentation access |
| FR-02.6 | Format output at application layer (Format_Greeting) |

### 3.3 Infrastructure Layer (FR-03)

**Priority**: High
**Description**: Concrete adapter implementations.

| ID | Requirement |
|----|-------------|
| FR-03.1 | Implement outbound port interfaces |
| FR-03.2 | Adapt external systems to Domain types |
| FR-03.3 | Handle infrastructure exceptions at boundaries |
| FR-03.4 | Convert exceptions to Result errors |
| FR-03.5 | Provide console writer adapter |

### 3.4 Presentation Layer (FR-04)

**Priority**: High
**Description**: User interface adapters (CLI).

| ID | Requirement |
|----|-------------|
| FR-04.1 | Cannot access Domain layer directly |
| FR-04.2 | Must use Application.Error for error handling |
| FR-04.3 | Must use Application.Command for input |
| FR-04.4 | Command line argument parsing |
| FR-04.5 | User-friendly error messages |
| FR-04.6 | Exit code mapping (0=success, 1=error) |

### 3.5 Bootstrap Layer (FR-05)

**Priority**: High
**Description**: Composition root with dependency wiring.

| ID | Requirement |
|----|-------------|
| FR-05.1 | Wire all generic instantiations |
| FR-05.2 | Connect ports to adapters |
| FR-05.3 | Minimal main procedure (delegate to Bootstrap) |
| FR-05.4 | Single-project structure (no aggregates) |
| FR-05.5 | Static wiring (compile-time resolution) |

### 3.6 Error Handling (FR-06)

**Priority**: Critical
**Description**: Railway-oriented programming with Result monad.

| ID | Requirement |
|----|-------------|
| FR-06.1 | No exceptions across layer boundaries |
| FR-06.2 | Result monad for all fallible operations |
| FR-06.3 | Error types with kind and message |
| FR-06.4 | Error Kinds: Validation_Error, Parse_Error, Not_Found_Error, IO_Error, Internal_Error |
| FR-06.5 | Is_Ok/Is_Error predicates |
| FR-06.6 | Value/Error_Info accessors |
| FR-06.7 | And_Then_Into for cross-type Result chaining |
| FR-06.8 | Bimap for transforming both Ok and Error values (v2.0.0) |
| FR-06.9 | Ensure for postcondition validation (v2.0.0) |
| FR-06.10 | With_Context for error context enrichment (v2.0.0) |
| FR-06.11 | Fallback for default values on error (v2.0.0) |
| FR-06.12 | Recover for converting errors to Ok values (v2.0.0) |
| FR-06.13 | Tap for side effects without changing Result (v2.0.0) |

### 3.7 Dependency Injection (FR-07)

**Priority**: Critical
**Description**: Static DI via Ada generics.

| ID | Requirement |
|----|-------------|
| FR-07.1 | Generic packages for use cases |
| FR-07.2 | Generic function parameters for ports |
| FR-07.3 | Compile-time instantiation in Bootstrap |
| FR-07.4 | Zero runtime overhead |
| FR-07.5 | Type-safe wiring |
| FR-07.6 | No heap allocation for DI |

### 3.8 Architecture Validation (FR-08)

**Priority**: High
**Description**: Automated boundary enforcement.

| ID | Requirement |
|----|-------------|
| FR-08.1 | Validate Presentation cannot access Domain |
| FR-08.2 | Validate Infrastructure can access Domain |
| FR-08.3 | Validate Application accesses Domain only |
| FR-08.4 | Validate Domain has zero dependencies |
| FR-08.5 | Python script for validation (arch_guard.py) |
| FR-08.6 | Make target integration (make check-arch) |

---

## 4. Non-Functional Requirements

### 4.1 Performance (NFR-01)

| ID | Requirement |
|----|-------------|
| NFR-01.1 | Static dispatch overhead: 0 (compile-time resolution) |
| NFR-01.2 | No heap allocation in hot paths |
| NFR-01.3 | Result monad overhead: minimal (stack-based) |
| NFR-01.4 | Build time: < 30 seconds (clean build) |
| NFR-01.5 | Test execution: < 5 seconds (all tests) |

### 4.2 Reliability (NFR-02)

| ID | Requirement |
|----|-------------|
| NFR-02.1 | All tests must pass (100% pass rate) |
| NFR-02.2 | No memory leaks |
| NFR-02.3 | Deterministic error handling (no exceptions) |
| NFR-02.4 | Type-safe boundaries (compile-time verification) |

### 4.3 Portability (NFR-03)

| ID | Requirement |
|----|-------------|
| NFR-03.1 | Support POSIX platforms (Linux, macOS, BSD) |
| NFR-03.2 | Support Windows platforms |
| NFR-03.3 | Support embedded platforms via custom adapters |
| NFR-03.4 | Standard Ada 2022 (no compiler-specific features) |
| NFR-03.5 | No platform-specific code in Domain/Application |
| NFR-03.6 | Alire-compatible project structure |

### 4.4 Maintainability (NFR-04)

| ID | Requirement |
|----|-------------|
| NFR-04.1 | Clear layer separation (enforced by arch_guard.py) |
| NFR-04.2 | Self-documenting code with docstrings |
| NFR-04.3 | > 90% test coverage |
| NFR-04.4 | Zero compiler warnings |
| NFR-04.5 | Consistent code style |

### 4.5 Usability (NFR-05)

| ID | Requirement |
|----|-------------|
| NFR-05.1 | Quick Start Guide for beginners |
| NFR-05.2 | Working examples in under 5 minutes |
| NFR-05.3 | Clear error messages |
| NFR-05.4 | Comprehensive documentation |
| NFR-05.5 | Make target help system |

### 4.6 Platform Abstraction (NFR-06)

| ID | Requirement |
|----|-------------|
| NFR-06.1 | Application layer defines abstract outbound ports using pure function signatures |
| NFR-06.2 | Infrastructure layer provides platform-specific adapters implementing ports |
| NFR-06.3 | Composition roots (Bootstrap.CLI, Bootstrap.Windows, Bootstrap.Embedded) wire adapters to ports |
| NFR-06.4 | Domain types used in port signatures, not infrastructure types |
| NFR-06.5 | New platforms addable without modifying application layer |
| NFR-06.6 | All platform adapters testable via mock implementations |

### 4.7 SPARK Formal Verification (NFR-07)

| ID | Requirement |
|----|-------------|
| NFR-07.1 | Domain layer shall pass SPARK legality checking |
| NFR-07.2 | All domain packages shall use `SPARK_Mode => On` |
| NFR-07.3 | No runtime errors provable in domain layer (overflow, range, division) |
| NFR-07.4 | All domain variables shall be properly initialized before use |
| NFR-07.5 | Pre/postconditions on domain operations shall be proven correct |
| NFR-07.6 | SPARK legality verification shall be runnable via `make spark-check` |
| NFR-07.7 | SPARK proof verification shall be runnable via `make spark-prove` |
| NFR-07.8 | Infrastructure/Presentation/Bootstrap layers may use `SPARK_Mode => Off` |

**Verification Scope:**

| Layer | SPARK_Mode | Rationale |
|-------|-----------|-----------|
| Domain | On | Pure business logic, provable |
| Application | On | Use cases, ports (mostly pure) |
| Infrastructure | Off | I/O operations |
| Presentation | Off | CLI/UI operations |
| Bootstrap | Off | Wiring and composition |

### 4.8 Testability (NFR-08)

| ID | Requirement |
|----|-------------|
| NFR-08.1 | Pure functions in Domain (easy to test) |
| NFR-08.2 | Port abstraction for test doubles |
| NFR-08.3 | Custom test framework (no external dependencies) |
| NFR-08.4 | Test organization by type (unit/integration/e2e) |
| NFR-08.5 | Coverage analysis support (GNATcoverage) |

---

## 5. System Requirements

### 5.1 Hardware Requirements

| Category | Requirement |
|----------|-------------|
| CPU | Any modern processor |
| RAM | 64 MB minimum |
| Disk | 10 MB minimum |

### 5.2 Software Requirements

| Category | Requirement |
|----------|-------------|
| Operating System | Linux, macOS, BSD, Windows 11 |
| Compiler | GNAT FSF 13+ or GNAT Pro |
| Build System | Alire 2.0+, GNU Make |

---

## 6. System Constraints

### 6.1 Technical Constraints

| ID | Constraint |
|----|------------|
| SC-01 | Must compile with GNAT FSF 13+ or GNAT Pro |
| SC-02 | Must use Ada 2022 language features |
| SC-03 | Must be Alire-compatible |
| SC-04 | Single-project structure (no aggregate projects) |
| SC-05 | Must use functional crate ^3.0.0 for Result monad |

### 6.2 Design Constraints

| ID | Constraint |
|----|------------|
| SC-06 | Must enforce hexagonal architecture boundaries |
| SC-07 | Presentation cannot access Domain directly |
| SC-08 | Domain must have zero external dependencies |
| SC-09 | Must use static dispatch (generics, not interfaces) |
| SC-10 | No exceptions across layer boundaries |
| SC-11 | DTO bounds larger than Domain validation bounds |

---

## 7. Verification and Validation

### 7.1 Verification Methods

| Method | Description |
|--------|-------------|
| Code Review | All code reviewed before merge |
| Static Analysis | Zero compiler warnings |
| Dynamic Testing | All tests must pass |
| Coverage Analysis | > 90% line coverage |
| Architecture Validation | arch_guard.py enforcement |

### 7.2 Traceability Matrix

| Requirement | Design Element | Test Coverage |
|-------------|----------------|---------------|
| FR-01 | Domain.Value_Object.Person | Unit tests |
| FR-02 | Application.Usecase.Greet | Unit + Integration |
| FR-03 | Infrastructure.Adapter.Console_Writer | Integration |
| FR-04 | Presentation.Adapter.CLI.Command.Greet | E2E tests |
| FR-05 | Bootstrap.CLI | E2E tests |
| FR-06 | Domain.Error.Result | All tests |
| FR-07 | Generic instantiation | Compile-time |
| FR-08 | arch_guard.py | Python tests |

---

## 8. Appendices

### 8.1 Glossary

See Section 1.3 Definitions and Acronyms.

### 8.2 Layer Responsibilities Summary

| Layer | Responsibilities | Dependencies | Tests |
|-------|------------------|--------------|-------|
| Domain | Business logic, validation | NONE | Unit |
| Application | Use cases, ports, formatting | Domain | Unit + Integration |
| Infrastructure | Adapters (driven) | App + Domain | Integration |
| Presentation | UI (driving) | Application ONLY | E2E |
| Bootstrap | DI wiring | ALL | E2E |

### 8.3 Error Handling Flow

```text
Domain Validation Error
    ↓
Result[Value, Error] returned
    ↓
Application checks Is_Error
    ↓
If error: propagate up via Result
    ↓
Presentation pattern matches Error_Kind via Application.Error
    ↓
User-friendly message displayed
    ↓
Exit code 1
```

### 8.4 Version 2.0.0 Changes

**Breaking Changes:**
- Upgraded to functional ^3.0.0 (incompatible with v1.x)

**New Requirements:**
- FR-06.8 through FR-06.13: Result combinator operations
- Windows CI support in GitHub Actions

**Test Metrics:**
- 85 unit tests
- 16 integration tests
- 8 e2e tests
- Total: 109 tests (all passing)

---

**Document Control:**
- Version: 2.0.0
- Last Updated: 2025-12-08
- Status: Released

**Change History:**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 2.0.0 | 2025-12-08 | Michael Gardner | Upgraded to functional ^3.0.0; added Result combinators (FR-06.8-13); updated test counts (101 total) |
| 1.0.0 | 2025-12-01 | Michael Gardner | Initial release aligned with hybrid architecture |
