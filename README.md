# tsp-site

Site público do **Tudo Sobre Promos** — cupons valendo agora e ofertas que saíram
nos grupos. Estático, servido pelo GitHub Pages em `tudosobrepromos.com`.

## Arquivos

| Caminho | O que é |
|---|---|
| `index.html` | a página inteira (HTML, CSS e JS num arquivo só) |
| `dados/feed.json` | ofertas de marketplace enviadas nos grupos |
| `dados/cupons.json` | cupons vigentes, com link de afiliado por loja |

## De onde vêm os dados

Ninguém edita `dados/` à mão. Quem escreve os dois arquivos é o
`feed-publico.js` do repositório **davileles/baileys-server**, que publica via
API do GitHub com debounce de 10 min e varredura de 30 min (cupom vence sozinho,
sem nenhuma oferta nova acontecer).

O repositório de destino vem da variável `GITHUB_REPO_PUBLICO` no Railway.
Endpoints de operação no baileys-server:

- `GET /publico/estado` — quantos itens, última publicação, último erro
- `POST /publico/publicar` — republica na hora, sem esperar o ciclo

## Por que um repositório separado

O GitHub Pages aceita **um domínio por repositório**. O `tudo-sobre-promos` já
usa o dele em `gestao.tudosobrepromos.com`, que é o painel interno de operação.

## A página lê de onde

`index.html` tenta `dados/` relativo primeiro e cai em
`https://gestao.tudosobrepromos.com/dados/` como alternativa. Isso mantém o site
funcionando durante a migração, enquanto o servidor ainda publica no repositório
antigo. O Pages responde com `Access-Control-Allow-Origin: *`, então a leitura
entre domínios funciona.
