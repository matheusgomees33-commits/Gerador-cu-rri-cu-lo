# Requisitos do protótipo: Gerador de Currículo · Tech-ZL

Lista do que o protótipo **possui hoje**, levantada a partir do código.
Coluna **Teste**: ✅ coberto por teste automatizado · ➖ implementado, sem teste automatizado.

## 1. Módulo Candidato (`index.html`)

### 1.1 Requisitos funcionais

| Código | Descrição | Onde | Teste |
|---|---|---|---|
| RF01 | O formulário é dividido em quatro abas: Pessoal, Experiência, Educação e Extras. | `index.html`, `ativarAba` | ✅ |
| RF02 | O sistema permite informar dados pessoais: nome, cargo/objetivo, e-mail, telefone, cidade, LinkedIn, GitHub, Instagram e resumo profissional. | `index.html` | ✅ |
| RF03 | O sistema permite adicionar, editar e remover várias experiências profissionais (empresa, cargo, período e descrição). | `adicionarExperiencia`, `atualizarExp`, `removerExperiencia` | ✅ |
| RF04 | O sistema permite adicionar, editar e remover várias formações acadêmicas (curso, instituição e período). | `adicionarEducacao`, `atualizarEdu`, `removerEducacao` | ✅ |
| RF05 | O sistema permite informar cursos complementares (um por linha), habilidades e idiomas (separados por vírgula) e carta de apresentação. | aba Educação/Extras | ✅ |
| RF06 | O sistema exibe o currículo em pré-visualização que se atualiza a cada alteração. | `renderizarCV` | ✅ |
| RF07 | O preview mostra apenas as seções preenchidas; contatos viram links (e-mail, telefone, redes). | `gerarHTMLCV` | ➖ |
| RF08 | O usuário navega entre as abas por *Anterior/Próximo* ou clicando na aba; os botões ficam desabilitados nas extremidades. | `ACOES`, `ativarAba` | ✅ |
| RF09 | O sistema mostra uma barra de progresso de preenchimento (nome, cargo, e-mail, resumo, experiência, formação e habilidades). | `atualizarProgresso` | ✅ |
| RF10 | O sistema salva automaticamente todos os dados do currículo após qualquer alteração. | `agendarSalvamento`, `salvarAgora` | ✅ |
| RF11 | O sistema recupera automaticamente os dados salvos ao abrir ou atualizar a página no mesmo navegador. | `carregarSalvo` | ✅ |
| RF12 | O sistema informa o estado do salvamento (*Salvando…*, *Salvo às HH:MM* ou mensagem de erro). | `mostrarStatus` | ✅ |
| RF13 | O sistema apaga todos os dados do formulário e os dados salvos por meio de "Limpar tudo", mediante confirmação. | `limparFormulario` | ✅ |
| RF14 | O sistema exporta o currículo em HTML; exige o nome preenchido e avisa caso contrário. | `exportarCV` | ➖ |
| RF15 | O sistema exporta o currículo em PDF; avisa caso a biblioteca ainda não tenha carregado. | `exportarPDF` | ➖ |
| RF16 | As ações de exportação ficam disponíveis na barra de navegação, no menu móvel e junto ao formulário. | `index.html` | ➖ |
| RF17 | O sistema oferece link de acesso ao módulo Empresa. | `index.html` | ➖ |
| RF18 | O formulário inicia vazio, para que cada pessoa preencha com as próprias informações. | `carregarSalvo` | ✅ |

### 1.2 Requisitos não funcionais

| Código | Descrição | Teste |
|---|---|---|
| RNF01 | O salvamento automático ocorre em até 1 s após o usuário parar de digitar (debounce de 500 ms). | ✅ |
| RNF02 | O armazenamento usa o `localStorage` do navegador, em uma única chave. | ✅ |
| RNF03 | A restauração dos dados não prejudica a pré-visualização em tempo real. | ✅ |
| RNF04 | Falhas de armazenamento (bloqueado, cheio) e dados corrompidos são tratados com mensagem ao usuário, sem quebrar a página. | ✅ |
| RNF05 | Os dados ficam somente no navegador/dispositivo do usuário, sem envio automático a terceiros. | ✅ |
| RNF06 | **Segurança:** todo texto digitado ou restaurado é escapado no preview e no HTML exportado; IDs vindos do armazenamento são regenerados. | ✅ |
| RNF07 | A última edição não se perde se a página for fechada antes do salvamento (`pagehide`/`visibilitychange`). | ✅ |
| RNF08 | **Tecnologia:** HTML5, CSS3 e JavaScript puro, sem framework nem etapa de build; site estático. | ➖ |
| RNF09 | **Responsividade:** layout adaptado a telas de até 1100, 900, 620 e 400 px, com menu móvel. | ➖ |
| RNF10 | **Acessibilidade:** foco visível no teclado, `aria-selected` nas abas, `role="progressbar"` e `role="status"`, e respeito a `prefers-reduced-motion`. | ➖ |
| RNF11 | **Impressão:** estilos `@media print` mostram só o currículo. | ➖ |
| RNF12 | **Dependências externas:** Google Fonts e html2pdf.js 0.10.1 (CDN); PDF e fontes exigem internet. | ➖ |
| RNF13 | **Manutenibilidade:** botões do formulário definidos em `botoes.json` e gerados por script Python validado. | ✅ |
| RNF14 | **Testabilidade:** suíte automatizada (Node + jsdom) executável com `npm test`. | ✅ |
| RNF15 | O sistema é apresentado em português do Brasil. | ➖ |

## 2. Módulo Empresa (`empresa.html`)

### 2.1 Requisitos funcionais

| Código | Descrição | Onde | Teste |
|---|---|---|---|
| RFE01 | O sistema permite informar o nome e o setor da empresa. | `empresa.html` | ➖ |
| RFE02 | O sistema permite cadastrar várias vagas (título obrigatório, área, nível, requisitos separados por vírgula e descrição) e removê-las, mantendo ao menos uma. | `adicionarVaga`, `removerVaga` | ➖ |
| RFE03 | O sistema gera, para cada vaga com título, um texto de requisitos técnicos formatado (inclui empresa, setor, nível, área, lista de requisitos, descrição e data). Exige ao menos uma vaga com título. | `gerarRequisitos`, `gerarTextoRequisitos` | ➖ |
| RFE04 | O sistema copia o texto gerado para a área de transferência, com alternativa para navegadores antigos. | `copiarPrompt` | ➖ |
| RFE05 | O sistema apresenta o prompt do agente Gemini Gems, em cartão que pode ser expandido/recolhido, com links para o Gemini. | `toggleGemsPrompt`, `preencherGemsPrompt` | ➖ |
| RFE06 | O usuário seleciona uma vaga e o sistema prepara a mensagem de início do processo seletivo para enviar ao Gems. | `prepararParaGemini`, `gerarMensagemGemini` | ➖ |
| RFE07 | O fluxo é guiado em três passos, com opção de voltar a cada etapa. | `voltarPasso1`, `irParaPasso3`, `voltarPasso2` | ➖ |
| RFE08 | O sistema apaga todos os dados do módulo por "Limpar tudo", mediante confirmação. | `limparTudo` | ➖ |
| RFE09 | O sistema mostra avisos temporários (toast) de sucesso e erro. | `mostrarToast` | ➖ |

### 2.2 Requisitos não funcionais

| Código | Descrição |
|---|---|
| RNFE01 | Mesma stack do módulo Candidato (HTML, CSS e JS puros), sem backend. |
| RNFE02 | Texto digitado pelo usuário é escapado ao ser renderizado (`esc`). |
| RNFE03 | Layout responsivo (pontos de quebra em 680 e 400 px) e menu móvel. |
| RNFE04 | Não há integração automática com IA: o usuário copia e cola o conteúdo no Gemini. |

## 3. Limitações conhecidas (fora do escopo atual)

- O módulo Empresa não persiste dados (somente em memória) e não tem testes automatizados.
- Não há autenticação, banco de dados nem sincronização entre dispositivos.
- Com várias abas abertas, vale a última gravação.
- Aparência e responsividade foram validadas por leitura do código e testes em jsdom, não em navegadores reais.
