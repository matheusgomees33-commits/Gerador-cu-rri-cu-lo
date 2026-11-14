
# Gerador-curriculo
Projeto da faculdade 
# Gerador de Currículo · projeto focado em auxiliar pessoas a montarem seus currículos 

Aplicação web **estática** (HTML, CSS e JavaScript puros, sem build) para montar um currículo profissional com **pré-visualização em tempo real** e exportá-lo em **PDF** ou **HTML**. O projeto também inclui um módulo para empresas que organiza os requisitos das vagas e prepara o material para um processo seletivo no **Gemini Gems**.

## Módulos

| Módulo | Arquivos | O que faz |
|---|---|---|
| **Candidato** | `index.html`, `style.css`, `script.js` | Formulário em 4 abas, preview ao vivo, salvamento automático, exportação PDF/HTML. |
| **Empresa** | `empresa.html`, `empresa.css`, `empresa.js` | Cadastro de empresa e vagas, geração de requisitos técnicos e instruções para o Gemini Gems. |

> O módulo Empresa **não chama nenhuma API de IA**: ele gera textos/prompts para o usuário copiar e colar no Gemini.

## Funcionalidades (Candidato)

- Formulário dividido em **Pessoal · Experiência · Educação · Extras**, com botões *Anterior/Próximo*.
- **Preview em tempo real**; seções sem conteúdo não aparecem no currículo.
- **Salvamento automático** no navegador (`localStorage`), em até 1 s após parar de digitar, com indicador visual (*Salvando… / Salvo às HH:MM / erro*).
- Os dados são **restaurados** ao reabrir ou atualizar a página.
- **Barra de progresso** de preenchimento.
- **Limpar tudo** (com confirmação) apaga o formulário e os dados salvos.
- **Exportar PDF** (html2pdf.js) e **Exportar HTML**.
- Layout responsivo e estilos de impressão.

O formulário começa **vazio**: cada pessoa preenche com os próprios dados.

## Como executar

Não há instalação para usar o site. Basta abrir `index.html` no navegador.

Se preferir um servidor local:

```bash
python3 -m http.server 8000
# acesse http://localhost:8000
```

É necessária conexão com a internet para carregar as **fontes do Google** e a biblioteca **html2pdf.js** (CDN) usada no PDF. Sem internet, o site funciona, mas com fontes padrão e sem exportar PDF.

## Estrutura

```
.
├── index.html            # página do candidato
├── style.css
├── script.js             # lógica do candidato (abas, preview, salvamento, exportação)
├── empresa.html          # página da empresa
├── empresa.css
├── empresa.js
├── botoes.json           # definição dos botões do formulário
├── tools/
│   └── gerar_botoes.py   # gera os botões no index.html a partir do botoes.json
├── tests/                # testes automatizados (Node + jsdom)
├── docs/
│   ├── REQUISITOS.md     # requisitos funcionais e não funcionais
│   └── CODIGO-FONTE-BRUTO.txt  # HTML, CSS e JS reunidos em um único arquivo
└── package.json          # usado apenas para rodar os testes
```

## Botões gerados por Python

Os botões do formulário (Anterior, Próximo, Limpar tudo, PDF, HTML) ficam em `botoes.json` e são injetados no `index.html` por um script Python. O comportamento de cada um está no objeto `ACOES` do `script.js` (ligado pelo atributo `data-acao`).

```bash
python3 tools/gerar_botoes.py           # atualiza o index.html
python3 tools/gerar_botoes.py --check   # falha se o index.html estiver desatualizado
```

Para criar um botão: adicione-o em `botoes.json`, rode o script e registre a ação em `ACOES`.

## Testes

Requer **Node.js 20+** (validado em Node 22) e **Python 3**.

```bash
npm install
npm test
```

A suíte (25 testes, jsdom) cobre o módulo do candidato: salvamento, restauração, limpeza, falhas de armazenamento, escape de HTML, navegação, progresso e o gerador Python. O módulo Empresa **ainda não tem testes automatizados**.

## Privacidade e segurança

- Os dados do currículo ficam **apenas no navegador e no dispositivo** da pessoa (`localStorage`); nada é enviado a servidores pelo código do projeto.
- Todo texto digitado é **escapado** antes de ir para o preview e para o HTML exportado, evitando injeção de HTML/JS.
- Quem usa um computador compartilhado deve usar **Limpar tudo** ao terminar.

## Limitações conhecidas

- Se o armazenamento do navegador estiver bloqueado ou cheio, o site avisa e continua funcionando, mas sem salvar.
- Com a mesma página aberta em várias abas, vale a última gravação.
- O módulo Empresa guarda os dados só em memória: atualizar a página perde o que foi digitado.
- Não há login, banco de dados nem backend.

## Documentação

- [`docs/REQUISITOS.md`](docs/REQUISITOS.md): requisitos funcionais e não funcionais.
- [`docs/CODIGO-FONTE-BRUTO.txt`](docs/CODIGO-FONTE-BRUTO.txt): cópia do HTML, CSS e JavaScript em um só arquivo (instantâneo; os arquivos do projeto são a fonte oficial).

