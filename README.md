# Apache Mahout Website Refactoring - GSoC 2025

This repository contains the implementation of the website refactoring project for Apache Mahout, developed as part of Google Summer of Code 2025. The project focuses on restructuring the Apache Mahout website to accurately reflect its pivot from scalable machine learning to quantum computing (Qumat) as its core direction.

## Project Overview

Apache Mahout has evolved significantly, transitioning from its original focus on scalable machine learning to quantum computing. This project addresses the misalignment between the current website and the project's new direction by implementing a comprehensive refactoring of the website architecture, content organization, and user experience.

### Current Website Challenges Addressed

1. **Misaligned Information Architecture**
   - Repositioned Qumat content from deep in navigation to prominence
   - Implemented consistent breadcrumbs throughout the site
   - Reduced navigation depth to follow the 3-click rule

2. **Usability Issues**
   - Enhanced search functionality with Qumat-prioritized results
   - Improved responsive design for all device types

3. **Content Discovery Problems**
   - Reorganized menu structure with Qumat as primary focus
   - Created clear relationships between legacy and quantum implementations
   - Developed clear contributor pathways

4. **Technical Debt**
   - Updated to modern frameworks with proper responsive design
   - Improved SEO performance with optimized metadata and keywords

## Implementation Details

### Technology Stack

```
- Static Site Generator: Jekyll/Hugo
- Frontend: HTML5, CSS3 (Flexbox/Grid), JavaScript (minimal)
- Content: Markdown with structured frontmatter
- Build Tools: npm, Webpack
- Deployment: GitHub Actions
```

### Core Architecture Components

#### Content Organization System

```yaml
# Example frontmatter for Qumat documentation page
---
layout: documentation
title: "Qumat Primer: Introduction to Quantum Computing"
category: qumat
priority: 1
version: current
related:
  - title: "Classical vs Quantum Algorithms"
    url: "/documentation/qumat/classical-vs-quantum"
  - title: "Getting Started with Qumat"
    url: "/documentation/qumat/getting-started"
deprecated: false
---
```

#### Navigation Component Structure

```jsx
// Primary navigation implementation highlighting Qumat
const PrimaryNavigation = () => {
  return (
    <nav className="primary-nav">
      <ul>
        <li className="nav-item featured">
          <a href="/qumat">Quantum Computing (Qumat)</a>
          <div className="submenu">
            <ul>
              <li><a href="/qumat/getting-started">Getting Started</a></li>
              <li><a href="/qumat/examples">Examples</a></li>
              <li><a href="/qumat/api">API Documentation</a></li>
            </ul>
          </div>
        </li>
        <li className="nav-item">
          <a href="/about">About Mahout</a>
        </li>
        <li className="nav-item legacy">
          <a href="/legacy">Legacy Components</a>
          <div className="submenu">
            <ul>
              <li><a href="/legacy/mapreduce">MapReduce</a></li>
              <li><a href="/legacy/samsara">Samsara</a></li>
            </ul>
          </div>
        </li>
        <li className="nav-item">
          <a href="/community">Community</a>
        </li>
      </ul>
    </nav>
  );
};
```

#### Deprecation Warning System

```jsx
// Reusable deprecation warning component
const DeprecationNotice = ({ component, alternativePath }) => {
  return (
    <div className="deprecation-notice">
      <div className="warning-icon">⚠️</div>
      <div className="warning-content">
        <h4>Legacy Component</h4>
        <p>
          This documentation covers {component}, which is no longer under active development.
          {alternativePath && (
            <span> Consider exploring <a href={alternativePath}>Qumat implementations</a> for new projects.</span>
          )}
        </p>
      </div>
    </div>
  );
};
```

#### Responsive Design Implementation

```css
/* Core responsive layout system */
.container {
  width: 100%;
  padding-right: 1.5rem;
  padding-left: 1.5rem;
  margin-right: auto;
  margin-left: auto;
}

/* Mobile-first approach with progressive enhancement */
@media (min-width: 640px) {
  .container {
    max-width: 640px;
  }
}

@media (min-width: 768px) {
  .container {
    max-width: 768px;
  }
}

@media (min-width: 1024px) {
  .container {
    max-width: 1024px;
  }
}

@media (min-width: 1280px) {
  .container {
    max-width: 1280px;
  }
}

/* Responsive navigation */
.primary-nav {
  width: 100%;
}

.primary-nav ul {
  display: flex;
  flex-direction: column;
}

@media (min-width: 768px) {
  .primary-nav ul {
    flex-direction: row;
  }
}

/* Featured content highlighting */
.featured-card {
  border-left: 4px solid #3366cc;
  padding: 1.5rem;
  background-color: rgba(51, 102, 204, 0.05);
  margin-bottom: 2rem;
}

/* Legacy content styling */
.legacy-content {
  border-left: 4px solid #f9a825;
  opacity: 0.9;
}
```

#### Search Enhancement System

```javascript
// Enhanced search functionality prioritizing Qumat content
const searchIndex = {
  buildIndex: function(content) {
    return content.map(item => ({
      ...item,
      // Apply boosting to Qumat content
      weight: item.category === 'qumat' ? 2.0 : 1.0
    }));
  },
  
  search: function(query, index) {
    const results = index.filter(item => 
      item.title.toLowerCase().includes(query.toLowerCase()) || 
      item.content.toLowerCase().includes(query.toLowerCase())
    );
    
    // Sort by weight (Qumat first) then by relevance
    return results.sort((a, b) => {
      // First compare by category weight
      if (a.weight !== b.weight) {
        return b.weight - a.weight;
      }
      
      // Then by title match
      const aTitle = a.title.toLowerCase().includes(query.toLowerCase());
      const bTitle = b.title.toLowerCase().includes(query.toLowerCase());
      if (aTitle !== bTitle) {
        return aTitle ? -1 : 1;
      }
      
      // Finally by content relevance
      return 0;
    });
  }
};
```

## Key Deliverables

### 1. Website Architecture and Design

- **Site Map and Information Architecture Document**
  - Complete visualization of new site structure
  - Content organization plan with rationale
  - User journey maps for different visitor personas

- **Design System Documentation**
  - Component specifications and usage guidelines
  - Typography and color systems
  - Responsive behavior definitions

### 2. Implemented Website

- **Modernized Homepage and Landing Pages**
  - Qumat-focused content prominently featured
  - Clear access points for different user needs
  - Proper information hierarchy guiding users to current focus

- **Responsive Navigation System**
  - Primary/secondary navigation structure
  - Mobile-optimized navigation components
  - Breadcrumb and contextual navigation elements

- **Documentation Framework**
  - Structured organization of Qumat documentation
  - Legacy documentation with deprecation notices
  - Consistent page templates for different content types

### 3. Content Migration and Enhancement

- **Updated Quantum Computing Content**
  - Enhanced Qumat primer with improved navigation
  - Restructured tutorials and learning resources
  - Code examples with proper formatting

- **Legacy Documentation System**
  - MapReduce and Samsara documentation with clear deprecation indicators
  - Context explanations for historical components
  - Migration guides for users transitioning to newer approaches

### 4. Technical Implementation

- **Responsive, Accessible Frontend**
  - WCAG 2.1 AA-compliant implementation
  - Performance-optimized assets and code
  - Cross-browser compatible components

- **Search and Discovery Features**
  - Content-aware search implementation
  - Related content suggestion system
  - Enhanced metadata for discoverability

### 5. Documentation and Handover

- **Maintenance Documentation**
  - Content update procedures and guidelines
  - Technical implementation details
  - Build and deployment processes

- **Future Enhancement Roadmap**
  - Prioritized recommendations for continued improvements
  - Identified areas for community involvement
  - Technical debt assessment and remediation plan

## Project Structure

```
apache-mahout-website/
├── src/
│   ├── components/
│   │   ├── Navigation/
│   │   ├── DeprecationNotice/
│   │   ├── CodeBlock/
│   │   └── ...
│   ├── layouts/
│   │   ├── Default.js
│   │   ├── Documentation.js
│   │   └── ...
│   ├── pages/
│   │   ├── index.js
│   │   ├── qumat/
│   │   ├── legacy/
│   │   └── ...
│   └── styles/
│       ├── main.css
│       └── ...
├── content/
│   ├── qumat/
│   ├── legacy/
│   └── ...
├── static/
│   ├── images/
│   └── ...
├── config.yml
├── package.json
└── README.md
```

## Timeline

This project was developed according to the following timeline during GSoC 2025:

- **Community Bonding Period** (May 8 - June 1, 2025)
- **Phase 1: Planning & Initial Development** (June 2 - June 30, 2025)
- **Phase 2: Implementation** (July 1 - August 10, 2025)
- **Phase 3: Refinement & Final Submission** (August 11-25, 2025)




