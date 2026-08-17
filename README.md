# Chronus Solutions — Landing Page

Site institucional público da Chronus Solutions (chronussolutions.com.br).
Este é o "cartão de visita" da empresa, guarda-chuva dos produtos — **não é
o software** (cada produto é um app à parte, em outro repositório, atrás de
login, hospedado no próprio subdomínio). Aqui não há nenhuma tela real de
produto: o mockup no hero é ilustrativo, feito em HTML/CSS.

A empresa tem hoje dois produtos, cada um com seção própria na página
(`#erp` e `#lex`) e link de acesso ao subdomínio real:

- **Chronus ERP** — gestão de clínicas e consultórios —
  `erp.chronussolutions.com.br`
- **Chronus Lex** — gestão de escritórios de advocacia —
  `lex.chronussolutions.com.br`

A identidade visual da página institucional é neutra e própria (fundo
escuro + azul), e **não** copia a identidade de nenhum produto — em
particular, não usa o azul-marinho + dourado do Chronus Lex. Isso é
proposital: a home precisa funcionar como teto para produtos com marcas
visuais diferentes.

## Stack e por que essa escolha

HTML + CSS + JavaScript puro, sem build step (sem Next.js, sem Astro, sem
bundler nenhum).

- **É uma página só, sem estado, sem rotas dinâmicas.** Não existe
  justificativa de complexidade para trazer um framework de app aqui — o
  agendamento público por clínica (`/agendar/nome-da-clinica`) é uma feature
  do sistema logado, não desta landing page.
- **Performance máxima de graça.** Sem JS de framework para hidratar, sem
  bundle a baixar: Lighthouse tende a 100 em Performance com o mínimo de
  esforço, o que importa direto para SEO.
- **Hospedagem trivial.** Roda em qualquer lugar: Vercel/Netlify/Cloudflare
  Pages (grátis, com HTTPS automático) ou até hospedagem compartilhada
  tradicional (cPanel), comum em registradores de domínio `.com.br`. Não
  depende de Node.js no servidor.
- **Zero manutenção de dependências.** Sem `package.json`, sem
  vulnerabilidade de dependência para atualizar daqui a um ano.

Se no futuro a página crescer (blog, múltiplos idiomas, formulário com
backend), migrar para Astro é o caminho natural — a estrutura de
seções/conteúdo deste HTML se aproveita quase 1:1.

## Estrutura

```
index.html        # todo o conteúdo e marcação semântica da página
css/style.css      # estilos (design tokens em :root, paleta escura + azul)
js/main.js         # menu mobile + ano dinâmico no rodapé (única lógica JS)
icon.png           # única imagem de marca: favicon, logo do header/rodapé e imagem de Open Graph
robots.txt
sitemap.xml
```

## Rodando localmente

Não precisa de instalação. Duas opções:

1. Abrir `index.html` direto no navegador.
2. Servir localmente (recomendado, evita diferenças de `file://`):
   ```
   npx serve .
   ```

## Deploy

Qualquer host de arquivo estático funciona. Sugestão mais simples:

1. **Vercel ou Netlify**: conectar este diretório (ou repositório Git) e
   apontar o domínio `chronussolutions.com.br` nas configurações de DNS do
   registrador — ambos emitem HTTPS automaticamente.
2. Depois do deploy, atualizar `sitemap.xml` e submeter no Google Search
   Console.

## Pendências antes de publicar

- **Add-on "Lembrete de agendamento por WhatsApp" (R$49/mês, seção
  `#planos`)**: publicado no ar por decisão sua, mesmo o recurso ainda
  estando em fase final de implementação no Chronus ERP de verdade — a
  justificativa foi que o link do site ainda não foi compartilhado com
  ninguém de fora. **Antes de divulgar o link publicamente, confirme que o
  envio automático de lembrete já está funcionando em produção.** O preço
  (R$49/mês, até 300 lembretes, excedente R$0,25/mensagem) é estimativa
  baseada no custo de mensagem Utility da Meta Cloud API (~R$0,06–0,09/msg
  em 2026) — ajuste depois de fechar com um provedor (Meta direta, Twilio,
  Zenvia etc.), já que cada um tem markup e mensalidade de plataforma
  diferentes.
- **Preços (seção `#planos`)**: os valores mostrados (R$129/R$249 no ERP,
  R$99/R$199 no Lex, mais os tiers "sob consulta") são uma **estimativa de
  mercado**, montada comparando com concorrentes diretos (iClinic, Feegow,
  ADVBOX, Astrea) — não é preço validado com cliente real. A composição de
  cada tier já reflete os módulos reais dos dois sistemas (Essencial/
  Autônomo = básico, Profissional/Escritório = módulos completos). Ajuste
  números ou módulos direto nos blocos `.price-tier` dentro de `index.html`
  (seção `id="planos"`) antes de considerar definitivo.
- **Custo variável da Consulta Processual (Lex)**: esse módulo depende de
  provedor pago (Escavador/Judit.io), cobrado por consulta. Hoje está
  incluído sem limite no plano Escritório (R$199/mês) — antes de publicar,
  vale confirmar o custo por consulta desses provedores e, se for alto,
  colocar um limite mensal de consultas na copy (ex: "até N consultas/mês,
  excedente à parte") pra não vender algo que dá prejuízo por assinante.
- **Subdomínios dos produtos**: os botões "Acessar sistema" / "Conhecer o
  Chronus ERP" / "Testar o Chronus Lex" / "Assinar pelo WhatsApp" apontam
  para `https://erp.chronussolutions.com.br/` e
  `https://lex.chronussolutions.com.br/`. Esses links só vão funcionar
  quando o DNS/deploy de cada subdomínio estiver configurado.
- **Mockups do produto são ilustrações, não screenshots reais**: os painéis
  visuais no hero e nas seções `#erp`/`#lex` (odontograma, painel de
  prazos) são compostos em HTML/CSS pra dar uma ideia da interface — não
  são capturas de tela do sistema de verdade, e cada um tem uma legenda
  dizendo isso. Se você me mandar screenshots reais do Chronus ERP e do
  Chronus Lex, eu troco os mockups pelas imagens reais (fica mais
  convincente para quem é cético com tecnologia).
- **Prova social**: a seção de depoimentos segue **desativada** — agora
  como um bloco de HTML comentado dentro de `index.html` (procure por
  `PROVA SOCIAL`), já com CSS pronto e placeholders entre `[colchetes]`.
  Quando tiver depoimentos reais, é só substituir os placeholders e tirar
  do comentário. Nenhum depoimento, estatística ou "número de clientes" foi
  inventado.
- **Imagem de Open Graph**: por ora usando `icon.png` (o mesmo ícone do
  favicon/logo) como preview ao compartilhar o link em WhatsApp/redes
  sociais. Funciona, mas é quadrado e não tem a proporção ideal de OG
  (1200×630) — vale gerar uma arte dedicada nesse formato quando sobrar
  tempo.
- **Contato**: WhatsApp `(35) 99862-6931` e e-mail
  `contato@chronussolutions.com.br` (confirmado com você — o texto original
  tinha um typo faltando o "h" do domínio).
- Ainda não recebi a descrição do **Chronus ERP** como um prompt formal
  separado (você chegou a citar que mandaria); o conteúdo da seção `#erp`
  hoje é o mesmo que você descreveu no primeiro pedido (sistema de clínicas)
  — se o ERP tiver funcionalidades além dessas, me passe que eu atualizo
  só essa seção.
