# Changelog

All notable changes to this project are documented in this file.

Format: [Keep a Changelog](https://keepachangelog.com/)
Versioning: [Semantic Versioning](https://semver.org/)

## [Unreleased]

### Added
- Upcoming features

## [2.0.0] - 2025-12-30

### Added
- **html-expert Agent**: Production-grade upgrade
  - Role & responsibility boundaries
  - Input/Output type-safe schemas
  - Error handling with retry strategy (exponential backoff)
  - Fallback behaviors for graceful degradation
  - Token/cost optimization configs
  - Comprehensive troubleshooting section

- **html-basics Skill**: Complete rewrite
  - All HTML5 element categories (document, text, grouping, embedded, interactive, metadata)
  - Nesting rules and validation
  - Templates for common patterns
  - Error codes with auto-fix capabilities

- **semantic-html Skill**: Production-grade upgrade
  - ARIA landmark role mapping
  - Document outline algorithm
  - Content type templates (article, navigation, breadcrumb, sidebar, figure)
  - Anti-pattern detection (div soup, class-as-role, heading abuse)
  - Semantic score calculation

- **accessibility Skill**: WCAG 2.2 compliant
  - Full WCAG 2.2 success criteria (Level A, AA, AAA)
  - ARIA roles, states, and properties reference
  - Keyboard navigation patterns
  - Screen reader compatibility matrix
  - 15 error codes with WCAG mapping
  - Testing tools integration

- **forms Skill**: Constraint Validation API
  - All HTML5 input types with keyboard and validation info
  - Complete autocomplete values reference
  - Common regex validation patterns
  - Form templates (contact, login, registration, search)
  - ValidityState properties
  - JavaScript validation examples

### Changed
- Updated all skills to v2.0.0
- Enhanced all config.yaml files with comprehensive element/pattern references
- Improved all GUIDE.md files with decision trees and troubleshooting sections

### Technical
- Based on 2024-2025 industry best practices
- References: MDN, W3C, WCAG 2.2, ARIA APG
- All skills follow atomic, single-responsibility design
- Comprehensive error codes with recovery procedures

## [1.0.0] - 2025-12-29

### Added
- Initial release
- SASMP v1.3.0 compliance
- Golden Format skills
- Protective LICENSE

---

**Maintained by:** Dr. Umit Kacar & Muhsin Elcicek
