---
name: Joomla Modern Standards
description: High-fidelity toolset for modern Joomla 5/6 architecture, derived directly from core components.
version: 3.0.0
---

# Joomla Modern Standards

This skill provides a high-fidelity toolset for scaffolding and developing Joomla 5 and 6 projects. It enforces the exact architectural patterns found in core components like `com_banners` and `com_content`.

## 0. General Rules & Standards (MANDATORY)

- **No Hardcoded Strings**: Never hardcode text strings in XML, PHP, or JavaScript. Always use language constants (e.g., `MOD_MY_LABEL`) and define them in `.ini` and `.sys.ini` files.
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
- **Joomla 6 Modern Module Standard**:
  - **No Entry File**: Do NOT create a `mod_name.php` in the root. The entry point is handled by the `ServiceProvider`.
  - **Service Provider**: Mandatory `services/provider.php` registering the `ModuleDispatcherFactory`.
  - **Dispatcher**: Business logic MUST reside in `src/Dispatcher/Dispatcher.php` using `getLayoutData()`.
  - **Assets**: Prefira utilizar `$wa->registerAndUseStyle()` e `$wa->registerAndUseScript()` no PHP do módulo como primeira opção. O uso de `joomla.asset.json` é perfeitamente válido, mas deve ser tratado como alternativa secundária.
