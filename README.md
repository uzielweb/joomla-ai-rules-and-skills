# Joomla AI Rules & Standards

Este repositório contém as regras globais e padrões de desenvolvimento de alta fidelidade para **Joomla 5 e 6**, focados na integração perfeita com assistentes de Inteligência Artificial modernos.

A comunidade de desenvolvimento Joomla e IA (Antigravity, Cursor, Github Copilot) precisa de diretrizes estritas para evitar que os modelos sugiram código legado (ex: prefixos `J`, classes antigas do Joomla 3, código inseguro) e arquiteturas defasadas.

Aqui você encontra a configuração exata para transformar sua IA em um desenvolvedor sênior de Joomla.

## 🚀 Assistentes Suportados

Temos configurações separadas e otimizadas para as principais IAs do mercado. Escolha a sua e siga as instruções no README de cada pasta:

- 🧠 **[Antigravity IDE](./antigravity/README.md)**: Skills avançadas (Ipsis Litteris Standards) e regras globais de comportamento nativas do ecossistema Gemini.
- 🖱️ **[Cursor AI](./cursor/README.md)**: Regras `.mdc` formatadas para o ambiente de contexto do Cursor.
- ✈️ **[GitHub Copilot](./copilot/README.md)**: Instruções unificadas `.github/copilot-instructions.md` para integração via VS Code/Codespaces.

## 🌟 O que estas regras ensinam à IA?
- Fim absoluto de prefixos `J` legados (forçando o uso de namespaces modernos como `Joomla\CMS\Language\Text`).
- Imposição de arquitetura MVC nativa (Models, Views, Dispatchers, Extension Classes).
- Gerenciamento limpo de assets usando o **Web Asset Manager** moderno.
- Testes, segurança (sem mock data, uso de prepared statements) e conventional commits.

Contribuições são bem-vindas! Sinta-se livre para abrir Issues ou Pull Requests com novas diretrizes de otimização de IA para Joomla.
