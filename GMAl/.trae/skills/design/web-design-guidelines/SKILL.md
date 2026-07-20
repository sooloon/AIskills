---
name: web-design-guidelines
description: >
  Web Interface Guidelines for designing high-quality, consistent, and user-friendly web interfaces. Covers layout, colors, typography, components, interactions, and accessibility.
  Use when designing or reviewing UI components, layouts, colors, typography, and interactions, or when user asks for web design best practices.
  Do NOT use when user needs platform-specific design guidelines (iOS HIG, Material Design 3), or branding/logo design.
allowed-tools: Read, Write
---

# Web Interface Guidelines

## Core Principles

### 1. Visual Hierarchy
- Use size, weight, color, and spacing to establish clear hierarchy
- Primary actions should be most prominent
- Secondary actions should be visually subordinate
- Maintain consistent spacing scale (4px, 8px, 12px, 16px, 24px, 32px, 48px)

### 2. Color System
- **Primary Color**: Use for primary actions, active states, key highlights
- **Secondary Colors**: Use for supporting elements, backgrounds
- **Semantic Colors**: 
  - Success: Green tones for positive states
  - Warning: Yellow/Orange tones for caution
  - Error: Red tones for errors and critical actions
  - Info: Blue tones for neutral information
- **Neutral Colors**: Grays for text, borders, backgrounds
- Maintain WCAG 2.1 AA contrast ratios (minimum 4.5:1 for text)

### 3. Typography
- Use system fonts or clean sans-serif fonts
- Establish clear type scale:
  - Display: 32-48px (page titles)
  - Heading 1: 24-28px (section titles)
  - Heading 2: 18-20px (subsections)
  - Body: 14-16px (main content)
  - Small: 12px (captions, metadata)
- Line height: 1.5 for body text, 1.2-1.3 for headings
- Maximum line length: 75 characters for readability

### 4. Layout & Spacing
- Use grid systems (12-column recommended)
- Consistent gutters (16-24px)
- Adequate whitespace (breathing room)
- Responsive breakpoints:
  - Mobile: < 768px
  - Tablet: 768px - 1024px
  - Desktop: > 1024px

### 5. Components

#### Buttons
- Clear visual affordance (looks clickable)
- Consistent padding (horizontal: 16-24px, vertical: 8-12px)
- Distinct states: default, hover, active, disabled
- Primary: filled, high contrast
- Secondary: outlined or ghost style

#### Cards
- Subtle shadows or borders for definition
- Consistent padding (16-24px)
- Clear content hierarchy within
- Rounded corners (4-8px) for modern feel

#### Forms
- Clear labels above or beside inputs
- Input height: 32-40px
- Adequate spacing between fields (16-24px)
- Inline validation with clear error messages
- Placeholder text should be helpful, not redundant

#### Navigation
- Clear current state indication
- Consistent placement
- Mobile: hamburger menu or bottom nav
- Desktop: horizontal top nav or sidebar

### 6. Interactions & Feedback
- **Hover States**: Indicate interactivity (cursor, color change, subtle lift)
- **Active States**: Show when element is being interacted with
- **Focus States**: Visible focus rings for accessibility (keyboard navigation)
- **Loading States**: Skeleton screens or spinners
- **Transitions**: Subtle, 150-300ms duration

### 7. Accessibility
- Keyboard navigation support
- ARIA labels for screen readers
- Sufficient color contrast
- Focus indicators
- Alt text for images
- Semantic HTML structure

### 8. Data Visualization
- Clear labels and legends
- Consistent color coding
- Appropriate chart types for data
- Tooltips for detailed information
- Responsive sizing

### 9. Icons
- Use consistent icon set
- Clear meaning (avoid ambiguity)
- Adequate size (16-24px typical)
- Proper spacing from text
- Alternative text for accessibility

### 10. Empty States
- Never leave blank spaces
- Explain why content is missing
- Provide clear next steps or CTAs
- Use illustrations where appropriate

## Design Patterns

### Dashboard Design
- Key metrics at top (KPI cards)
- Visual hierarchy for data
- Charts and graphs for trends
- Recent activity or alerts
- Quick actions accessible

### List/Table Views
- Clear column headers
- Alternating row colors (subtle)
- Sortable columns indicated
- Pagination or infinite scroll
- Search and filter options

### Detail Views
- Clear back navigation
- Key information prominent
- Related content accessible
- Action buttons clearly placed

## When to use

- Designing or reviewing web UI components
- Establishing visual design systems
- Checking UI consistency and accessibility
- Keywords: web design, UI guidelines, component design, layout review, color system, typography

## When NOT to use

- Platform-specific guidelines needed (iOS HIG, Material Design 3)
- Branding or logo design
- Print design requirements

## Anti-Patterns to Avoid
- Cluttered layouts with no whitespace
- Inconsistent spacing or alignment
- Too many colors or font styles
- Tiny click targets (< 44px)
- Ambiguous button labels
- Missing error states
- Breaking browser back button
- Popups without clear close actions

## Error Handling

- Design system has no defined component → reference core principles and extrapolate consistently
- Contrast ratio unclear → recommend WCAG AA compliance check tools
- Responsive breakpoint conflict → prioritize mobile-first approach

## ⚠️ Gotchas

- **不要为了好看牺牲可访问性** — 低对比度文字看起来很"高级"但违反 WCAG 标准
- **点触目标不要小于 44px** — 移动端小于 44px 的点触目标会导致用户操作困难
- **不要混用多种图标库** — 统一使用一个图标集，保持视觉一致性
- **空状态必须有内容** — 永远不要让页面出现空白区域，必须有说明和 CTA
- **不要忽略 focus 状态** — 键盘导航用户依赖 focus 样式，去掉 outline 必须提供替代方案
