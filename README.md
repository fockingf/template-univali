# Template de Trabalho Acadêmico — UNIVALI

Template LaTeX reutilizável para **TCC, monografias, dissertações de mestrado e
teses de doutorado**, em conformidade com as normas ABNT adotadas pela UNIVALI:

- **NBR 14724:2024** — apresentação de trabalhos acadêmicos
- **NBR 6023:2025** — referências
- **NBR 10520:2023** — citações

Baseado em [abnTeX2](https://www.abntex.net.br/). O texto vem preenchido com
*lorem ipsum* e placeholders — substitua pelo seu conteúdo.

## Como começar

1. **Escolha o tipo de trabalho** na primeira linha de `thesis.tex`, na opção do
   `\documentclass` (veja a tabela abaixo).
2. **Preencha os metadados** (título, autor, orientador, data, curso) no início
   de `thesis.tex`.
3. **Escreva os capítulos** em `tex/*.tex` (substitua o lorem ipsum).
4. **Cadastre as referências** em `referencias.bib` e cite com `\parencite{}` /
   `\textcite{}`.
5. **Compile** (veja "Compilação").

## Tipos de trabalho (opções do `\documentclass`)

Escolha **um** nível. Combine com os modificadores quando fizer sentido.

| Opção                       | Tipo de trabalho             | Grau               |
|-----------------------------|------------------------------|--------------------|
| `graduacao` ou `tcc`        | Trabalho de Conclusão de Curso | Bacharel         |
| `especializacao` ou `monografia` | Monografia              | Especialista       |
| `mestrado`                  | Dissertação                  | Mestre / Mestra    |
| `doutorado` (padrão)        | Tese                         | Doutor / Doutora   |

Modificadores: `qualificacao` (só mestrado/doutorado), `pre-defesa`,
`pos-defesa` (padrão), `impressao`.

Exemplos:
```latex
\documentclass[tcc]{packages/ppg}
\documentclass[especializacao]{packages/ppg}
\documentclass[mestrado,qualificacao]{packages/ppg}
\documentclass[doutorado,pos-defesa]{packages/ppg}
```

### Curso / instituição

A pós-graduação em Computação Aplicada da UNIVALI já vem pré-configurada via
`\curso{PPGC}`. Para **qualquer outro curso** (inclusive TCC/monografia), passe o
nome completo e defina a área e a especialidade você mesmo:

```latex
\curso{Curso de Ciência da Computação -- UNIVALI}
\area{Engenharia de Software}
\especialidade{em Ciência da Computação}   % completa "...título de Bacharel em Ciência da Computação"
```

Para mudar o cabeçalho impresso no topo da **capa**, use `\capainstituicao{...}`
(opcional; só é necessário se não usar o PPGCA). A cidade da capa/folha de rosto
é definida com `\local{...}`.

## Compilação

Use **XeLaTeX** ou **LuaLaTeX** (não pdfLaTeX — o template usa as fontes Arial e
Times em `packages/fonts/` via `fontspec`). A bibliografia usa **biber**.

```sh
xelatex thesis
biber thesis
makeglossaries thesis   # gera a lista de siglas
xelatex thesis
xelatex thesis
```

Ou, com latexmk: `latexmk -xelatex thesis.tex` (rode `makeglossaries thesis`
manualmente se a lista de siglas ficar vazia).

No **Overleaf**: Menu → *Settings* → *Compiler* = **XeLaTeX**; documento
principal = `thesis.tex`. O Overleaf executa biber automaticamente.

## Estrutura de arquivos

```
template/
├── thesis.tex            # arquivo principal: opções, metadados, inclusão dos capítulos
├── referencias.bib       # base de referências (biblatex)
├── packages/
│   ├── ppg.cls           # classe UNIVALI (formatação ABNT) — não precisa editar
│   └── fonts/            # fontes Arial e Times (TTF)
├── imagens/              # suas figuras
└── tex/
    ├── _placeholders.tex # utilitário \figuraplaceholder (pode remover no fim)
    ├── introducao.tex
    ├── fundamentacao-teorica.tex
    ├── materiais-e-metodos.tex
    ├── resultados.tex
    ├── conclusao.tex
    └── pre-textual/      # dedicatória, agradecimentos, epígrafe, declaração de IA
```

## Recursos demonstrados nos capítulos

- Citações: `\parencite{}`, `\textcite{}`, `\parencite[p. 12]{}`
- Figura com legenda e fonte (e placeholder `\figuraplaceholder`)
- Equação numerada e referenciável (`\autoref{eq:...}`)
- `quadro` (tabela com bordas) e tabela `booktabs`
- Gráfico com `pgfplots`
- Listagem de código (`lstlisting`)
- Listas (`itemize`, `enumerate` com rótulos personalizados)
- Lista de siglas via `\nomenclature[S]{}`

## Figuras, tabelas e quadros (legenda e fonte)

Conforme a NBR 14724:2024 (5.8/5.9): a **legenda** vai **acima** (centralizada) e a
**fonte** vai **abaixo, alinhada à margem esquerda do elemento** — ambas em tamanho
**menor e uniforme**. Para isso, envolva o elemento (imagem, `tikzpicture` ou
`tabular`) com **`\elementocomfonte{<elemento>}{Fonte: ...}`**, que mede a largura do
elemento e alinha a fonte à sua borda esquerda:

```latex
\begin{figure}[!htb]
  \caption{Sua legenda.}        % legenda ACIMA, centralizada
  \label{fig:minha}
  \centering
  \elementocomfonte{\includegraphics[width=0.7\textwidth]{imagens/figura.png}}%
    {Fonte: Elaborada pelo autor (\the\year{}).}
\end{figure}
```

O mesmo vale para `table` (tabular `booktabs`) e `quadro` (tabular com bordas) — veja
os exemplos prontos em `tex/fundamentacao-teorica.tex`, `tex/resultados.tex` e
`tex/materiais-e-metodos.tex`. Use `\figuraplaceholder{largura}{altura}{caminho}`
como espaço reservado enquanto não tiver a imagem real.

## Elementos opcionais

No `thesis.tex`, descomente conforme necessário:
- `\incluifichacatalografica{...}` — ficha catalográfica (obrigatória na versão final)
- `\incluifolhadeaprovacao{...}` — folha de aprovação (após a defesa)
- listas de tabelas / algoritmos / códigos
- ambientes `apendicesenv` (apêndices) e `anexosenv` (anexos)
