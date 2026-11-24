# Landing page — Torres & Vilas Boas Advocacia

HTML/CSS puro, sem framework e sem bibliotecas. Fontes e ícones ficam dentro do projeto (nada vem do Google Fonts nem de CDN).

```
public/index.html          ← a página
public/obrigado/index.html ← página de conversão: registra o lead e abre o WhatsApp
public/fonts/              ← Playfair Display + DM Sans (woff2, só os pesos usados)
public/img/foto-1..3.webp  ← fotos do carrossel (1 Lucas, 2 Murilo, 3 os dois)
public/favicon.png         ← monograma TVB (ícone da aba)
public/img/logo.webp       ← logo (fundo transparente)
```

## 1. O que trocar antes de publicar

| O quê | Onde |
|---|---|
| Número do WhatsApp | `obrigado/index.html`: variável `WHATSAPP` **e** o `href` do botão `#wa` |
| Telefone (link e texto) | `index.html`: botão `tel:` na seção CTA, no rodapé e no JSON-LD (`telephone`) |
| ID do Google Tag Manager | `GTM-XXXXXXX` aparece 2x em cada arquivo (no `<head>` e no `<noscript>`) |
| Fotos do carrossel | `img/foto-1.webp` (Dr. Lucas), `foto-2.webp` (Dr. Murilo), `foto-3.webp` (sem legenda): 896×1194, WebP, até ~80 KB cada |
| OAB e credenciais | seção "Credenciais" e legenda da foto |
| Título/descrição da página | `<title>` e `<meta name="description">`: use as palavras-chave do anúncio |
| Depoimentos | seção escondida com `hidden`: só publique com depoimentos reais e autorizados |

Enquanto o ID for `GTM-XXXXXXX`, o GTM não carrega (de propósito, pra não dar erro).

## 2. Como funciona a conversão

Todos os botões de WhatsApp levam para `/obrigado/?origem=...&area=...`.
A página `/obrigado`:
1. carrega o GTM e envia o evento `lead_whatsapp` (com `origem` e `area`);
2. depois de 1,2 s abre o WhatsApp com a mensagem pronta (por área, quando vem de um card).

No **Google Tag Manager**:
- Tag **Google Ads – Conversão**, com o gatilho *Visualização de página* e a condição `Page Path` contém `/obrigado`.
- Opcional: tag de conversão para o clique no telefone, com o gatilho *Evento personalizado* = `phone_click`.
- Também são enviados os eventos `whatsapp_click` (no clique, na página principal) e `lead_whatsapp` (na `/obrigado`), caso você prefira usar evento.

## 3. Publicar (grátis)

**Cloudflare Workers (em uso):** o `wrangler.jsonc` publica a pasta `public/`. Na Cloudflare: Build command vazio, Deploy command `npx wrangler deploy`.


**Vercel:** em vercel.com, escolha *Add New → Project*, importe o repositório do GitHub (ou arraste a pasta) e clique em Deploy. Não precisa de configuração.
**Netlify:** em app.netlify.com/drop, arraste a pasta inteira.
**Cloudflare Pages:** em *Workers & Pages → Create → Pages → Upload assets*.

Depois, aponte o domínio do Registro.br (DNS) para o provedor escolhido.

## 4. Testar
- PageSpeed: https://pagespeed.web.dev (teste a URL publicada, aba *Celular*).
- Clique nos botões e confira se a `/obrigado` abre o WhatsApp com o número certo.
- No GTM, use o modo *Visualizar* para ver os eventos disparando.
