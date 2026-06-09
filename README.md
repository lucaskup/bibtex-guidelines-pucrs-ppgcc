<!-- Made by Lucas S. Kupssinskü -->

# Guia rápido de BibTeX para teses e dissertações

Best practices using BibTeX citations — PPGCC/PUCRS.

Material de consulta para padronizar o arquivo `.bib`. O objetivo é evitar os problemas que aparecem na homologação dos volumes finais: referências sem página inicial e final, sem DOI, e com o mesmo periódico ou evento escrito de formas diferentes.

Contexto assumido: arquivo `.bib` único, com citações feitas via `natbib`, usando `\citet` e `\citep`.

---

## 1. Dicionário de nomes com `@string`

A causa mais comum de falta de padronização é o mesmo periódico ou evento ser digitado de formas diferentes em cada referência. A solução é definir o nome uma única vez como uma macro e reusar essa macro em todas as entradas.

```bibtex
@string{jmlr    = "Journal of Machine Learning Research"}
@string{neurips = "Advances in Neural Information Processing Systems ({NeurIPS})"}
@string{cvpr    = "{IEEE/CVF} Conference on Computer Vision and Pattern Recognition (CVPR)"}
```

Uso dentro das entradas, sem aspas, referenciando a macro:

```bibtex
@article{silva2023,
  ...
  journal = jmlr,
  ...
}
```

Regras práticas:

* Os blocos `@string` devem aparecer no início do `.bib`, antes de qualquer entrada que os utilize.
* Defina cada nome uma vez só, com uma única chave, e nunca redigite o nome completo nas entradas.
* Use sempre o nome oficial completo do periódico ou evento, não a sigla isolada.
* Proteja siglas e tokens em maiúsculas com chaves, como `{IEEE/CVF}` ou `({ASYNC})`, para o estilo não alterar a capitalização.

A lista completa, com as principais conferências e periódicos da área, está no arquivo [`exemplo.bib`](exemplo.bib) que acompanha este guia. Os exemplos acima já seguem a convenção usada lá: sigla entre parênteses no fim e maiúsculas protegidas por chaves.

---

## 2. Tipos de entrada e quando usar cada um

> **Aviso importante sobre campos não lidos.** Vários estilos clássicos de BibTeX leem apenas um conjunto fixo de campos e ignoram silenciosamente campos como `doi`, `issn`, `url` e `month`, mesmo que estejam preenchidos corretamente no `.bib`. Por isso, para garantir que DOI e ISSN apareçam na referência, coloque essas informações no campo `note`, que é um campo de texto livre presente em todos os tipos de entrada. Detalhes na seção 8.

| Tipo | Quando usar | Campos que costumam faltar |
|---|---|---|
| `@article` | Periódicos e preprints arXiv | páginas inicial e final, ISSN |
| `@inproceedings` | Artigos em eventos, conferências, workshops | páginas inicial e final |
| `@phdthesis` | Tese de doutorado | school, year |
| `@mastersthesis` | Dissertação de mestrado | school, year |
| `@misc` | Referências online, blogs, white papers | data de acesso, url |
| `@techreport` | Patentes (ver seção 7) | número, depositante |

---

## 3. `@article`: periódicos e arXiv

Use para artigos de periódico e para preprints do arXiv.

### Artigo de periódico

Campos obrigatórios: `author`, `title`, `journal`, `year`, `volume`, `pages`.
Campos recomendados na homologação: `number`, e o DOI e o ISSN dentro de `note`, já que muitos estilos não leem os campos `doi` e `issn` separados.

```bibtex
@article{silva2023,
  author  = {Silva, Ana and Souza, Bruno},
  title   = {Um título descritivo do artigo},
  journal = jmlr,
  year    = {2023},
  volume  = {24},
  number  = {7},
  pages   = {1450--1460},
  note    = {ISSN 1532-4435. DOI: \url{https://doi.org/10.0000/exemplo.2023.123}}
}
```

Regra das páginas. A entrada deve ter página inicial e final. Muitos periódicos modernos informam apenas o número do artigo e a quantidade de páginas, sem a página final. Nesse caso, calcule a página final assim:

```
página final = página inicial + número de páginas do artigo - 1
```

Exemplo: o artigo começa na página 1450 e tem 11 páginas, então a final é 1450 + 11 - 1 = 1460, e o campo fica `pages = {1450--1460}`. Note o uso de dois hifens, `--`, que o BibTeX converte no traço de intervalo.

### Preprint arXiv

O arXiv não tem ISSN nem intervalo de páginas. A forma mais portável, que funciona na maioria dos estilos, é colocar a identificação no próprio campo `journal`:

```bibtex
@article{costa2024,
  author  = {Costa, Carla},
  title   = {Título do preprint},
  journal = {arXiv preprint arXiv:2401.01234},
  year    = {2024}
}
```

---

## 4. `@inproceedings`: artigos em eventos

Use para conferências e workshops.

Campos obrigatórios: `author`, `title`, `booktitle`, `year`, `pages`.
Campos recomendados: `publisher` ou `organization`, e o DOI dentro de `note`.

```bibtex
@inproceedings{lima2022,
  author    = {Lima, Diego},
  title     = {Título do artigo de conferência},
  booktitle = neurips,
  year      = {2022},
  pages     = {1123--1135},
  publisher = {Curran Associates},
  note      = {DOI: \url{https://doi.org/10.0000/exemplo.conf.2022}}
}
```

A regra das páginas inicial e final da seção 3 vale igualmente aqui.

---

## 5. `@phdthesis` e `@mastersthesis`: teses e dissertações

No Brasil, tese é de doutorado e dissertação é de mestrado. Isso mapeia direto nos dois tipos:

* Tese de doutorado, use `@phdthesis`.
* Dissertação de mestrado, use `@mastersthesis`.

Campos obrigatórios: `author`, `title`, `school`, `year`.
Campos recomendados: `type` (para deixar explícito o grau e o programa), `address`.

```bibtex
@phdthesis{pereira2021,
  author  = {Pereira, Eduardo},
  title   = {Título completo da tese},
  school  = {Pontifícia Universidade Católica do Rio Grande do Sul},
  year    = {2021},
  type    = {Tese de Doutorado em Ciência da Computação},
  address = {Porto Alegre}
}

@mastersthesis{rocha2020,
  author  = {Rocha, Fernanda},
  title   = {Título completo da dissertação},
  school  = {Pontifícia Universidade Católica do Rio Grande do Sul},
  year    = {2020},
  type    = {Dissertação de Mestrado em Ciência da Computação},
  address = {Porto Alegre}
}
```

---

## 6. `@misc`: referências online, blogs e white papers

Use para conteúdo que não é artigo de periódico nem de evento, como páginas web, posts de blog e white papers de empresas.

Campos obrigatórios na prática: `author` ou `title`, `year`, `howpublished`, `note`.
A data de acesso é obrigatória e vai no campo `note`.

```bibtex
@misc{openai2023,
  author       = {{OpenAI}},
  title        = {Título do white paper ou da página},
  year         = {2023},
  howpublished = {\url{https://exemplo.com/pagina}},
  note         = {Acessado em: 8 jun. 2026}
}
```

Observações:

* Use chaves duplas em `{{OpenAI}}` quando o autor for uma organização, para evitar que o BibTeX trate o nome como sobrenome.
* `\url{}` exige o pacote `url` ou `hyperref` no preâmbulo do documento.

---

## 7. Patentes

O BibTeX clássico não tem um tipo `@patent`, esse tipo existe apenas no `biblatex`. A forma mais limpa é usar `@techreport`, porque ele imprime campos estruturados que servem bem para patentes: `type` para o rótulo, `number` para o registro, `institution` para o depositante.

```bibtex
@techreport{martins2019patente,
  author      = {Martins, Gabriel},
  title       = {Título da invenção},
  year        = {2019},
  type        = {Patente},
  number      = {BR 10 2019 000000 0},
  institution = {Nome da instituição depositante},
  address     = {Brasília},
  note        = {\url{https://exemplo.inpi.gov.br/registro}}
}
```

Se em algum momento o programa migrar para `biblatex` com `biber`, passe a usar o tipo dedicado `@patent`, que tem campos próprios como `number` e `holder`.

---

## 8. Atenção: DOI e ISSN

Esta é a provável causa raiz das referências aparecerem sem DOI na homologação. Muitos estilos clássicos de BibTeX só reconhecem um conjunto fixo de campos. Os campos `doi`, `issn`, `url` e `month` costumam ficar fora dessa lista, então são lidos como inexistentes e nunca aparecem na bibliografia, por mais que estejam preenchidos no `.bib`. O campo continua no arquivo, mas o estilo não tem como imprimi-lo.

Solução prática e portável: coloque DOI e ISSN dentro do campo `note`, que aparece em todos os tipos de entrada. Exemplo:

```bibtex
note = {ISSN 1532-4435. DOI: \url{https://doi.org/10.0000/exemplo.2023.123}}
```

Dois detalhes para o `\url` funcionar:

* O preâmbulo do documento precisa carregar o pacote `url` ou o `hyperref`.
* O DOI completo, como link `https://doi.org/...`, é preferível ao número solto, porque fica clicável e dispensa o leitor procurar.

Usar o campo `note` é a forma mais simples e funciona em qualquer estilo. Editar o próprio `.bst` para tratar `doi` e `issn` é possível, mas é trabalhoso e foge do escopo deste guia.

---

## 9. Citações com `natbib`: `\citet` e `\citep`

Use os dois comandos do `natbib` conforme o papel da citação na frase:

* `\citet` para citação textual, quando o autor faz parte da frase. Exemplo: `\citet{silva2023}` gera algo como "Silva e Souza (2023) propuseram...".
* `\citep` para citação entre parênteses, quando a referência vem como apoio e não como sujeito. Exemplo: `\citep{silva2023}` gera algo como "... como já demonstrado (Silva e Souza, 2023)".

---

## 10. Checklist antes da entrega do volume final

* Todos os nomes de periódicos e eventos vêm de macros `@string`, sem variação de digitação.
* Toda entrada `@article` de periódico e toda `@inproceedings` tem página inicial e final, no formato `inicial--final`.
* Páginas finais ausentes foram calculadas como inicial mais o número de páginas menos um.
* DOI e ISSN estão no campo `note` e aparecem de fato na referência formatada, já que muitos estilos ignoram os campos `doi` e `issn` soltos.
* Teses e dissertações usam `@phdthesis` e `@mastersthesis` com `school` e `year`.
* Referências online em `@misc` têm data de acesso no campo `note`.
* As citações no texto usam `\citet` na forma textual e `\citep` na forma entre parênteses.
* O documento compila sem avisos de campos obrigatórios faltando.

---

## 11. Coerência de idioma entre o `.bib` e o documento

Alguns campos do `.bib` são texto livre escrito por você e não são traduzidos automaticamente. Eles precisam estar no mesmo idioma do documento final. Se a tese é em inglês, esses campos vão em inglês, se é em português, vão em português.

Os campos mais sensíveis são:

* O `type` de teses e dissertações. Em português, algo como "Tese de Doutorado em Ciência da Computação", em inglês, "PhD Thesis in Computer Science".
* O `type` de patentes em `@techreport`. Em português "Patente", em inglês "Patent".
* A data de acesso no `note` das referências online. Em português "Acessado em: 8 jun. 2026", em inglês "Accessed on: June 8, 2026".

Os nomes de periódicos e conferências do dicionário `@string` são uma exceção, eles ficam no idioma oficial da publicação, normalmente inglês, e não mudam com o idioma do documento.

Vale lembrar que termos gerados automaticamente, como o "In:" antes do nome do evento ou o rótulo padrão de tese, vêm do estilo de bibliografia e não do `.bib`. A coerência desses termos com o idioma do documento é definida junto com o estilo, nos outros documentos do processo.

