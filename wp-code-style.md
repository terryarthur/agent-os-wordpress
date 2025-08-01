---
title: WordPress Coding & Development Standards for agent‑os
description: |
  This document defines coding standards and best‑practice conventions for building WordPress plugins and block themes using the agent‑os framework.  It consolidates the official WordPress coding standards for PHP, CSS, HTML, JavaScript and Markdown; security and accessibility guidelines; and development conventions for plugins and full site editing (FSE) themes.  These standards allow agent‑os to automatically enforce consistent code formatting, ensure secure and accessible implementations, and provide a solid developer experience across all WordPress projects.
version: 0.1.0

---

## Overview

These standards mirror the WordPress project’s own guidelines while adapting them to the agent‑os ecosystem.  They are intended to be consumed by agent‑os instructions and can be dropped into any WordPress‑based project during setup.  Adhering to these rules improves readability, minimizes bugs, and enhances user experience.

Each section describes conventions for a specific language or topic.  Where appropriate, examples illustrate correct usage.  Citations at the end of this file reference the source documentation from WordPress for further reading.

### General Principles

- **Follow official WordPress coding standards.**  WordPress has published detailed rules for each language.  When in doubt, defer to those standards and use the nearest equivalent in agent‑os.
- **Prefix everything.**  To avoid naming collisions, prefix all globally accessible PHP functions, classes, constants and variables with a unique namespace or at least a four‑letter prefix【98023375798376†L91-L131】.  Do not use core prefixes like `wp_`, `__`, `WordPress` or `_`.
- **Secure all inputs and outputs.**  Never trust data from users or external sources.  Always sanitize incoming data (e.g., `sanitize_text_field`, `sanitize_key`) and escape output using the appropriate function (`esc_html`, `esc_attr`, `esc_url`)【98023375798376†L247-L281】.  Validate capabilities with `current_user_can` and protect forms with nonces (`wp_create_nonce`, `wp_verify_nonce`).
- **Design for accessibility.**  Ensure that interfaces meet WCAG 2.2 Level AA requirements—provide text alternatives, ensure contrast ratios of at least 4.5:1, support keyboard navigation, and label form controls【181474316998242†L139-L151】【688395595616639†L74-L90】.
- **Optimize for user experience.**  Use descriptive link text, underline links, use different colours for visited links, ensure readable font sizes, and use descriptive button labels【688395595616639†L74-L90】.
- **Document your code.**  Include meaningful comments and docblocks.  For PHP and JS functions/classes/hooks, provide a summary, parameter documentation, return values and a `@since` annotation【198828885871868†L83-L146】.
- **Use modern WordPress features.**  For FSE themes, configure `theme.json` to define global settings and styles; create block templates, patterns and parts; and use block registration APIs for custom blocks.

## PHP Coding Standards

### Syntax & Formatting

- **Opening tags.**  Always use full PHP tags `<?php … ?>` and avoid shorthand tags.  Do not omit the closing tag in templates.
- **Indentation.**  Use a single tab for each level of indentation.  Tabs keep code aligned with WordPress core formatting【415195069547595†L71-L149】.
- **Quotes.**  Use single quotes for strings unless the string contains variables or requires escape sequences【866848821094246†L129-L189】.
- **Spacing.**  Place one space after keywords (`if`, `foreach`, etc.) and around operators (`=`, `+`).  Do not put spaces inside parentheses or after type casts.
- **Yoda conditions.**  When comparing variables to literal values, put the literal on the left (`if ( 10 === $count )`) to avoid accidental assignment【866848821094246†L129-L189】.
- **Braces.**  Use Allman style: place the opening brace on its own line for classes, functions and control structures.  Align closing braces vertically.
- **Includes.**  Do not wrap `include`, `require`, `include_once` or `require_once` with parentheses【866848821094246†L129-L189】.
- **Namespaces and prefixes.**  Use namespaces or prefixes to avoid naming collisions.  Always check for existing functions or classes before defining your own (`if ( ! function_exists( 'myplugin_function' ) ) { … }`)【98023375798376†L91-L131】.

### Security & Validation

- **Sanitize all inputs.**  Use built‑in sanitization functions (e.g., `sanitize_text_field`, `sanitize_email`, `sanitize_key`, `sanitize_textarea_field`) to clean user input.
- **Validate capabilities.**  Use `current_user_can()` to ensure the user has permission to perform an action.
- **Protect forms.**  Use nonces with `wp_nonce_field()` and verify them with `check_admin_referer()` or `wp_verify_nonce()`.
- **Escape all outputs.**  Use `esc_html()` for plain text, `esc_attr()` for attribute values, `esc_url()` for URLs, `esc_js()` for inline JavaScript and `wp_kses_post()` for post content.
- **Database queries.**  Use `$wpdb->prepare()` with placeholders to prevent SQL injection.  Prefer the WordPress CRUD functions (`get_posts`, `wp_insert_post`, etc.) over direct SQL when possible.

### Documentation

- **Docblocks.**  Precede every function, class, hook, and file with a docblock.  Write a one‑sentence summary in third‑person singular, followed by `@since` with a version, and `@param` and `@return` annotations【198828885871868†L83-L146】.
- **Inline comments.**  Use `//` for single‑line comments and `/* … */` for multi‑line comments.  Explain why code exists, not what it does when the intent is clear.

## CSS Coding Standards

### Structure & Selectors

- **Indentation.**  Use tabs for indentation.  Nested selectors should be indented one tab deeper than their parent【239523798467839†L95-L160】.
- **Selector naming.**  Use hyphenated class names (kebab‑case) and avoid underscores or camelCase【239523798467839†L95-L160】.  Use descriptive names tied to the content or component.
- **Ordering.**  Group properties logically: layout properties (`position`, `display`, `float`, `width`, etc.), box model (`margin`, `border`, `padding`), typography (`font`, `line-height`, `color`) and visual properties (background, shadows, transitions)【239523798467839†L169-L228】.
- **Vendor prefixes.**  Do not write vendor prefixes manually.  Use Autoprefixer as part of your build process to generate them automatically【239523798467839†L320-L417】.

### Values & Formatting

- **Units.**  Omit units for zero values (e.g., `margin: 0;`).  Use lowercase letters for hex colors and shorthand where possible (`#fff` instead of `#FFFFFF`)【239523798467839†L95-L160】.
- **Quoting.**  Use double quotes around attribute selectors and strings.  Use single quotes only when necessary to avoid escape characters.
- **Media queries.**  Place media queries after the rules they modify.  Indent the content of the media query by one tab.  Order breakpoint rules from narrow to wide screens【239523798467839†L169-L228】.

### Comments

- Start multi‑line comments with `/*` and align asterisks on following lines.  Provide meaningful descriptions of sections or complex logic.  Use comments to demarcate major sections of your stylesheet【239523798467839†L320-L417】.

## HTML Coding Standards

### Syntax & Formatting

- **Validation.**  All HTML must validate against the W3C specification.  Use HTML5 syntax and avoid deprecated elements and attributes【415195069547595†L71-L149】.
- **Self‑closing tags.**  Self‑closing elements (e.g., `<br>`, `<hr>`, `<img>`) must have a space before the slash: `<img src="..." alt="..." />`【415195069547595†L71-L149】.
- **Case & quoting.**  Tag names and attribute names must be lowercase.  Always quote attribute values and omit the value for boolean attributes (use `disabled` instead of `disabled="disabled"`)【415195069547595†L71-L149】.
- **Indentation.**  Use tabs to indent nested elements.  When attributes wrap across multiple lines, align them under the first attribute and indent subsequent attributes by one tab【517833167828616†L2-L33】.
- **Structure.**  Use semantic elements (`<header>`, `<nav>`, `<article>`, `<section>`, `<aside>`, `<footer>`).  Avoid excessive nested divs (the “div‑itis” problem).

### Accessibility

- **Alt text.**  Provide meaningful `alt` text for images.  Use empty alt attributes (`alt=""`) for purely decorative images so assistive technologies ignore them.
- **Labels & forms.**  Associate `label` elements with their corresponding `input` using the `for` attribute and matching `id`【688395595616639†L150-L167】.  Do not rely on placeholder text as the only label.
- **Landmarks & headings.**  Use headings (`h1`–`h6`) in order without skipping levels.  Use ARIA landmarks (`role="navigation"`, `role="main"`) to provide additional semantics where appropriate.

## JavaScript Coding Standards

### Syntax & Formatting

- **Indentation.**  Use tabs for indentation.  Indent case statements inside switch by one tab【136600875207074†L145-L171】.
- **Quotes.**  Use single quotes for strings except when the string itself contains a single quote【136600875207074†L145-L171】.
- **Semicolons.**  Always terminate statements with semicolons.  Avoid relying on automatic semicolon insertion【136600875207074†L145-L171】.
- **Braces.**  Always use braces for `if`, `else`, `for`, `while`, and `do…while`.  Place the opening brace on the same line as the statement and the closing brace on a new line【136600875207074†L145-L171】.
- **Whitespace.**  Surround binary operators with spaces and do not insert spaces after function names in calls (e.g., `myFunction()` not `myFunction ()`).
- **Objects & arrays.**  For objects with many properties, place one property per line, aligning the colons.  For arrays, place a space after each comma and avoid trailing commas.【136600875207074†L145-L171】

### WordPress Specific

- Use the `wp` global packages (`wp.element`, `wp.components`, `wp.data`, etc.) instead of external dependencies when building block editors.
- For internationalization, wrap strings in `wp.i18n.__()` or `wp.i18n._x()`.  Provide text domain via the PHP file to match the plugin or theme.
- Use `wp.enqueueScript` and `wp.enqueueStyle` in PHP to register and enqueue your scripts/styles; specify dependencies so that `wp‑scripts` can handle bundling.

### Documentation

- Document functions with JSDoc‑style comments.  Begin with a description, list all parameters using `@param`, and document the return value using `@returns`.  Include version information with `@since` to indicate when the function was added.

## Markdown Style Guide

- Use ATX‑style headings (`#`, `##`, `###`) to denote structure.  Do not skip levels; `#` should be reserved for the document title【725997230265909†L80-L86】.
- For emphasis, use asterisks (`*bold*`, `_italic_`) and tildes for strike‑through (`~~strikethrough~~`).
- Create unordered lists with hyphens (`-`) and ordered lists with numbers followed by a period (`1.`).  Use proper indentation for nested lists【725997230265909†L109-L127】.
- Block quotes should begin with a greater‑than symbol (`>`).  For multi‑paragraph quotes, repeat the symbol for each paragraph.
- Use fenced code blocks (triple backticks) and specify the language when appropriate (e.g., ```php, ```js).  Avoid mixing HTML and Markdown in the same block unless necessary【725997230265909†L156-L186】.
- When including tables, keep them narrow.  Place only short phrases or keywords in table cells to avoid line wrapping.  Long explanations belong in the body text.

## Accessibility Standards

- **WCAG compliance.**  Code must meet WCAG 2.2 Level AA for perceivability, operability, understandability and robustness【181474316998242†L72-L90】.
- **Contrast & colour.**  Maintain a contrast ratio of at least 4.5:1 between text and its background【181474316998242†L139-L151】.  Do not convey meaning purely through colour.
- **Keyboard navigation.**  Ensure all interactive elements are reachable and usable via keyboard.  Indicate focus state clearly and do not trap focus.
- **ARIA & semantics.**  Use semantic HTML and ARIA roles/attributes only when necessary to enhance semantics.  Do not misuse roles or create redundant labels.
- **Media alternatives.**  Provide captions and transcripts for time‑based media.  Provide text alternatives for non‑text content, including icons and emojis.

## Plugin Development Guidelines

### File Structure & Headers

- **Main file & header.**  Every plugin must have a main PHP file with a plugin header comment containing at least `Plugin Name`.  Other optional headers include `Description`, `Version`, `Requires at least`, `Requires PHP`, `Author`, `Text Domain`, etc.【501405134542863†L84-L115】.
- **Prefix everything.**  Prefix all functions, classes, hooks and constants with a unique identifier.  Alternatively, wrap code in a namespace.  Check for existing names with `function_exists`, `class_exists` or `defined`【98023375798376†L91-L131】.
- **File organisation.**  Keep the main file and `uninstall.php` in the root; place additional PHP files in `includes/` or similar folders.  Separate admin‑only code using `is_admin()`【98023375798376†L247-L281】.
- **Protect direct access.**  In every PHP file, immediately exit if WordPress is not loaded:

  ```php
  if ( ! defined( 'ABSPATH' ) ) {
      exit; // Silence is golden.
  }
  ```【98023375798376†L247-L281】

### Hooks & Enqueueing

- Use actions and filters to hook your functionality into WordPress.  Do not modify core files.
- Enqueue scripts and styles using `wp_enqueue_script()` and `wp_enqueue_style()`.  Specify dependencies, version numbers and whether the script should be loaded in the footer.
- Localize scripts with `wp_localize_script()` to pass data from PHP to JavaScript.

### Security & Internationalisation

- Sanitize and escape all data entering or leaving WordPress.  Always validate user capabilities and verify nonces before processing form submissions.
- Make your plugin translation‑ready.  Load your text domain using `load_plugin_textdomain()` and wrap all user‑facing strings in internationalization functions (e.g., `__( 'My string', 'my-text-domain' )`).

### Activation & Uninstall

- Use `register_activation_hook()` to handle installation tasks (e.g., setting options).  Use `register_deactivation_hook()` for clean‑up tasks.  For permanent clean‑up tasks (deleting options), implement an `uninstall.php` file or use `register_uninstall_hook()`.

## Theme Development Guidelines (Full Site Editing)

### Required Files & Structure

- **style.css.**  A block theme must include a `style.css` file with a header comment that contains at least `Theme Name`.  This file can be empty if all styling is handled in `theme.json`【39832120860305†L87-L115】.
- **templates/index.html.**  Every block theme requires an `index.html` template inside the `templates` directory.  This is the fallback template used when no other template is available【39832120860305†L87-L115】.
- **theme.json.**  Define global settings (colors, typography, spacing) and styles in `theme.json`.  This file allows users to modify design elements via the Site Editor without custom CSS.
- **functions.php.**  Use this file to register block patterns, scripts, styles or additional theme support features.  It is optional for purely block‑based themes but recommended for adding hooks or filters.
- **screenshot.png.**  Include a screenshot of your theme to display in the Themes admin screen.  The recommended size is 1200×900 pixels.
- **Standard folders.**  Use `/templates` for templates, `/parts` for template parts, `/patterns` for block patterns and `/styles` for theme variations【39832120860305†L146-L163】.

### Full Site Editing & Blocks

- **Block templates.**  Create HTML files within `/templates` that consist of blocks.  Use comments at the top to specify a description (e.g., `<!-- wp:template-part {"slug":"header"} /-->`).
- **Template parts.**  Store reusable pieces like headers, footers and sidebars within `/parts`.  Reference them in templates via the `template-part` block.
- **Block patterns.**  Provide curated patterns in `/patterns`.  Register patterns in `functions.php` and specify categories and keywords for the pattern explorer.
- **theme.json.**  Define presets for colors, gradients, font sizes and spacing.  Leverage settings to enable or disable core features (e.g., custom units).

### Best Practices

- Follow the same security, accessibility and internationalization guidelines as plugins.  Escape dynamic content in templates using PHP functions (`esc_html`, `esc_attr`) or by using the appropriate block attributes.
- Keep custom CSS to a minimum.  Use `theme.json` whenever possible to define design system values instead of manual CSS overrides.
- Use `wp_enqueue_block_style()` to add styles that are associated with specific blocks instead of globally enqueuing CSS.  This ensures styles load only when needed.
- Provide a `readme.txt` file in your theme root following the WordPress Theme Directory guidelines.  Document the theme’s purpose, installation steps, license and changelog.

## Security Checklist

1. Check the user’s capability (`current_user_can()`) before performing privileged actions.
2. Sanitize all incoming data with the appropriate sanitization function.
3. Validate and escape all outgoing data before rendering it on the page.
4. Verify nonces on every form submission or state‑changing request.
5. Use prepared statements or high‑level WordPress APIs instead of constructing SQL queries manually.
6. Avoid exposing sensitive information (such as file paths or debug messages) in errors or responses.

## UI/UX Checklist

1. Link your site’s logo to the homepage and include alt text【688395595616639†L74-L90】.
2. Use descriptive anchor text rather than generic phrases like “click here”【688395595616639†L74-L90】.
3. Underline hyperlinks and use a different colour for visited links【688395595616639†L74-L90】.
4. Maintain a colour contrast ratio of at least 4.5:1 and use a minimum font size of 14 px【688395595616639†L150-L167】.
5. Pair every form input with a visible label and avoid using placeholders as labels【688395595616639†L150-L167】.
6. Label buttons using the pattern “Verb + Noun” (e.g., **Create Post**)【688395595616639†L150-L167】.

## Citations

The guidelines above draw from the official WordPress coding standards and development handbooks.  Key sources include the WordPress PHP coding standards【866848821094246†L129-L189】【198828885871868†L83-L146】, CSS standards【239523798467839†L95-L160】【239523798467839†L169-L228】【239523798467839†L320-L417】, HTML standards【415195069547595†L71-L149】, JavaScript standards【136600875207074†L145-L171】, Markdown style guide【725997230265909†L80-L86】【725997230265909†L156-L186】, accessibility guidelines【181474316998242†L72-L90】【181474316998242†L139-L151】, UI best practices【688395595616639†L74-L90】【688395595616639†L150-L167】, plugin best practices【98023375798376†L91-L131】【98023375798376†L247-L281】【501405134542863†L84-L115】, and theme development guidance【39832120860305†L87-L115】【39832120860305†L146-L163】.