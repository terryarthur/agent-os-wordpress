---
title: WordPress Plugin & Theme Development
description: |
  This instruction guides the agent through the process of building or modifying WordPress plugins and block themes using the agent‑os framework.  It ensures the agent collects all necessary context, applies WordPress coding standards, and implements best practices for security, accessibility and user experience.  The instruction is designed to be reused across multiple projects and should be included in the base installation of agent‑os.
globs:
  - "**/*.php"
  - "**/*.js"
  - "**/*.css"
  - "**/*.html"
  - "**/*.md"
  - "**/readme.txt"
  - "**/style.css"
  - "**/theme.json"
  - "**/templates/*.html"
version: 0.1.0
---

<pre_flight_check>

1. Verify that the project targets WordPress and specify whether you are building a **plugin** or a **block theme**.  If unclear, ask the user.
2. Confirm that you have access to the WordPress installation or the project repository.  Determine the root directory where new files should be created.
3. Ensure that the agent has read the **WordPress Coding & Development Standards for agent‑os** (`wp-code-style.md`) so that all code adheres to the defined guidelines.
4. Check that all required information (plugin/theme name, slug, prefix, version, description, target WordPress version, features, etc.) is available.  If not, ask for it in the context questions.

</pre_flight_check>

<process_flow>

  <step name="Determine project type and gather details">
    <instructions>
    - Ask the user whether the project is a **plugin** or **block theme**.  This determines the subsequent steps.  If the user already supplied this information, continue.
    - Collect essential metadata:
      - **Plugin**: plugin name, unique prefix or namespace, plugin slug (folder name), version, description, minimum WordPress version, minimum PHP version, dependencies, and intended features.
      - **Theme**: theme name, slug, text domain, version, description, whether it is full site editing (FSE), and any planned templates, template parts, block patterns, style variations or palette definitions.
    - Determine the target languages (PHP, CSS, HTML, JavaScript) and whether internationalization is required.
    </instructions>
    <context_questions>
      - Is this project a **plugin** or a **block theme**?  If both, please describe each item separately.
      - What is the **name** and **slug** of the plugin/theme?  The slug will be used for folder names and text domains.
      - Provide a **unique prefix or namespace** to prevent naming collisions in PHP functions and classes.
      - What version numbers (plugin/theme version, minimum WordPress version, minimum PHP version) should be set?
      - What are the core **features** or functionalities you intend to implement?  (e.g., custom post type, block registration, REST API integration, custom templates, etc.)
      - Do you require **internationalization** (translation support)?
    </context_questions>
  </step>

  <step name="Establish project structure">
    <instructions>
    - For a **plugin**:
        1. Create a root folder with the plugin slug.  Inside, create the main PHP file (e.g., `my-plugin.php`) containing a header comment with fields like `Plugin Name`, `Description`, `Version`, `Requires at least`, `Requires PHP`, `Author`, `Text Domain`, etc.【501405134542863†L84-L115】.
        2. Add an `uninstall.php` file if the plugin needs to clean up database entries on deletion.
        3. Create subdirectories for `includes/` (core functionality), `admin/` (admin UI), `public/` (front‑end UI) and `assets/` (CSS, JS, images).  Only include code for the relevant context; do not over‑engineer.
        4. Each PHP file should begin with a check for `defined( 'ABSPATH' )` to prevent direct access【98023375798376†L247-L281】.
        5. Use actions and filters to integrate functionality rather than modifying core files.
    - For a **block theme**:
        1. Create a folder with the theme slug.  Add a `style.css` containing the theme header (Theme Name, Author, Version, Text Domain, etc.)【39832120860305†L87-L115】.
        2. Create `/templates/index.html` as the default block template.  Additional templates (e.g., `single.html`, `archive.html`) should reside in the `templates` folder.
        3. Provide `theme.json` to define global styles, palettes, typography, spacing and block support settings.
        4. Add folders `/parts` for template parts (header, footer, sidebar) and `/patterns` for reusable block patterns【39832120860305†L146-L163】.
        5. Include an optional `functions.php` file for registering patterns, styles and other PHP logic.
        6. Include `screenshot.png` (1200×900) to represent the theme in the WordPress admin.
    - Throughout, adhere to the file and folder naming conventions specified in the **WordPress Coding & Development Standards for agent‑os**.
    </instructions>
  </step>

  <step name="Implement coding standards & best practices">
    <instructions>
    - Apply the coding rules defined in `wp-code-style.md` to all created files:
        - Use proper indentation (tabs) and spacing for PHP, CSS, HTML and JS.
        - Use single quotes for strings where possible, and Yoda conditions in PHP.【866848821094246†L129-L189】
        - Organize CSS properties logically and use Autoprefixer rather than manual vendor prefixes【239523798467839†L169-L228】【239523798467839†L320-L417】.
        - Write semantic HTML, quote attribute values, and avoid `true="false"` boolean attributes【415195069547595†L71-L149】.
        - Use semicolons and braces consistently in JavaScript【136600875207074†L145-L171】.
    - Prefix or namespace all PHP functions, classes and constants to prevent collisions【98023375798376†L91-L131】.  Check for existing functions using `function_exists()` or `class_exists()`.
    - Ensure every function, class and hook is documented with a docblock.  Include a summary, `@since` tag, and `@param`/`@return` information【198828885871868†L83-L146】.
    - When outputting data to the browser, always escape using the appropriate `esc_*` function.  When receiving data, always sanitize and validate before processing【98023375798376†L247-L281】.
    </instructions>
  </step>

  <step name="Security, accessibility and user experience checks">
    <instructions>
    - **Security**: Verify that all user inputs are sanitized using the correct sanitization functions (e.g., `sanitize_text_field`, `sanitize_email`).  Use nonces on forms and verify them with `wp_verify_nonce()`.  Ensure capability checks (`current_user_can()`) precede privileged actions.  Always use prepared statements or high‑level APIs for database queries.
    - **Accessibility**: Ensure that every image has an appropriate `alt` attribute.  Use semantic HTML elements and ARIA roles only when necessary.  Provide sufficient color contrast (ratio ≥ 4.5:1) and support keyboard navigation【181474316998242†L139-L151】.  Provide labels for all form fields and ensure that navigation structures are perceivable by screen readers【688395595616639†L150-L167】.
    - **User Experience**: Apply UI best practices—link the logo to the homepage, underline hyperlinks, distinguish visited links, maintain adequate font sizes, and label buttons with verb+noun phrases【688395595616639†L74-L90】.  Avoid using only placeholder text as labels and ensure interactive elements are large enough to tap on touch devices.
    </instructions>
  </step>

  <step name="Internationalization & localization (optional)">
    <instructions>
    - If internationalization is requested, load the text domain in PHP using `load_plugin_textdomain()` for plugins or `load_theme_textdomain()` for themes.  Ensure that your plugin or theme slug matches the text domain.
    - Wrap all user‑visible strings in translation functions: `__( 'String', 'text-domain' )`, `_e()`, `_x()`, or `wp.i18n.__()` in JavaScript.
    - When defining block metadata (e.g., in `block.json`), include `title`, `description` and `keywords` fields that can be translated.
    </instructions>
  </step>

  <step name="Finalize and review">
    <instructions>
    - Review the generated files to ensure they meet the expectations set by the WordPress Coding & Development Standards and that the file structure is correct.
    - Validate PHP syntax using `php -l` or similar tools, and run your code through a linter if available (e.g., `phpcs` with WordPress ruleset).
    - Confirm that your plugin or theme activates without errors in a WordPress environment and that templates render correctly in the Site Editor.
    - Provide a summary of the created files and key implementation details to the user.
    </instructions>
  </step>

</process_flow>

<execution_parameters>
  max_depth: 5
  # Do not exceed five levels of nested tasks.  Each step should produce tangible output before moving to the next.
</execution_parameters>