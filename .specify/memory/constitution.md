# Python Modular Monolith (PMM) Constitution

## Core Principles

### I. Strict Module Encapsulation
Each module owns its specific data models and business logic. Cross-module database access is strictly forbidden; no module may directly query tables belonging to another. This ensures each module remains independent and reduces side effects.

### II. Interface-First (Public APIs)
Interaction between modules must only occur via well-defined public interfaces, typically a `public.py` exposed at the module's root. This decouples implementation details and enables future extraction of a module into a microservice with minimal effort.

### III. Shared Nucleus (Core Module)
Foundation code, such as centralized routing, global middleware, authentication logic, and common utility types, resides in a `core/` or `base/` module. Modules depend on `core/`, but `core/` never depends on feature modules.

### IV. Loose Coupling via Internal Events
For side-effect-heavy cross-module operations (e.g., sending an email when a user registers), use an internal async event bus. This prevents temporal coupling and avoids blocking a module's core process with secondary tasks from another module.

### V. High Component Testability
Every module must be independently testable. Unit tests should mock external module dependencies to verify the module's logic in isolation. Integration tests are reserved for verifying contract compliance between modules.

## Architectural Constraints

### Technology Stack
- **Web Framework**: FastAPI (v0.100+)
- **ORM & Models**: SQLModel / SQLAlchemy (v2.0+)
- **Data Validation**: Pydantic v2
- **Migrations**: Alembic (using partitioned migration paths)
- **Formatting**: Ruff, Black, and MyPy for strict typing

### Modular Structure
All feature modules must follow a standard internal structure:
- `models.py`: Database schemas (SQLModel)
- `schemas.py`: Request/Response Pydantic models
- `service.py`: Business logic and orchestration
- `router.py`: API endpoints
- `public.py`: Exposed interfaces for other modules
- `tasks.py`: Background or event-driven handlers

## Development Workflow

### Rule 1: Module Creation
To add a feature, create a new directory in `src/modules/[module_name]`. Register the module's router in `src/main.py` using a centralized registration pattern.

### Rule 2: TDD & Documentation
New features must include tests under `src/modules/[module_name]/tests/`. Documentation for public APIs in `public.py` is mandatory.

### Rule 3: Zero Circular Dependencies
Circular dependencies between modules are a violation of this constitution. If two modules are too tightly coupled, they should either be merged or their shared logic extracted to `core/`.

## Governance
This constitution supersedes all individual developer preferences. Amendments to these core principles require a formal architectural review and an update to this document. All Pull Requests must be checked for violations of module boundaries.

**Version**: 1.0.0 | **Ratified**: 2026-03-31 | **Last Amended**: 2026-03-31

<!-- Example: Version: 2.1.1 | Ratified: 2025-06-13 | Last Amended: 2025-07-16 -->
