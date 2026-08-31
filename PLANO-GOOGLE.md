# Plano: aparecer no Google

**Não implementado.** Escrito em 30/08, quando o Edu disse: *"quando digito
'maxIptvOficial' na busca, nunca aparecemos"*.

---

## O que eu medi

| verificação | resultado |
|---|---|
| site no ar | ✅ `HTTP 200`, GitHub Pages, HTTPS ok |
| `<title>` | ✅ "Max IPTV — Filmes, séries e canais ao vivo, numa TV só" |
| `<meta description>` | ✅ presente e boa |
| `robots.txt` | ❌ **404** |
| `sitemap.xml` | ❌ **404** |
| `<link rel="canonical">` | ❌ ausente |
| tags `og:` (link em WhatsApp/redes) | ❌ ausentes |
| `www.maxiptvoficial.com.br` | ❌ **não responde em HTTPS** |
| busca por "maxiptvoficial" | ❌ nenhum resultado do site |

---

## A causa principal, sem rodeios

**O site não está no índice do Google.** Isso não é penalidade nem erro de
configuração — é o estado normal de um site novo que ninguém apontou para o
Google e para o qual nenhum outro site aponta.

O Google não varre a internet inteira à procura de domínios novos. Ele chega a
uma página por um **link** de outra já conhecida, ou porque **alguém pediu**.
O nosso site tem uma página só, nenhum link externo apontando para ele, e nunca
foi submetido. Não há por onde ele chegar.

Título e descrição estarem bons não ajuda em nada enquanto isso — eles decidem
como o resultado **aparece**, não se ele existe.

---

## O plano

### 1. Google Search Console ⭐ é isto que resolve

`search.google.com/search-console` → adicionar `maxiptvoficial.com.br` →
verificar a posse (ele vai pedir um registro **TXT** no Registro.br — mesmo
lugar onde a gente pôs o DKIM hoje) → **URL Inspection** → *Request indexing*.

É o passo que tira o site do limbo. Sem ele, todo o resto é decoração.

Depois de submeter, o Search Console também passa a **dizer** por que uma
página não está indexada, em vez de a gente adivinhar. Vale por isso sozinho.

⏱️ Indexar leva de alguns dias a duas semanas. Não é instantâneo, e insistir no
botão não acelera.

### 2. `robots.txt` e `sitemap.xml`

Dois arquivos, na raiz do repositório:

`robots.txt`
```
User-agent: *
Allow: /

Sitemap: https://maxiptvoficial.com.br/sitemap.xml
```

`sitemap.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url><loc>https://maxiptvoficial.com.br/</loc></url>
</urlset>
```

Com uma página só, o sitemap quase não faz diferença sozinho — mas é o que o
Search Console pede, e custa dois arquivos.

> A ausência de `robots.txt` **não** estava bloqueando nada: sem ele, o padrão é
> "pode entrar". Vale criar por higiene e pelo sitemap, não porque era o bug.

### 3. Consertar o `www`

`www.maxiptvoficial.com.br` tem CNAME no DNS mas **não responde em HTTPS** — o
certificado do GitHub Pages não cobre esse nome. Quem digitar "www" vê erro de
segurança.

Não é o motivo de não aparecer na busca, mas é um jeito real de perder visita —
e "erro de certificado" num site de assinatura destrói confiança na hora.

No GitHub Pages: **Settings → Pages**, conferir o domínio e reemitir o
certificado com o `www` incluído.

### 4. Canonical e Open Graph

No `<head>`:

```html
<link rel="canonical" href="https://maxiptvoficial.com.br/">
<meta property="og:title" content="Max IPTV — Filmes, séries e canais ao vivo, numa TV só">
<meta property="og:description" content="...">
<meta property="og:image" content="https://maxiptvoficial.com.br/og.png">
<meta property="og:url" content="https://maxiptvoficial.com.br/">
```

O canonical evita que raiz e `www` sejam vistos como dois sites. As `og:`
decidem como o link aparece quando **você** manda no WhatsApp — e, no começo, é
daí que vem quase toda a visita. Hoje o link vai "pelado".

### 5. Links de fora (o que realmente move o ponteiro depois)

Indexar te faz **existir** na busca. Aparecer bem depende de alguém apontar para
você. Sem gastar nada:

- perfis de rede social da marca com o link na bio;
- o link do site nas fichas das lojas (Samsung, App Store) quando publicarmos —
  esses são links de peso;
- os apps já mandam gente para `app.maxiptvoficial.com.br` no pareamento.

---

## Expectativa honesta

Feito o passo 1, buscar **"maxiptvoficial"** deve trazer o site em dias — é uma
palavra que ninguém mais disputa, então basta estar no índice.

Buscar **"iptv"** ou **"assistir tv online"** é outra conversa: são termos
disputados por gente que investe há anos. Não é meta para agora.

⚠️ E vale saber: "IPTV" é um termo que buscadores e lojas tratam com
desconfiança, por associação com pirataria. Isso afeta anúncios pagos e revisão
de loja mais do que a busca comum, mas é bom não ser pego de surpresa — o site
deixar claro que é serviço legítimo, com CNPJ e contato, ajuda de verdade.

---

## Ordem

1. **Search Console + Request indexing** — sem isso, nada acontece;
2. `robots.txt` + `sitemap.xml` — dois arquivos, 5 minutos;
3. canonical + `og:` — melhora o link compartilhado hoje;
4. `www` no certificado;
5. links de fora, contínuo.
