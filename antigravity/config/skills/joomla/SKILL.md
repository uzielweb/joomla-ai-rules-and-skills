---
name: Joomla Development (Ipsis Litteris)
description: High-fidelity toolset for modern Joomla 5/6 architecture, derived directly from core components.
version: 3.0.0
---

# Joomla Development - Ipsis Litteris Standards

This skill provides a high-fidelity toolset for scaffolding and developing Joomla 5 and 6 projects. It enforces the exact architectural patterns found in core components like `com_banners` and `com_content`.

## 0. General Rules & Standards (MANDATORY)

- **No Hardcoded Strings**: Never hardcode text strings in XML, PHP, or JavaScript. Always use language constants (e.g., `MOD_MY_LABEL`) and define them in `.ini` and `.sys.ini` files.
- **Branching & Gitignore for Upgrades (MANDATORY)**: Before performing any major Joomla upgrade (such as upgrading to Joomla 6), you MUST create a new git branch. In this branch, you must immediately download the correct `.gitignore` (for Joomla 6, download it from `https://raw.githubusercontent.com/uzielweb/my-joomla-gitignore/main/joomla6.gitignore`).
- **Multilingual by Default & Prefix Standard (MANDATORY)**: Every extension MUST include at least **Portuguese (pt-BR)** and **English (en-GB)** language files. In both frontend and administrator space, language files placed in the main `/language/` directories must ALWAYS have the language code prefix (e.g., `pt-BR.com_icode.ini` or `en-GB.com_icode.ini`). Failing to add the prefix prevents standard language loaders from loading translations automatically.
- **Parametric Translations via `sprintf`**: Do not concatenate variables inside hardcoded HTML elements (e.g., `<strong>ID:</strong> <?php echo $this->item->id; ?>`). Define a translation key containing formatters like `%s` or `%d` (e.g., `COM_ICODE_TESTES_ID_FORMAT="<strong>ID:</strong> %s"`) and display it using `sprintf(Text::_('CONSTANT'), $value)` to maintain perfectly localized and cleanly formatted structures.
- **Strictly No Emojis - Always FontAwesome (MANDATORY)**: Never use standard emojis (💻, 🔍, 🛡️, 🤝, ✔, ✖, ⌛) for interface badges, icons, or headers. Always use equivalent FontAwesome icons (e.g., `fa-laptop-code`, `fa-search`, `fa-shield-alt`, `fa-handshake`, `fa-check`, `fa-times`, `fa-hourglass-half`) to keep a cohesive, high-quality, professional corporate aesthetic.
- **Styles Must Be Kept in Media (MANDATORY)**: Styles should never be embedded inline or inside template views. They must always reside inside specialized assets located in `/media/` (e.g., `/media/com_icode/css/frontend.css`) and loaded using Joomla's Web Asset Manager (`$wa->useStyle()`).
- **Portuguese Git Commits (MANDATORY)**: Always write and format Git commit messages in **Portuguese (pt-BR)** unless explicitly requested otherwise. This ensures clean, uniform, and localized history tracking across the repository.
- **Global Storage Patterns**: 
  - **Languages**: Store language files in the common Joomla folders (`/language/` with `pt-BR/` and `en-GB/` subfolders).
  - **Media**: All assets (CSS, JS, Images, Vendors) must be placed in the common `/media/` directory (e.g., `/media/mod_my_module/`).
  - **Template Assets**: When specific to a template, use `/media/templates/site/template_name/`.
- **Zero Scratch Files (MANDATORY)**: Never create temporary scripts (`scratch/*.php`) or "maintenance" files on the project root.
- **Joomla 5/6 Modern Module Standard (Boilerplate `mod_hello`)**:
  - **No Entry File**: Do NOT create a `mod_name.php` in the root. The entry point is handled by the `ServiceProvider`.
  - **Service Provider (`services/provider.php`)**: Anonymous class implementing `ServiceProviderInterface` registering the `ModuleDispatcherFactory` and `HelperFactory`.

    ```php
    return new class () implements ServiceProviderInterface {
        public function register(Container $container) {
            $container->registerServiceProvider(new ModuleDispatcherFactory('\\Joomla\\Module\\Hello'));
            $container->registerServiceProvider(new HelperFactory('\\Joomla\\Module\\Hello\\Site\\Helper'));
            $container->registerServiceProvider(new Module());
        }
    };
    ```

  - **Dispatcher (`src/Dispatcher/Dispatcher.php`)**: Extends `AbstractModuleDispatcher` and implements `HelperFactoryAwareInterface` (using `HelperFactoryAwareTrait`). Loads data in `getLayoutData()` via the helper factory:

    ```php
    $data['list'] = $this->getHelperFactory()->getHelper('HelloHelper')->getItems($data['params'], $this->getApplication());
    ```

  - **Helper Class (`src/Helper/HelloHelper.php`)**: Implements `DatabaseAwareInterface` and uses `DatabaseAwareTrait` for database access via `$this->getDatabase()`.
  - **Extension Class**: An Extension class (`src/Extension/MyModule.php`) is **not** mandatory for basic modules, as routing/dispatching is resolved by the Dispatcher.
  - **Subforms**: Use `type="subform"` for all repeatable/nested data (lists, grids, partners).
  - **Web Asset Manager (WAM) Options**:
    - *Programmatic (Simple)*: Registered directly in `tmpl/default.php` via `$wa->registerAndUseStyle()` and `$wa->registerAndUseScript()` without needing `joomla.asset.json`.
    - *Declarative (Complex)*: Using standard `media/joomla.asset.json` configuration.
  - **Manifest XML (`mod_name.xml`)**: Namespace mapping defined in `<namespace path="src">Joomla\Module\Hello</namespace>` and services folder mapped as `<folder module="mod_hello">services</folder>`.

## 0.1 CLI Extension Management (Joomla 4/5/6)

Joomla provides built-in CLI commands to manage extensions without using the Web UI. These are located in `cli/joomla.php`.

- **Scan for new files**: `php cli/joomla.php extension:discover`
  - Scans directories like `/modules`, `/plugins`, and `/components` for manually uploaded extension files.
- **List discovered items**: `php cli/joomla.php extension:discover:list`
  - Displays a table of all extensions found during the scan that are not yet installed.
- **Install discovered items**: `php cli/joomla.php extension:discover:install`
  - Performs the installation process for all discovered extensions (executes XML manifest, creates database entries).
- **List all commands**: `php cli/joomla.php list`
  - Shows all available CLI commands (cache, configuration, site, etc.).

> [!TIP]
> Use the full path to Laragon's PHP if the system default is not configured with necessary extensions (like `mysqli`).
> Example: `D:\laragon\bin\php\php-8.5.7-Win32-vs17-x64\php.exe cli/joomla.php extension:discover`

## 1. Global Refence Sources (Golden Sources)

### 1.1 Gitignore

Source: [uzielweb/my-joomla-gitignore](https://github.com/uzielweb/my-joomla-gitignore)

- **Joomla 6**: [Raw Link](https://raw.githubusercontent.com/uzielweb/my-joomla-gitignore/main/joomla6.gitignore)
- **Joomla 5**: [Raw Link](https://raw.githubusercontent.com/uzielweb/my-joomla-gitignore/main/joomla5.gitignore)
- **Joomla 4**: [Raw Link](https://raw.githubusercontent.com/uzielweb/my-joomla-gitignore/main/joomla4.gitignore)
- **Joomla 3**: [Raw Link](https://raw.githubusercontent.com/uzielweb/my-joomla-gitignore/main/joomla3.gitignore)

### 1.2 Default Template (Minimalista)

Source: [uzielweb/minimalista](https://github.com/uzielweb/minimalista/)

### 1.3 Portuguese (Brazil) Language Pack

Source: [uzielweb/joomla6-pt-br](https://github.com/uzielweb/joomla6-pt-br)
Official package for Joomla 6 localization.

### 1.4 Documentação Oficial (Joomla)

- **Developer Manual**: [Joomla! Programmers Documentation](https://manual.joomla.org/)
- **API Reference**: [Joomla! API](https://api.joomla.org)
- **Framework Documentation**: [Joomla! Framework](https://framework.joomla.org)

---

## 2. "Ipsis Litteris" Scaffolding Toolset

The scaffolding engine generates code 100% compliant with modern Joomla core standards.

### 2.1 Core Scaffolding Command

Generate a complete component structure (Admin + Site + Media).

```bash
# Usage: python scripts/scaffold_component.py <PluralName> <Vendor>
python scripts/scaffold_component.py Portfolio MyAgency
```

### 2.2 Architecture Generated

- **Service Provider**: Anonymous class implementation with Dependency Injection.
- **Extension Class**: Bootable, implementing `CategoryServiceInterface` and `RouterServiceInterface`.
- **Router Service**: Rule-based SEF routing (`RouterViewConfiguration`).
- **Models**: Standards-compliant `ListModel` and `AdminModel`.
- **Tables**: Robust data integrity with `check()`, `bind()`, and `store()` overrides.
- **WAM**: Automated `joomla.asset.json` registration.

---

## 3. Engineering & UI Standards

- **Namespaces**: Always use PSR-4 compliant namespaces (`Vendor\Component\Name\Administrator`).
- **Web Asset Manager**: Assets registered in `joomla.asset.json` are prioritised over manual loading.
- **Overrides**: All UI should follow the structured `tmpl` pattern with `HTMLHelper` calls.
- **Print System**: Native support for high-quality PDF extraction using the fixed-header hybrid pattern.

### 3.1 Repeatable Subforms UX Standards: Dynamic Sub-tabs & Collapsible Accordion

When designing complex repeatable subforms (e.g. Test Cases, Items, Partners) containing heavy fields like rich text WYSIWYG editors (TinyMCE), adhere to these strict engineering standards:

1. **Dynamic Editor Sub-tabs (Iframe Safe)**:
   - **Do NOT repackage or move repeatable fields in the DOM**. Moving active WYSIWYG editors triggers an iframe reload in the browser, destroying event listeners and freezing the editor.
   - **Solution**: Keep elements in their natural DOM tree. Build horizontal tab headers dynamically using pure Javascript and toggle active/inactive states using CSS classes (`display: none` vs `display: block`).

2. **Zero-Hardcoded Translations (Dynamic Label Extraction)**:
   - Never hardcode text strings or interface labels in external JavaScript files.
   - **Solution**: Extract translated field strings dynamically from the rendered `.control-label label` elements in the DOM, cleaning up trailing colons (`:`) or red asterisks (`*`) programmatically. This guarantees flawless compatibility with Joomla's multilingual `.ini` system.

3. **Collapsible Cards Accordion with Smart Load UX**:
   - To prevent long scrolling, implement an accordion collapse button (`.btn-collapse-card`) inside each subform row's `.btn-toolbar`.
   - Use high-definition, inline-drawn outline SVG vector paths (such as chevron polyline paths `M18 15 12 9 6 15` with `stroke-width="3"`) that scale perfectly and rotate `180deg` via CSS during state changes.
   - **Smart Load UX rule**:
     - **Existing rows** (containing saved data) must load **collapsed by default** on page load, styled with a subtle contrasted background (using theme-specific background CSS variables) and an elegant vertical left border highlight (using theme-accented custom properties to support light and dark modes natively), presenting a clean and scannable summary list.
     - **Newly added rows** (when the user clicks "Add new row") must remain **expanded by default**, allowing immediate input without clicking.

4. **Pragmatic Asset Architecture**:
   - Do NOT embed inline `<script>` tags inside PHP template files.
   - Register the javascript asset in `/media/com_component/js/frontend.js` under the `joomla.asset.json` manifest with a `defer: true` attribute. Load it standardly using Joomla's Web Asset Manager in the template head: `$wa->useScript('com_component.frontend')`.

---

## 5. Fetching Articles in Template Overrides

When building template overrides for `com_content/categories` (list of categories view), the Joomla view **does NOT load articles automatically**. The `$this->items` array contains only category/subcategory objects, never article objects.

### 5.1 Correct Pattern: MVC Factory with Params

Use the `ArticlesModel` via `bootComponent()`. **Critical**: you MUST set the `params` state, otherwise the model internally calls `$this->getState('params')->get()` on null and throws a fatal error.

```php
$app = Factory::getApplication();

$articlesModel = $app->bootComponent('com_content')
    ->getMVCFactory()
    ->createModel('Articles', 'Site', ['ignore_request' => true]);

// CRITICAL: Set params to prevent 'Call to a member function get() on null'
$articlesModel->setState('params', $app->getParams());

$articlesModel->setState('filter.category_id', $categoryId);
$articlesModel->setState('filter.published', 1);
$articlesModel->setState('list.limit', 10);
$articlesModel->setState('list.ordering', 'a.publish_up');
$articlesModel->setState('list.direction', 'DESC');

$articles = $articlesModel->getItems();
```

### 5.2 What NOT to do

- **Do NOT use raw `$db->getQuery()` SQL** in template overrides. It bypasses ACL, language filters, and workflow states.
- **Do NOT assume `$category->items` exists**. In the categories view, category objects have no `items` property.
- **Do NOT forget `setState('params', ...)`**. This is the #1 cause of `get() on null` errors when using ArticlesModel outside its native context.

### 5.3 Available Article Properties

The model returns full article objects with: `id`, `title`, `alias`, `introtext`, `fulltext`, `catid`, `images`, `urls`, `publish_up`, `language`, `state`, etc.

---

**Note**: All template files and scripts are located in `skills/joomla/`.
To update the blueprints, edit the files in `resources/templates/component/`.
