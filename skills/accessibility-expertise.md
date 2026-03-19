# Accessibility Expertise Skill

## Purpose
This skill provides comprehensive guidance on web and mobile accessibility best practices. **The primary method for accessibility testing and analysis is using the Evinced MCP Tools**, which provide automated accessibility scanning and detailed remediation guidance. For additional context, clarification, and deeper understanding of accessibility principles, refer to the W3C Web Accessibility Initiative (WAI) standards and WCAG guidelines.

## Prerequisites

**IMPORTANT**: Before using this skill, ensure the Evinced MCP server is running:
- Use the **evinced-sdk-setup** skill to check and start the Evinced server if needed
- Verify MCP tools are available by searching for "evinced" tools
- If tools are not available, follow the evinced-sdk-setup workflow to start the server

## When to Use This Skill
- Evaluating websites, applications, or digital content for accessibility compliance
- Designing user interfaces that are inclusive and accessible to people with disabilities
- Developing features with accessibility in mind
- Conducting accessibility audits or reviews
- Testing for accessibility issues using Evinced MCP Tools
- Creating accessible documentation or content
- Understanding WCAG principles and success criteria

## Primary Tool: Evinced MCP

**Start all accessibility audits with Evinced MCP Tools** for efficient, automated analysis:

### 1. Analyze a Page or Component
Use `mcp_evinced-lates_evinced_analyze_page` to scan for accessibility issues:

**Parameters:**
- **tabMode**: `"active_tab"` (analyze current tab) or `"new_tab"` (open and analyze a specific URL)
- **url**: Required when using `"new_tab"` mode - the URL to analyze
- **componentSelector**: 
  - `""` (empty string) - Analyze the entire page
  - CSS selector (e.g., `".modal-dialog"`, `"#header"`) - Focus on a specific component
- **model**: Your AI model identifier (e.g., `"claude-sonnet-4"`)

**Returns**: A report with accessibility issues found, including issue signatures and summaries

**When to Use:**
- At the start of any accessibility review
- After implementing new features or components
- During code review for accessibility compliance
- Before deploying changes to production
- When investigating reported accessibility issues

### 2. Get Detailed Remediation Guidance
Use `mcp_evinced-lates_evinced_get_issue_details` to retrieve comprehensive fix instructions:

**Parameters:**
- **reportId**: The report ID from the analyze_page call
- **issueSignature**: The specific issue signature to investigate
- **model**: Your AI model identifier

**Returns**: Detailed information including:
- **DOM snippet**: Exact code causing the issue
- **Remediation instructions**: Step-by-step guidance on how to fix
- **Bounding box**: Visual location of the issue on the page
- **Additional context**: WCAG criteria violated, severity level

### Recommended Workflow

1. **Initial Scan**: Run `mcp_evinced-lates_evinced_analyze_page` on the page or component you're reviewing
2. **Review Results**: Examine the report for identified accessibility issues
3. **Prioritize Issues**: Focus on critical and high-severity issues first
4. **Get Details**: For each issue, use `mcp_evinced-lates_evinced_get_issue_details` to understand:
   - What code is causing the problem
   - How to fix it
   - Where it's located on the page
5. **Implement Fixes**: Apply the remediation guidance to your code
6. **Verify**: Re-run the analysis to confirm issues are resolved
7. **Manual Testing**: Supplement with keyboard navigation and screen reader testing
8. **Refer to W3C WAI**: If you need clarification on WCAG principles or best practices, consult the W3C resources

### Component-Level Analysis

For complex UI components, use the `componentSelector` parameter to focus your analysis:

```
// Analyze just a modal dialog
componentSelector: ".modal-dialog"

// Analyze a specific form
componentSelector: "#checkout-form"

// Analyze navigation menu
componentSelector: "nav.main-navigation"
```

This targeted approach is especially useful for:
- Modal dialogs and overlays
- Complex forms with multiple steps
- Data tables and grids
- Custom dropdown menus and autocomplete
- Carousels and image galleries
- Interactive charts and visualizations

## Core Accessibility Principles (POUR)

**Reference**: https://www.w3.org/WAI/standards-guidelines/wcag/

For understanding the principles behind accessibility issues identified by Evinced, refer to the four foundational WCAG principles (Perceivable, Operable, Understandable, Robust):

### 1. Perceivable
Information and user interface components must be presentable to users in ways they can perceive.

**Guidelines:**
- **Text Alternatives**: Provide text alternatives for all non-text content (images, icons, charts, etc.)
- **Time-based Media**: Provide captions, audio descriptions, and transcripts for multimedia
- **Adaptable Content**: Create content that can be presented in different ways without losing information or structure
- **Distinguishable**: Make it easier for users to see and hear content (sufficient color contrast, resizable text, etc.)

**Key Considerations:**
- Alternative text for images must be meaningful and contextual
- Captions should be synchronized and accurate
- Color should not be the only means of conveying information
- Text must have sufficient contrast ratio (4.5:1 for normal text, 3:1 for large text)
- Content should be accessible in both visual and auditory formats

### 2. Operable
User interface components and navigation must be operable by all users.

**Guidelines:**
- **Keyboard Accessible**: All functionality must be available via keyboard
- **Enough Time**: Provide users sufficient time to read and use content
- **Seizures and Physical Reactions**: Do not design content that causes seizures or physical reactions
- **Navigable**: Provide ways to help users navigate, find content, and determine where they are
- **Input Modalities**: Make it easier to operate functionality through various inputs beyond keyboard

**Key Considerations:**
- No keyboard traps (users can navigate in and out of all elements)
- Provide skip navigation links for repetitive content
- Use clear, descriptive headings and labels
- Ensure focus indicators are visible
- Allow users to pause, stop, or hide moving content
- Avoid content that flashes more than 3 times per second
- Support touch, voice, and pointer inputs

### 3. Understandable
Information and the operation of the user interface must be understandable.

**Guidelines:**
- **Readable**: Make text content readable and understandable
- **Predictable**: Make web pages appear and operate in predictable ways
- **Input Assistance**: Help users avoid and correct mistakes

**Key Considerations:**
- Use clear, simple language appropriate for the audience
- Identify the language of the page and any language changes
- Navigation should be consistent across pages
- Components that look the same should function the same way
- Provide clear error messages and suggestions for correction
- Offer confirmation for important actions (submissions, deletions, purchases)
- Provide labels and instructions for form inputs

### 4. Robust
Content must be robust enough to be interpreted reliably by a wide variety of user agents, including assistive technologies.

**Guidelines:**
- **Compatible**: Maximize compatibility with current and future user tools and assistive technologies

**Key Considerations:**
- Use valid, semantic HTML markup
- Ensure proper use of ARIA (Accessible Rich Internet Applications) attributes
- Provide name, role, and value for all UI components
- Ensure compatibility with screen readers, magnifiers, and other assistive tools
- Test with multiple browsers and assistive technologies

## WCAG Conformance Levels

**Reference**: https://www.w3.org/WAI/WCAG22/Understanding/conformance#levels

When reviewing Evinced reports, issues are typically mapped to WCAG conformance levels:

### Level A (Minimum)
- Essential accessibility features
- If not met, some users will find it impossible to access content
- **Examples**: Text alternatives, keyboard access, sufficient color contrast

### Level AA (Mid-range, Recommended)
- Addresses major barriers for most users with disabilities
- **Recommended target for most organizations and legal requirements**
- **Examples**: Enhanced contrast (4.5:1), consistent navigation, error identification

### Level AAA (Highest)
- Highest level of accessibility
- Not always achievable for all content types
- **Examples**: Extended audio descriptions, sign language interpretation, highest contrast (7:1)

**Best Practice**: Target **Level AA conformance** as the standard for most digital products. Level AAA should be applied where possible but is not required for full site conformance.

## WCAG Versions

### WCAG 2.2 (Current Standard - October 2023)
- **13 guidelines** organized under 4 principles (POUR)
- **86 success criteria** (Level A, AA, AAA)
- Backwards compatible with WCAG 2.1 and 2.0
- **New in 2.2**: 9 additional success criteria including:
  - Focus appearance
  - Dragging movements
  - Target size (minimum)
  - Consistent help
  - Accessible authentication

### WCAG 2.1 (June 2018)
- Added 17 success criteria focused on mobile accessibility, low vision, and cognitive disabilities
- **New in 2.1**: Orientation, reflow, non-text contrast, text spacing, status messages

### WCAG 2.0 (December 2008)
- Foundation for all subsequent versions
- 12 guidelines
- All success criteria maintained in later versions

**Note**: Content that conforms to WCAG 2.2 automatically conforms to 2.1 and 2.0. Use WCAG 2.2 as the reference standard.

## Common Accessibility Issues and Solutions

**Note**: These issues are typically detected by Evinced MCP Tools. Use this reference to understand the context and WCAG principles behind the issues you encounter in Evinced reports.

### Visual Accessibility
**Issues:**
- Insufficient color contrast
- Missing alt text for images
- Text embedded in images
- Small font sizes that can't be resized

**Solutions:**
- Use contrast checking tools (target 4.5:1 minimum)
- Provide meaningful alternative text for all images (decorative images should have empty alt="")
- Use live text instead of text in images whenever possible
- Ensure text can be resized up to 200% without loss of functionality
- Support both light and dark modes

### Keyboard and Focus Management
**Issues:**
- Elements not keyboard accessible
- Invisible or unclear focus indicators
- Illogical tab order
- Keyboard traps

**Solutions:**
- Ensure all interactive elements are keyboard accessible (Tab, Enter, Space, Arrow keys)
- Provide clear, visible focus indicators (at least 2px outline with sufficient contrast)
- Maintain logical tab order (left-to-right, top-to-bottom)
- Test navigation with Tab, Shift+Tab, and ensure users can always exit elements
- Use `tabindex="0"` for custom interactive elements, avoid positive tabindex values

### Semantic HTML and ARIA
**Issues:**
- Div soup (non-semantic markup)
- Missing or incorrect ARIA labels
- Improper heading hierarchy
- Forms without proper labels

**Solutions:**
- Use semantic HTML elements (`<nav>`, `<main>`, `<article>`, `<button>`, `<header>`, etc.)
- Use ARIA attributes only when semantic HTML is insufficient
- Maintain proper heading hierarchy (h1 → h2 → h3, no skipping levels)
- Associate labels with form inputs using `<label for="id">` or wrapping
- Use `aria-label`, `aria-labelledby`, and `aria-describedby` appropriately
- Mark up roles (`role="navigation"`, `role="banner"`, etc.) when needed

### Forms and Error Handling
**Issues:**
- Missing form labels
- Unclear error messages
- No indication of required fields
- Errors announced only visually

**Solutions:**
- Provide visible labels for all form inputs
- Mark required fields clearly (both visually and programmatically)
- Provide clear, specific error messages (not just "Invalid input")
- Indicate errors both visually and programmatically (aria-invalid, aria-describedby)
- Provide helpful suggestions for correction
- Allow submission confirmation/review for critical actions

### Mobile and Touch Accessibility
**Issues:**
- Touch targets too small
- No alternative to gestures
- Orientation locked
- Content doesn't reflow on small screens

**Solutions:**
- Ensure touch targets are at least 44x44 pixels (or provide adequate spacing)
- Provide alternatives to complex gestures (swipe, pinch, multi-finger)
- Support both portrait and landscape orientations
- Ensure content reflows and remains accessible at 320px width
- Test with screen readers on mobile (VoiceOver on iOS, TalkBack on Android)

### Multimedia Accessibility
**Issues:**
- Videos without captions
- Audio content without transcripts
- Automatic audio playback
- No audio descriptions for visual content

**Solutions:**
- Provide synchronized captions for all video content
- Provide transcripts for audio-only content
- Don't auto-play audio (or provide easy controls to stop it)
- Provide audio descriptions for video content with important visual information
- Include text transcripts as an alternative to multimedia content

### Dynamic Content and Single Page Applications
**Issues:**
- Screen reader not notified of content changes
- Focus lost when content updates
- Loading states not communicated
- Route changes not announced

**Solutions:**
- Use ARIA live regions (`aria-live="polite"` or `"assertive"`) for dynamic updates
- Manage focus appropriately when content changes (move focus to new content or provide skip links)
- Communicate loading/busy states with `aria-busy` or status messages
- Announce route/page changes to screen readers (update page title, move focus)
- Use `role="status"` for status messages and notifications

## Accessibility Testing Strategy

### 1. PRIMARY: Automated Testing with Evinced MCP

**Always start with Evinced MCP Tools** as your first line of accessibility testing:

#### Step 1: Run Evinced Analysis
```
mcp_evinced-lates_evinced_analyze_page
- tabMode: "active_tab" or "new_tab"
- url: [URL if using new_tab]
- componentSelector: "" (full page) or "#component-id" (specific component)
- model: "claude-sonnet-4"
```

#### Step 2: Review Report and Get Details
For each issue in the report, retrieve detailed remediation guidance:
```
mcp_evinced-lates_evinced_get_issue_details
- reportId: [from analyze_page response]
- issueSignature: [specific issue to investigate]
- model: "claude-sonnet-4"
```

#### Step 3: Implement Fixes
Apply the remediation instructions provided by Evinced to your code

#### Step 4: Re-scan
Run the analysis again to verify issues are resolved

**Evinced detects:**
- Missing or incorrect ARIA attributes
- Color contrast violations
- Missing alt text and labels
- Keyboard accessibility issues
- Semantic HTML problems
- Focus management issues
- Form labeling and error handling
- Heading hierarchy violations
- And many more WCAG violations

### 2. SUPPLEMENTARY: Manual Testing

**After running Evinced**, supplement with manual testing to catch issues that automated tools miss (approximately 60-70% of issues):
#### Keyboard Navigation
   - Navigate entire interface using only keyboard
   - Check all interactive elements are reachable and operable
   - Verify visible focus indicators
   - Test modal dialogs and menus for proper focus management

#### Screen Reader Testing
   - Test with at least one screen reader (NVDA, JAWS on Windows; VoiceOver on macOS/iOS; TalkBack on Android)
   - Verify all content is announced correctly
   - Check headings, landmarks, and navigation
   - Test form inputs and error messages
   - Verify dynamic content is announced

#### Visual Inspection
   - Check color contrast with tools
   - Verify content at 200% zoom
   - Test with different color schemes / dark mode
   - Review alternative text for images
   - Check for text in images

#### Content and Structure
   - Verify heading hierarchy (H1→H2→H3)
   - Check semantic HTML usage
   - Review ARIA usage (avoid over-reliance)
   - Validate HTML markup
   - Review reading order and tab order

### 3. Other Automated Testing Tools (Optional)

If Evinced MCP is not available or you want additional validation, you can use:
- **axe DevTools**: Browser extension for automated accessibility testing
- **WAVE**: Web accessibility evaluation tool
- **Lighthouse**: Built into Chrome DevTools, includes accessibility audit
- **pa11y**: Command-line accessibility testing tool

**Note**: Prefer Evinced MCP Tools when available for the most comprehensive automated analysis with detailed remediation guidance.

### 4. Testing with Assistive Technologies
- **Screen Readers**: NVDA (Windows, free), JAWS (Windows, paid), VoiceOver (macOS/iOS, built-in), TalkBack (Android, built-in)
- **Screen Magnifiers**: ZoomText, built-in OS magnification
- **Voice Control**: Dragon NaturallySpeaking, Voice Control on macOS/iOS
- **Specialized Browsers**: High contrast modes, reader modes

## Best Practices Checklist

### Design Phase
- [ ] Consider accessibility from the beginning (not as an afterthought)
- [ ] Design with sufficient color contrast (4.5:1 minimum)
- [ ] Don't rely on color alone to convey information
- [ ] Ensure touch targets are at least 44x44 pixels
- [ ] Design clear focus states for all interactive elements
- [ ] Use simple, clear language
- [ ] Create consistent navigation patterns

### Development Phase
- [ ] Use semantic HTML elements
- [ ] Provide alternative text for images
- [ ] Ensure keyboard accessibility for all interactions
- [ ] Maintain logical heading hierarchy
- [ ] Label all form inputs properly
- [ ] Implement ARIA attributes correctly (when needed)
- [ ] Manage focus for dynamic content
- [ ] Test with keyboard only
- [ ] Test with screen readers
- [ ] Validate HTML markup

### Testing Phase
- [ ] **Run Evinced MCP analysis** (primary automated testing)
- [ ] Review Evinced report and get detailed remediation guidance
- [ ] Implement fixes based on Evinced recommendations
- [ ] Re-run Evinced to verify fixes
- [ ] Conduct manual keyboard testing
- [ ] Test with screen readers (NVDA, JAWS, VoiceOver, or TalkBack)
- [ ] Verify color contrast ratios
- [ ] Test at 200% zoom
- [ ] Test on mobile devices with assistive tech
- [ ] Review with actual users with disabilities (if possible)
- [ ] Document all issues and fixes

### Maintenance Phase
- [ ] Include accessibility in code reviews
- [ ] Test new features for accessibility
- [ ] Monitor for regression issues
- [ ] Stay updated with WCAG guidelines
- [ ] Provide accessibility training for team members
- [ ] Maintain accessibility documentation
- [ ] Collect user feedback on accessibility

## Resources and References

### Primary Tool
- **Evinced MCP**: Use the deferred tools `mcp_evinced-lates_evinced_analyze_page` and `mcp_evinced-lates_evinced_get_issue_details` for all accessibility testing

### Reference Documentation (W3C WAI)

Refer to these resources when you need clarification on WCAG principles, success criteria, or accessibility concepts:
- **WCAG 2.2 Standard**: https://www.w3.org/TR/WCAG22/
- **WCAG Overview**: https://www.w3.org/WAI/standards-guidelines/wcag/
- **How to Meet WCAG (Quick Reference)**: https://www.w3.org/WAI/WCAG22/quickref/
- **Understanding WCAG**: In-depth explanations for each success criterion
- **Techniques for WCAG**: Implementation techniques and examples
- **WAI-ARIA Authoring Practices**: https://www.w3.org/WAI/ARIA/apg/

### Testing Tools
- **axe DevTools**: https://www.deque.com/axe/devtools/
- **WAVE**: https://wave.webaim.org/
- **Color Contrast Analyzer**: https://www.tpgi.com/color-contrast-checker/
- **Lighthouse**: Built into Chrome DevTools
- **Screen Reader Compatibility**: https://www.powermapper.com/tests/screen-readers/

### Additional Learning
- **WebAIM**: https://webaim.org/ - Articles, guides, and training
- **A11y Project**: https://www.a11yproject.com/ - Community-driven accessibility resources
- **Inclusive Components**: https://inclusive-components.design/ - Component patterns

## Common Pitfalls to Avoid

1. **Relying solely on automated testing** - Only catches 30-40% of issues
2. **Adding ARIA when semantic HTML would work** - ARIA should enhance, not replace semantics
3. **Removing focus outlines** - Never remove without providing an alternative visible indicator
4. **Using placeholder as label** - Placeholders disappear and are not proper labels
5. **Auto-playing media** - Causes confusion and accessibility issues
6. **Opening new windows without warning** - Disorients users, especially with screen readers
7. **Using vague link text** - "Click here" and "Read more" lack context
8. **Positive tabindex values** - Creates unpredictable tab order
9. **Hiding content with display:none from everyone** - Use proper techniques to hide visually but keep for screen readers when needed
10. **Forgetting mobile accessibility** - Test with VoiceOver/TalkBack on actual devices

## Conclusion

Accessibility is not a feature or an afterthought—it's a fundamental aspect of quality web development that ensures digital products are usable by everyone, regardless of ability.

**Primary Approach**: Always start with Evinced MCP Tools for efficient, automated accessibility analysis with detailed remediation guidance. Supplement with manual testing (keyboard navigation, screen reader testing) to catch issues that automated tools miss.

**Reference Resources**: Use W3C WAI resources (https://www.w3.org/WAI/) when you need deeper understanding of WCAG principles, success criteria, or accessibility concepts beyond what's provided in Evinced reports.

**Remember**: Making content accessible benefits everyone, not just users with disabilities. Accessible design principles lead to clearer content, better usability, improved SEO, and wider audience reach.