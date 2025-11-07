# Copilot Instructions for CBOrm Documentation

## Project Overview

This is the **GitBook documentation** for CBOrm (ColdBox ORM Extensions), a Hibernate ORM abstraction and enhancement layer for CFML engines (BoxLang, Lucee, Adobe ColdFusion). The documentation is hosted on GitBook and maintained at https://github.com/ortus-docs/cbox-cborm-docs.

**Source Code Repository**: https://github.com/coldbox-modules/cborm (separate from docs)

## Documentation Architecture

### GitBook Structure
- **SUMMARY.md**: Table of contents defining the entire navigation structure - **ALWAYS update this when adding/moving pages**
- **README.md**: Landing page (Introduction) with overview and features
- **Nested folders**: Each major section has a folder with a README.md (section landing) and individual pages
- **.gitbook/**: GitBook assets (images, diagrams) - use relative paths like `![](.gitbook/assets/image.png)`

### Content Organization (from SUMMARY.md)
```
├── README.md (Introduction)
├── intro/ (Release History, About This Book)
├── getting-started/ (Installation, Basic CRUD tutorials)
├── base-orm-service-1/ (Service Layer documentation)
│   └── service-methods/ (CRUD, Criteria, Querying, etc.)
├── virtual-services/ (Virtual Entity Services)
├── active-record/ (ActiveEntity pattern)
├── criteria-queries/ (Fluent query builders)
│   ├── criteria-builder/
│   └── detached-criteria-builder/
└── orm-events/ (Advanced features: REST, Events, Mementifier)
```

## Critical Documentation Conventions

### GitBook-Specific Markdown Syntax

**1. Code Blocks with Titles**
```markdown
{% code title="User.cfc" overflow="wrap" lineNumbers="true" %}
```javascript
component persistent="true" {
    property name="id" fieldtype="id" generator="native";
    property name="firstName";
}
```
{% endcode %}
```

**2. Multi-Language Examples (BoxLang + CFML)**
Always provide **BoxLang examples first**, then CFML:
```markdown
{% tabs %}

{% tab title="BoxLang" %}
BoxLang example code with groovy syntax highlighting
{% endtab %}

{% tab title="CFML" %}
CFML example code with javascript syntax highlighting
{% endtab %}

{% endtabs %}
```

**3. Callout Boxes**
```markdown
{% hint style="warning" %}
Important warning for users
{% endhint %}

{% hint style="danger" %}
Critical information that must be followed
{% endhint %}

{% hint style="info" %}
Helpful tip or additional information
{% endhint %}
```

**4. Page Headers (YAML Front Matter)**
```markdown
---
description: Brief description for SEO and page metadata
icon: terminal
---

# Page Title
```

### Language Syntax Highlighting
- **BoxLang**: Use `language="groovy"`
- **CFML/CFScript**: Use `language="javascript"` (CFScript uses JavaScript-like syntax)
- **SQL**: Use `language="sql"`
- **Bash**: Use `language="bash"`

## Content Patterns & Guidelines

### Example Code Requirements
1. **Real working examples**: All code must compile and run - test against actual CBOrm
2. **Complete context**: Show necessary imports, property injection, or setup
3. **Common use cases first**: Start with simple examples, progress to advanced
4. **Active Entity examples**: Always extend `cborm.models.ActiveEntity`
5. **Service injection**: Use WireBox DSL: `inject="entityService:EntityName"`

### Documentation Structure Per Page
```markdown
---
description: What this feature does (1-2 sentences)
---

# Feature Name

Brief introduction paragraph explaining the feature and when to use it.

## Basic Usage

Simple example showing the most common scenario.

## Arguments/Parameters

Table or list of all parameters with types and descriptions.

## Examples

### Example 1: Common Scenario
Code example with explanation

### Example 2: Advanced Scenario
Code example with explanation

## Related Topics

- Link to related documentation pages
```

### API Documentation References
Link to live API docs for detailed method signatures:
```markdown
{% hint style="info" %}
See the [API Docs](https://apidocs.ortussolutions.com/#/coldbox-modules/cborm/) for complete method signatures.
{% endhint %}
```

## Key CBOrm Concepts to Document

### Service Layer Hierarchy
- **BaseORMService**: Core service with all CRUD methods, inject via `entityService`
- **VirtualEntityService**: Auto-created per-entity service via `entityService:EntityName`
- **Concrete Services**: Custom services extending BaseORMService or VirtualEntityService

### ActiveEntity Pattern
- Entities extend `cborm.models.ActiveEntity` for Active Record pattern
- Methods like `.save()`, `.delete()`, `.refresh()` available on entities
- Dynamic finders: `findByUsername()`, `findAllByStatus()`, etc.

### Criteria Queries
- **CriteriaBuilder**: Fluent API for building complex queries with chaining
- **DetachedCriteriaBuilder**: For subqueries and projections
- **Restrictions**: Hibernate criterion wrappers (eq, like, between, etc.)
- **Projections**: Aggregates and custom result sets

### ORM Event Handling
- Lifecycle events: preLoad, postLoad, preInsert, postInsert, preUpdate, postUpdate, preDelete, postDelete
- Custom event handler: `cborm.models.EventHandler`
- Integration with ColdBox interceptors

## Common Documentation Tasks

### Adding New Feature Documentation
1. Create the `.md` file in appropriate section folder
2. **UPDATE SUMMARY.md** - Add entry in correct hierarchical position
3. Add YAML front matter with description
4. Follow standard page structure (see above)
5. Include BoxLang + CFML examples in tabs
6. Add related topic links at bottom

### Updating Existing Pages
1. Check SUMMARY.md for page location and title consistency
2. Maintain existing callout styles and code block formatting
3. Test all code examples for accuracy
4. Update version-specific information (check intro/release-history/)

### Adding Code Examples
1. Always use `{% code title="filename.ext" %}` wrapper
2. Provide both BoxLang and CFML versions when showing application code
3. Use realistic entity names (User, Post, Order, etc.)
4. Show proper WireBox injection patterns
5. Include necessary Application.cfc ORM configuration in getting-started examples

## Version & Platform Support

**Current Major Version**: 4.x
**Supported Platforms**:
- BoxLang 1.0+
- Adobe ColdFusion 2023+
- Lucee 5.x, 6.x

**Dependencies**:
- ColdBox 7.0+
- cbvalidation (for entity validation)
- mementifier (for memento pattern/JSON serialization)
- cbstreams (for streaming query results)
- cbpaginator (for pagination)

## External Resources

- **Source Code**: https://github.com/coldbox-modules/cborm
- **API Docs**: https://apidocs.ortussolutions.com/#/coldbox-modules/cborm/
- **ForgeBox**: https://forgebox.io/view/cborm
- **Issues**: https://ortussolutions.atlassian.net/browse/CBORM
- **Hibernate Docs**: https://docs.jboss.org/hibernate/orm/5.6/userguide/html_single/Hibernate_User_Guide.html

## Writing Style Guidelines

- **Active voice**: "Use this method to..." not "This method can be used to..."
- **User-focused**: Explain the "why" and "when", not just the "what"
- **Consistent terminology**:
  - "entity" not "object" or "ORM entity"
  - "service" not "service layer" or "ORM service"
  - "criteria query" not "criteria builder query"
- **Code-first**: Show working code before explaining theory
- **Progressive disclosure**: Simple → intermediate → advanced

## Common Pitfalls to Avoid

1. ❌ Creating pages without updating SUMMARY.md - navigation breaks
2. ❌ Using plain markdown code blocks instead of GitBook `{% code %}` syntax
3. ❌ Showing only CFML examples - always include BoxLang first
4. ❌ Forgetting `overflow="wrap" lineNumbers="true"` in code blocks
5. ❌ Using absolute image paths - use relative `.gitbook/assets/` paths
6. ❌ Not testing code examples - they must be accurate and working
7. ❌ Mixing up language syntax highlighting (use groovy for BoxLang, javascript for CFML)

## GitBook MCP Server Integration

GitBook provides an MCP (Model Context Protocol) server for enhanced documentation capabilities:
- Documentation search and retrieval
- Content structure understanding
- Best practices for GitBook formatting

Access via: https://gitbook.com/docs/~gitbook/mcp

## FontAwesome Icons

GitBook supports FontAwesome icons in headers via the `icon:` front matter property. Use for visual categorization of pages.
