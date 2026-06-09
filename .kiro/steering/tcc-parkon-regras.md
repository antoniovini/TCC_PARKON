---
inclusion: always
---

# Regras obrigatórias — Projeto TCC PARKON (LaTeX / ABNT)

> Este documento é de leitura OBRIGATÓRIA antes de qualquer alteração no projeto.
> Ele consolida as decisões e regras definidas pelo autor. Siga-as sempre.

## Contexto do projeto

- O projeto é a versão LaTeX do TCC "Proposta de um Aplicativo para Reserva de Vagas
  de Estacionamento: Estudo de Caso do Parkon", de ANTÔNIO VINICIUS FREITAS FERREIRA,
  curso de Sistemas de Informação da Universidade Federal Fluminense (UFF).
- O projeto LaTeX fica em `TCC-PARKON/` e é AUTOCONTIDO — já inclui todos os arquivos do
  template **PPGC-UFF-Latex-Abnt** (ABNT) necessários para compilar e manter o padrão.
  (A pasta `PPGC-UFF-Latex-Abnt/` no workspace é apenas o template original de origem, com
  exemplos; NÃO é necessária para o projeto.)
- A **fonte da verdade do conteúdo** é o arquivo `TCC_PARKON_V3.pdf` (na raiz do workspace).
- Distribuição: **MiKTeX** (Windows). Binários em `%LOCALAPPDATA%\Programs\MiKTeX\miktex\bin\x64`.
- Repositório Git remoto: `git@github.com:antoniovini/TCC_PARKON.git` (branch `main`).
- Especificação completa: `.kiro/specs/tcc-parkon-latex-migration/` (requirements, design, tasks).

## Regra 1 — Fidelidade ao template (NÃO inventar)

- O projeto é AUTOCONTIDO: todos os arquivos "motor" do template PPGC-UFF-Latex-Abnt
  (`PPGC_UFF.cls`, `cvs-id.def`, `biblatex-abnt/`, `*.lbx`) já estão dentro de `TCC-PARKON/`,
  idênticos ao original (verificado por hash MD5). Eles definem TODOS os comandos/métodos
  ABNT (`\autor`, `\titulo`, `\capa`, `\folhaderosto`, `resumo`, `abstract`,
  `\pretextualchapter`, fontes ABNT etc.). NÃO é necessário consultar a pasta do template
  externa — o próprio projeto é a referência canônica de formato.
- Esses arquivos do template (.cls, .def, .lbx, biblatex-abnt/) são tratados como
  IMUTÁVEIS: não editar.
- NÃO inventar conteúdo. Todo texto, título, legenda, tabela e referência deve vir do
  `TCC_PARKON_V3.pdf`. Quando algo não existir na fonte, OMITIR o elemento (como manda o
  template) e deixar um comentário `% TODO:` / `% NOTA:` explicando — nunca preencher com
  texto fictício.
- Para entender como um comando da classe funciona, leia diretamente o `PPGC_UFF.cls`
  do próprio projeto.

## Regra 2 — Layout de figuras e tabelas (posição fixa)

- Telas/screenshots em formato retrato NÃO podem usar `width=0.8\linewidth` (estouram a
  página A4). Usar dimensionamento por altura: `height=0.34\textheight,keepaspectratio`
  (ou `0.42\textheight` para figura isolada).
- Diagramas (paisagem/quadrados) usam largura proporcional ao seu formato.
- **Tabelas e figuras devem ficar FIXAS na posição** com `[H]` (pacote `float`, já incluído
  no preâmbulo). NÃO usar `[!ht]`/`[htbp]` para tabelas nem para as telas do app — isso faz
  os floats "vazarem" para outras seções, deixando títulos sem conteúdo e jogando imagens na
  seção errada (problemas já corrigidos; não reintroduzir).
- Pares de telas descritos juntos no texto ("A Figura X... Já a Figura Y...") ficam
  **lado a lado** num único `figure`, cada imagem em sua `minipage` com legenda e `\label`
  próprios. As imagens vêm ANTES do parágrafo que as descreve (como no PDF original).
- Toda referência cruzada usa `\autoref{}`; rótulos seguem `cap:`, `sec:`, `fig:`, `tab:`.

## Regra 3 — Datas

- As datas do trabalho (capa, folha de rosto, folha de aprovação em `pre-textuais/cap0.tex`)
  são **2026**: `\data{2026}`, "Aprovada em dezembro de 2026", "Niterói / 2026".
- NÃO alterar as datas/anos das REFERÊNCIAS (chaves como `flutter2025`, `gradin2025` e as
  datas de "Acesso em 08 de dez. 2025"). Esses são anos de acesso/publicação reais das fontes;
  mudá-los quebra as citações e falsifica os dados bibliográficos.

## Regra 4 — Bibliografia e siglas

- Bibliografia em `bibliografia.bib` (biblatex, `style=abnt`, `backend=biber`). Chaves no
  padrão `{sobrenomePrimeiroAutor}{ano}`. Não renomear chaves já citadas nos capítulos.
- Entradas órfãs (não citadas) são aceitáveis, apenas sinalizadas com `% NOTE: [ORPHAN]`.
- Siglas em `pre-textuais/acronimos.tex` via `glossaries`. Primeira ocorrência no corpo usa
  `\acrfull{}` (ou `\acrfullpl{}`); subsequentes usam `\acrshort{}`/`\acrshortpl{}`.
- Códigos literais como `RF01`, `RNF07` são mantidos verbatim (não trocar por comandos de sigla).

## Regra 5 — Compilação (pipeline obrigatório)

Sempre rodar a partir de `TCC-PARKON/`, com os binários do MiKTeX, nesta ordem:

```
pdflatex -interaction=nonstopmode tese.tex
biber tese
makeglossaries-lite tese      <-- usar a versao "-lite" (a maquina NAO tem Perl)
pdflatex -interaction=nonstopmode tese.tex
pdflatex -interaction=nonstopmode tese.tex
```

- Critério de sucesso: 0 erros (`^!`), 0 "Float too large", 0 referências/citações indefinidas.
  Avisos benignos (babel deprecated, etc.) são aceitáveis.
- `makeglossaries` padrão FALHA (sem Perl). Usar SEMPRE `makeglossaries-lite`.
- Limpar arquivos temporários/logs criados durante a verificação ao terminar.

## Regra 6 — Git

- Não versionar auxiliares do LaTeX (já cobertos pelo `.gitignore`: `*.aux`, `*.log`, `*.bbl`,
  `*.glo`, etc.) nem `pdf_full.txt`. O `tese.pdf` É versionado (é o entregável).
- O autor normalmente faz o push manualmente. Só fazer commit/push quando explicitamente pedido.

## Idioma

- Conversar e documentar em português.
