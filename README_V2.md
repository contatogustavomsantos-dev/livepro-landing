# VendaPRO Landing Page — V2

## O que mudou na V2

- **VSL Gate**: toda a página fica bloqueada até o vídeo atingir `pitchTimeSeconds`. Após o desbloqueio, nav e seções pós-pitch aparecem.
- **VENDAPRO_CONFIG**: objeto JS centralizado com todas as variáveis configuráveis.
- **Copy reescrita**: nova tese, novas seções, CTAs unificados em "Solicitar diagnóstico estratégico".
- **14 seções** pós-pitch com IDs semânticos.
- **15 eventos de rastreamento** via `trackEvent()` compatível com GA4, Meta Pixel e TikTok Pixel.
- **SEO completo**: title/description, canonical, OG, Twitter Card, Schema Organization + ProfessionalService (VideoObject comentado).
- **Acessibilidade**: aria-hidden, aria-live, focus-visible, reduced-motion.
- **QA params**: `?preview=after` e `?reset=1`.

---

## Configuração rápida

Abra `index.html` e localize o objeto `VENDAPRO_CONFIG` no `<script>` principal:

```javascript
var VENDAPRO_CONFIG = {
  pitchTimeSeconds:  1200,                       // tempo (segundos) para desbloquear página
  storageKey:        'vendapro_v2_pitch_unlocked_v1',
  showPricing:       false,                      // true → exibe seção #pricing-block
  videoProvider:     'native',
  videoSrc:          'SUBSTITUIR_URL_DO_VIDEO',  // ← substituir aqui
  quizUrl:           'quiz.html'
};
```

### Substituir o vídeo

Altere `videoSrc` pela URL direta do arquivo de vídeo (mp4/webm hospedado em CDN ou storage):

```javascript
videoSrc: 'https://cdn.exemplo.com/vendapro-pitch.mp4',
```

Enquanto `videoSrc === 'SUBSTITUIR_URL_DO_VIDEO'`, o player fica oculto e o placeholder é exibido.

### Ativar seção de preços

```javascript
showPricing: true,
```

### Ajustar tempo de desbloqueio

```javascript
pitchTimeSeconds: 900,  // 15 min
```

---

## Parâmetros QA (URL)

| Parâmetro | Efeito |
|---|---|
| `?preview=after` | Ignora o gate e exibe toda a página (sem precisar assistir o vídeo) |
| `?reset=1` | Limpa o sessionStorage e força o gate a aparecer novamente |

Exemplos:
- `https://vendapro.com.br/?preview=after` — visualização pós-pitch
- `https://vendapro.com.br/?reset=1` — resetar estado da sessão

---

## Eventos de rastreamento

| Evento | Quando dispara |
|---|---|
| `page_view` | Carregamento da página |
| `video_start` | Usuário dá play no vídeo |
| `video_25` | Vídeo atingiu 25% |
| `video_50` | Vídeo atingiu 50% |
| `video_75` | Vídeo atingiu 75% |
| `video_complete` | Vídeo chegou ao fim |
| `pitch_reached` | Tempo `pitchTimeSeconds` atingido → desbloqueio |
| `landing_unlocked` | Página desbloqueada (pós-pitch visível) |
| `cta_hero_click` | Click em CTA no gate ou nav |
| `cta_final_click` | Click no CTA da seção final #diagnostico |
| `cta_sticky_click` | Click no sticky CTA mobile |
| `faq_open` | Usuário abriu um item do FAQ (parâmetro: `question`) |
| `scroll_50` | Usuário rolou 50% da página |
| `scroll_90` | Usuário rolou 90% da página |

---

## TODOs pendentes

- [ ] **Substituir `videoSrc`** pela URL real do vídeo
- [ ] **Criar `og-image.jpg`** (1200×630 px) e hospedar na raiz
- [ ] **Confirmar URL canônica** (`<link rel="canonical">`)
- [ ] **Foto de Gustavo Marques** — substituir placeholder na seção #gestao
- [ ] **Arquivo de legendas** — substituir `SUBSTITUIR_ARQUIVO_LEGENDAS.vtt` (acessibilidade)
- [ ] **Desbloquear VideoObject Schema** — preencher e descomentar no `<head>`
- [ ] **Revalidar dados TikTok Shop** — seção #tiktok-shop tem nota de alerta
- [ ] **Conectar GA4 / Meta Pixel / TikTok Pixel** — `trackEvent()` já está configurado, basta adicionar os scripts de rastreamento antes do `</head>`

---

## Estrutura das seções pós-pitch

```
#tese         — A tese da VendaPRO
#problema     — 6 pain cards
#operacao     — 10 itens do escopo operacional
#metodo       — 5 etapas + bloco de suporte
#escopo       — Responsabilidades VendaPRO vs. empresa
#qualificacao — 4 condições mínimas
#resultados   — Stat bar + 3 cases de prova
#canais       — 3 canais (Shopee, TikTok Shop, ML)
#tiktok-shop  — Dados TikTok Shop Brasil (3 stats)
#gestao       — Gustavo Marques
#quem         — Para quem é / não é
#faq          — 7 perguntas frequentes
#pricing-block— Preços (hidden por padrão)
#diagnostico  — CTA final
```

---

## Deploy

Push para `master` aciona o Vercel auto-deploy via GitHub integration.

```bash
git add index.html README_V2.md
git commit -m "feat: VendaPRO V2 — VSL gate, 14 sections, full SEO"
git push origin master
```
