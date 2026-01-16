# Deteritus

Um blogue pessoal minimalista construído com Hugo. Inclui múltiplos tipos de publicação, críticas com classificações, sistema de favoritos e design focado em tipografia limpa.

## Início Rápido

```bash
# Instalar Hugo (se não estiver instalado)
brew install hugo

# Servir localmente com live reload
hugo server -D

# Compilar para produção
hugo
```

O site estará disponível em `http://localhost:1313/`.

## Criar Publicações

Todas as publicações vão para o diretório `content/blog/`.

### Convenção de Nomes de Ficheiros

```
AAAA-MM-DD_nome-slug.md
```

Exemplo: `2024-02-25_sobre-leitura.md`

### Estrutura Básica de Publicação

Cada publicação precisa de frontmatter no início:

```yaml
---
slug: "o-teu-slug"        # Obrigatório: determina o URL (ex: /o-teu-slug/)
title: "Título da Publicação"
date: 2024-02-25
tags: ["tag1", "tag2"]
favorite: true           # Opcional: adiciona ícone de estrela
---

O teu conteúdo aqui...
```

**Importante:** O campo `slug` determina o URL. Uma publicação com `slug: "sobre-leitura"` estará disponível em `/sobre-leitura/`.

## Tipos de Publicação

### 1. Publicação Regular (padrão)

Publicação de blogue padrão com título e conteúdo.

```yaml
---
slug: "os-meus-pensamentos"
title: "Os Meus Pensamentos sobre X"
date: 2024-02-25
tags: ["pensamentos"]
favorite: true  # Opcional
---
```

### 2. Citação

Para partilhar citações sem título. Aparece com borda verde à esquerda e texto em itálico.

```yaml
---
slug: "citacao-sagan"
title: "Carl Sagan sobre a Leitura"  # Usado para arquivo/pesquisa
date: 2024-01-20
tags: ["citacoes"]
post_type: "quote"
attribution: "Carl Sagan"  # Opcional: apresentado separadamente com estilo diferente
---

O texto da citação aqui...
```

### 3. Ligação

Para partilhar ligações externas com comentário. O título liga ao URL externo com seta.

```yaml
---
slug: "artigo-interessante"
title: "The Web's Grain"
date: 2024-01-25
tags: ["ligacoes", "web"]
post_type: "link"
link_url: "https://exemplo.com/artigo"
favorite: true  # Opcional
---

O teu comentário sobre o artigo ligado...
```

### 4. Foto

Para partilhar fotos com legendas.

```yaml
---
slug: "luz-matinal"
title: "Luz Matinal"
date: 2024-02-01
tags: ["fotos", "natureza"]
post_type: "photo"
photo_url: "/img/luz-matinal.jpg"
---

Legenda e descrição...
```

### 5. Crítica de Álbum

Para críticas de álbuns de música com classificações. A capa aparece à direita com legenda.

```yaml
---
slug: "critica-ok-computer"
title: "OK Computer"
date: 2024-02-10
tags: ["musica", "criticas"]
post_type: "album"
artist: "Radiohead"
album: "OK Computer"
year: 1997
rating: "A+"
cover_url: "/img/ok-computer.jpg"  # Opcional
cover_caption: "Legenda personalizada aqui"  # Opcional - padrão é "Álbum (Ano)"
favorite: true  # Opcional
---

A tua crítica...
```

### 6. Crítica de Filme

Para críticas de filmes com classificações. O cartaz aparece à direita com legenda.

```yaml
---
slug: "critica-parasite"
title: "Parasite"
date: 2024-02-15
tags: ["cinema", "criticas"]
post_type: "movie"
year: 2019
rating: "A"
poster_url: "/img/parasite.jpg"  # Opcional
cover_caption: "Legenda personalizada aqui"  # Opcional - padrão é "Título (Ano)"
favorite: true  # Opcional
---

A tua crítica...
```

### 7. Crítica de Livro

Para críticas de livros com classificações. A capa aparece à direita com legenda.

```yaml
---
slug: "critica-matadouro-cinco"
title: "Matadouro Cinco"
date: 2024-02-20
tags: ["livros", "criticas"]
post_type: "book"
author: "Kurt Vonnegut"
year: 1969
rating: "A+"
cover_url: "/img/matadouro-cinco.jpg"  # Opcional
cover_caption: "Legenda personalizada aqui"  # Opcional - padrão é "Autor, Ano"
favorite: true  # Opcional
---

A tua crítica...
```

## Sistema de Classificação

As críticas usam notas em letras: **A+, A, A-, B+, B, B-, C+, C, C-, D+, D, D-, F**

As classificações aparecem como emblemas coloridos junto ao título:
- **A+, A**: Verde (excelente)
- **A-, B+**: Verde claro (muito bom)
- **B, B-**: Oliveira (bom)
- **C+, C, C-**: Cinzento (médio)
- **D+, D, D-**: Bege (abaixo da média)
- **F**: Rosa acastanhado (fraco)

## Favoritos

Adiciona `favorite: true` ao frontmatter de qualquer publicação para a marcar como favorita. Um ícone de estrela aparecerá junto ao título, e a publicação será listada na secção Favoritos da página Arquivo.

## Imagens

Coloca imagens em `static/img/`. Referencia-as no frontmatter como `/img/nome-ficheiro.jpg`.

## Estrutura do Site

- **Início** (`/`): Últimas 10 publicações com paginação
- **Arquivo** (`/archive/`): Todas as publicações organizadas por Favoritos, Tags, tipo de publicação (Filmes, Álbuns, Livros) e cronologicamente
- **Galeria** (`/gallery/`): Grelha de todas as publicações de foto
- **Pesquisa** (`/search/`): Pesquisa do lado do cliente com Pagefind
- **Sobre** (`/about/`): Página sobre
- **Colofão** (`/colophon/`): Créditos do site e detalhes técnicos
- **Tags** (`/tags/nome-tag/`): Publicações filtradas por tag, agrupadas por ano

## Funcionalidades

- **URLs limpos**: Publicações acessíveis em `/:slug/` (ex: `/sobre-leitura/`)
- **Modo escuro**: Segue automaticamente a preferência do sistema
- **Publicações similares**: Mostra publicações relacionadas baseadas em tags partilhadas
- **Ligações**: Mostra que publicações ligam à publicação atual
- **Feed RSS**: Disponível em `/index.xml`
- **Design responsivo**: Otimizado para mobile e desktop

## Personalização do Tema

Os ficheiros do tema estão em `themes/deteritus/`:

- `layouts/`: Templates HTML
- `static/css/style.css`: Todos os estilos com propriedades CSS personalizadas
- `static/fonts/`: Ficheiros de fonte Iowan Old Style

### Variáveis CSS

Edita cores em `static/css/style.css`:

```css
:root {
  --bg: #fffff8;      /* Fundo */
  --text: #111;       /* Cor do texto */
  --muted: #666;      /* Texto secundário */
  --link: #2a7a4b;    /* Cor das ligações (verde floresta) */
  --border: #ddd;     /* Cor das bordas */
}
```

## Reconstruir Índice de Pesquisa

Após adicionar novas publicações localmente, reconstrói o índice Pagefind:

```bash
hugo && npx pagefind --site public
```

## Deployment

O site é automaticamente publicado no GitHub Pages quando fazes push para o branch `main`.

### Configuração Inicial

1. **Criar repositório GitHub** (se ainda não feito):
   ```bash
   git init
   git add .
   git commit -m "Commit inicial"
   git remote add origin git@github.com:tiagojct/deteritus.com.git
   git push -u origin main
   ```

2. **Ativar GitHub Pages**:
   - Vai a Definições do repositório → Pages
   - Em "Build and deployment", seleciona **GitHub Actions**

3. **Configurar domínio personalizado** (opcional):
   - Em Definições → Pages, adiciona o teu domínio personalizado
   - Configura DNS com o teu fornecedor de domínio:
     - Para domínio apex (deteritus.com): Adiciona registos A apontando para os IPs do GitHub
     - Para subdomínio www: Adiciona registo CNAME apontando para `tiagojct.github.io`

### Deployment Manual

Para publicar manualmente:
```bash
git add .
git commit -m "A tua mensagem de commit"
git push
```

O GitHub Action irá automaticamente compilar o Hugo, gerar o índice de pesquisa e publicar.

## Criar Publicações

Usa o script `newpost` para criar publicações a partir de archetypes:

```bash
./newpost <tipo> <slug>
```

### Tipos Disponíveis

| Tipo | Utilização |
|------|------------|
| `post` | Publicações de blogue regulares |
| `quote` | Publicações de citação com atribuição |
| `link` | Publicações de ligação externa |
| `photo` | Publicações de foto |
| `album` | Críticas de álbuns de música |
| `movie` | Críticas de filmes |
| `book` | Críticas de livros |

### Exemplos

```bash
./newpost post os-meus-pensamentos
./newpost quote sagan-sobre-livros
./newpost link artigo-interessante
./newpost album critica-ok-computer
./newpost movie critica-parasite
./newpost book matadouro-cinco
```

Isto cria um ficheiro como `content/blog/2024-01-15_os-meus-pensamentos.md` com o template de frontmatter apropriado.

## Referência de Frontmatter

| Campo | Tipo | Usado Por | Descrição |
|-------|------|-----------|-----------|
| `slug` | string | Todos | Caminho URL (obrigatório) |
| `title` | string | Todos | Título da publicação |
| `date` | date | Todos | Data de publicação |
| `tags` | array | Todos | Lista de tags |
| `post_type` | string | Todos | post, quote, link, photo, album, movie, book |
| `favorite` | bool | Todos | Mostra ícone de estrela |
| `attribution` | string | quote | Atribuição da citação (autor, fonte) |
| `link_url` | string | link | URL externo |
| `photo_url` | string | photo | Caminho da imagem |
| `artist` | string | album | Nome do artista |
| `album` | string | album | Título do álbum |
| `author` | string | book | Nome do autor |
| `year` | int | album, movie, book | Ano de lançamento |
| `rating` | string | album, movie, book | Nota em letra (A+ a F) |
| `cover_url` | string | album, book | Caminho da imagem de capa |
| `poster_url` | string | movie | Caminho da imagem do cartaz |
| `cover_caption` | string | album, movie, book | Legenda personalizada para imagem de capa/cartaz |
