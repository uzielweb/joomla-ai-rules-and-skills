---
name: Joomla Modern Standards
description: High-fidelity toolset for modern Joomla 5/6 architecture, derived directly from core components.
version: 3.0.0
---

# Joomla Modern Standards

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
- **Joomla 6 Modern Module Standard**:
  - **No Entry File**: Do NOT create a `mod_name.php` in the root. The entry point is handled by the `ServiceProvider`.
  - **Service Provider**: Mandatory `services/provider.php` registering the `ModuleDispatcherFactory`.
  - **Dispatcher**: Business logic MUST reside in `src/Dispatcher/Dispatcher.php` using `getLayoutData()`.
  - **Assets**: Prefira utilizar `$wa->registerAndUseStyle()` e `$wa->registerAndUseScript()` no PHP do módulo como primeira opção. O uso de `joomla.asset.json` é perfeitamente válido, mas deve ser tratado como alternativa secundária.

## Template Web Assets (WAM) Configuration & Standards

For template-specific assets (CSS, JS) in Joomla 4/5/6, always register them declaratively via a `joomla.asset.json` file in the template's root directory:

1. **Definition Registry File**:
   - Create/modify `/templates/<template_name>/joomla.asset.json`.
   - Ensure the JSON `name` property matches the template's folder name.
   - Use the `"template."` prefix for all asset names (e.g. `"template.print-helper"`, `"template.custom"`).

2. **Asset Directory Resolution**:
   - The files referenced in the `joomla.asset.json` URI properties must be stored in the common media folder under `/media/templates/site/<template_name>/`.
   - Script (`"type": "script"`) files go in `/media/templates/site/<template_name>/js/`.
   - Style (`"type": "style"`) files go in `/media/templates/site/<template_name>/css/`.

3. **PHP Loading in Views / Overrides**:
   - Always retrieve the Web Asset Manager from the active document:
     `$wa = $this->document->getWebAssetManager();`
   - Use the asset prefix: `$wa->useScript('template.print-helper');` or `$wa->useStyle('template.print');`. This decouples layouts and views from hardcoded URLs and component-bound assets.

## Joomla Security Hardening & Vulnerability Mitigation

Recent exploits in widely used extensions require strict defensive layers. Below are the key security alerts and standard hardening practices.

### Vulnerability Alerts (June 2026)

1. **JCE (Joomla Content Editor) RCE (CVE-2026-48907 - CVSS 10.0)**:
   - **Vulnerability**: Unauthenticated Remote Code Execution allowing attackers to inject arbitrary editor profiles, enabling upload and execution of malicious PHP scripts (web shells).
   - **Mitigation**: Update immediately to **JCE 2.9.99.6** (or newer). Inspect server access logs for requests targeting `index.php?option=com_jce&task=profiles.import`. Review editor profiles and active accounts for unauthorized changes.
2. **SP Page Builder Critical RCE**:
   - **Vulnerability**: Unauthenticated file upload zero-day via the `asset.uploadCustomIcon` task, allowing execution of PHP scripts and creation of rogue "Super Administrator" accounts.
   - **Mitigation**: Update immediately to **SP Page Builder 6.6.2** (or newer). Audit Joomla users for unexpected Super User profiles (often using mock domains like `@secure.local`).

### Server Hardening (`.htaccess` Best Practices)

Applying security directives in `.htaccess` mitigates exploit attempts even if an extension is unpatched:

1. **Block Direct PHP Execution in Sensitive Paths**:
   Prevent execution of uploaded PHP scripts in the images, media, and templates directories:
   ```apache
   # Block PHP execution in upload folders
   <DirectoryMatch "^.*/(images|media|tmp|cache)/.*$">
       <FilesMatch "\.(php|php[3-8]|phtml)$">
           Order Deny,Allow
           Deny from all
       </FilesMatch>
   </DirectoryMatch>
   ```
2. **Disable Directory Browsing**:
   ```apache
   Options -Indexes
   ```
3. **Protect configuration.php and XML files**:
   ```apache
   <FilesMatch "(configuration\.php|joomla\.xml|web\.config|\.ini|\.env)$">
       Order Deny,Allow
       Deny from all
   </FilesMatch>
   ```
4. **Force HTTPS and Secure Headers**:
   ```apache
   <IfModule mod_headers.c>
       Header always set X-Content-Type-Options "nosniff"
       Header always set X-Frame-Options "SAMEORIGIN"
       Header always set Referrer-Policy "strict-origin-when-cross-origin"
   </IfModule>
   ```

### Security References & Vulnerability Databases

Use the following official links to monitor security alerts, verify vulnerable extensions, and report CVEs:

- **Joomla Security Centre**: [https://developer.joomla.org/security-centre.html](https://developer.joomla.org/security-centre.html)
- **Joomla Vulnerable Extensions List (VEL)**: [https://vel.joomla.org/](https://vel.joomla.org/)
- **Joomla CVE Details Database**: [https://www.cvedetails.com/vulnerability-list/vendor_id-3496/Joomla.html](https://www.cvedetails.com/vulnerability-list/vendor_id-3496/Joomla.html)
- **Joomla! Issue Tracker**: [https://issues.joomla.org/](https://issues.joomla.org/)
- **Joomla! CMS GitHub Issues**: [https://github.com/joomla/joomla-cms/issues](https://github.com/joomla/joomla-cms/issues)

# Antigravity - Regras Globais

Este arquivo define as diretrizes obrigatórias para o comportamento do assistente de IA.

## 🛠️ Desenvolvimento & Engenharia

- **Execução PHP**: Não crie arquivos PHP temporários para testar snippets. Execute o código diretamente no terminal via `php -r "..."` para manter o workspace limpo.

## 💾 Versionamento (Git)

- **Idioma dos Commits**: Todas as mensagens de commit devem ser escritas obrigatoriamente em **português**.

## 🌐 Redes e Ferramentas

- **Preferência de Ferramentas**: Priorize o uso de `wget` e `curl` para downloads e inspeção de URLs. Use o navegador interno apenas se for estritamente necessário para interação visual ou JS complexo.

## ⌨️ Padrões de Código (PHP & JS)

- **Boas Práticas de JS**: Priorize ES6+ e Vanilla JS, evitando criar dependências extras caso o Bootstrap ou Joomla já ofereçam a solução nativamente.

## 🗣️ Comunicação

- **Seja Direto**: Vá direto ao ponto. Não preciso de longas explicações sobre como o código funciona, a não ser que eu peça. Priorize me entregar o código funcional ou o comando a ser executado.
- **Idioma**: Mantenha toda a nossa comunicação e a documentação interna do código 100% em PT-BR.

## 🗄️ Banco de Dados (MySQL)

- **Consultas Destrutivas**: Sempre peça a minha confirmação explícita antes de executar comandos `DROP`, `DELETE` ou `TRUNCATE` no banco de dados.
- **Testes com DB**: Prefira usar `SELECT` com `LIMIT` durante testes de query no terminal, para evitar sobrecarga ou travamentos.

## 🎨 Front-End e Estilização

- **Padrão Bootstrap**: Sempre que criar ou editar interfaces (HTML/UI), utilize as classes utilitárias nativas do Bootstrap 5. Evite criar CSS customizado se já houver uma classe do Bootstrap que resolva o problema. Evite verborragia e excesso de classes, preferindo sempre deixar classes herdáveis que possam ser personalizadas no css, a não ser em páginas estritamente em html.
- **Outros Frameworks CSS**: Evite verborragia e excesso de classes ao lidar com outros frameworks CSS. Mantenha o markup o mais limpo possível e entregue o código de forma direta e eficiente, sem explicações longas sobre cada classe utilizada.

## 💡 Padrões e Arquitetura
- **Comentários no Código**: Sempre que escrever comentários ou documentar métodos diretamente dentro do código-fonte, utilize **inglês** como idioma padrão. (A comunicação do chat e mensagens de commit permanecem em português, conforme regra anterior).
- **Princípio DRY (Don't Repeat Yourself)**: Sempre busque desenvolver de forma modular e focada no reaproveitamento. Evite duplicação de código criando funções, classes ou componentes reutilizáveis em vez de copiar e colar blocos lógicos.

## 📉 Economia de Tokens e Respostas
- **Zero Cerimônia**: Nunca inicie ou termine mensagens com saudações, confirmações ("Claro!", "Entendido", "Aqui está o código") ou despedidas.
- **Apenas a Solução**: Forneça imediatamente o código, o comando ou a resposta seca. Se a resposta for apenas um comando de terminal, retorne *apenas* o comando e nada mais.
- **Sem Explicações Proativas**: Nunca explique o passo a passo de como o código funciona, a menos que eu pergunte explicitamente "Como isso funciona?" ou "Explique".
- **Código Parcial (Snippets)**: Quando me mostrar código no chat, mostre **apenas** a função ou o bloco exato que foi alterado. Nunca imprima o arquivo inteiro no chat a menos que seja um arquivo novo. Use comentários como `// ... código existente ...` para omitir o resto.
- **Tratamento de Logs/Erros**: Quando eu colar um log de erro, não repita o erro na sua resposta. Apenas diga onde está o problema e forneça a correção.
- **Sem Resumos**: Após executar uma tarefa ou plano, não crie um parágrafo enorme resumindo o que você já fez. Apenas confirme a conclusão de forma breve.

## 🚫 Arquivos Temporários
- **Zero Scratch Files (MANDATORY)**: Nunca crie scripts temporários (ex: scratch/*.php, arquivos .py ou .md temporários), arquivos de manutenção ou qualquer outro tipo de arquivo temporário na raiz do projeto ou no workspace. Não deixe rastros no disco.

## 🔗 Links e Arquivos
- **Links Obrigatórios**: Sempre que modificar ou criar um arquivo, exiba obrigatoriamente no chat o link clicável em formato markdown (ex: [nome_do_arquivo](file:///caminho/absoluto)) para fácil acesso.

## ⚙️ Qualidade e Padrões Globais
- **Zero Mock (Sem TODOs)**: Nunca gere funções incompletas ou com placeholders // TODO. Entregue a implementação 100% funcional.
- **Sem Comentários Óbvios**: Não comente o que é óbvio no código. Priorize código autoexplicativo com nomes claros.
- **Segurança Paranoica**: Trate sempre todas as entradas, use queries parametrizadas e escape a saída HTML (evitar XSS/SQLi).
- **Commits Semânticos**: Use o padrão Conventional Commits (eat:, ix:, chore:, etc).
- **Strict Linting**: Siga estritamente PSR-12 (PHP) e boas práticas modernas de JS.
- **Testes por Padrão**: Ao criar regras de negócio complexas, inclua ou sugira testes.
