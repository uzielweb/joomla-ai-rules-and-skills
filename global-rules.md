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
