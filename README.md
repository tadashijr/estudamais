# EstudaMais

Resumo automático e quiz interativo gerados por IA. Cole um conteúdo, receba os pontos principais e um questionário; a cada erro o sistema explica e gera novas perguntas sobre o mesmo conceito, com outra abordagem, até você acertar tudo.

**Acesse:** https://SEU-USUARIO.github.io/estudamais/

## Como publicar

1. Crie um repositório **público** chamado `estudamais`
2. **Add file → Upload files** → envie `index.html` e `.nojekyll`
3. **Settings → Pages → Source: Deploy from a branch** → branch `main` (ou `master`), pasta `/ (root)` → **Save**
4. Aguarde 1–2 minutos e abra a URL indicada na própria tela de Pages

Pronto. Nada de Actions, secrets, backend ou serviços externos.

## Como usar

Na primeira vez, abra **⚙️ Configuração da IA** e cole sua chave do OpenRouter (crie em https://openrouter.ai/keys). A chave fica salva no `localStorage` do seu navegador — não vai para o repositório nem para nenhum servidor além do próprio OpenRouter. O botão **Esquecer chave** apaga.

Depois: cole o conteúdo, escolha a quantidade de perguntas e o nível, e clique em gerar.

## Limpar e recomeçar

O botão **🧹 Limpar** fica ao lado de "Gerar resumo e quiz" e zera tudo: texto, anexos, resumo e rodadas do quiz. Fica desabilitado quando não há nada para limpar e pede confirmação se um estudo já estiver em andamento.

Ao fechar um quiz com 100% de acerto, o botão **🔁 Estudar outro conteúdo** faz o mesmo.

## Saída estruturada

O quiz precisa voltar num formato exato para virar interface. A aplicação usa três níveis, escolhidos por modelo a partir do campo `supported_parameters` da própria API do OpenRouter:

| Nível | Quando | Como |
|---|---|---|
| **Schema estrito** ⚙ | modelo aceita `structured_outputs` | `response_format: json_schema` com `strict: true` — a estrutura é imposta na decodificação, o modelo não consegue devolver formato inválido |
| **Modo JSON** | modelo aceita `response_format` | `json_object` — garante JSON sintático, mas não os campos certos |
| **Prompt** | demais modelos | só a instrução no system prompt |

Se uma chamada em nível mais alto for recusada (400/404/422), cai automaticamente para o próximo. Modelos com ⚙ no seletor suportam o nível estrito; o rótulo ao lado de cada rodada do quiz mostra qual nível foi efetivamente usado.

A validação no cliente (`validateQuestions`) continua ativa nos três níveis — schema estrito garante os tipos, não que o índice de `correct` aponte para uma alternativa existente ou que a quantidade de perguntas seja a pedida.

## Modelos gratuitos

A caixa **"Mostrar apenas modelos gratuitos"** vem marcada. O filtro considera gratuito o modelo cujo id termina em `:free` ou cujo preço de entrada e saída é zero na resposta da API — ou seja, é lido da própria OpenRouter, não de uma lista fixa que envelhece.

Desmarque para ver o catálogo inteiro. Modelos gratuitos costumam ter limites de requisições por minuto mais apertados; se um quiz falhar por rate limit, troque de modelo ou aguarde.

## Anexar arquivos

Além de colar texto, dá para anexar arquivos: clique na área tracejada, arraste ou cole com Ctrl+V. Depois clique em **Extrair texto dos arquivos** — o texto de todos eles é acrescentado à caixa de estudo, com o nome do arquivo como cabeçalho.

| Formato | Como é lido | Onde acontece |
|---|---|---|
| `.pdf` com texto | pdf.js, página por página | no navegador |
| `.pdf` escaneado | páginas viram imagem e vão para OCR | modelo de visão |
| `.docx` | mammoth.js | no navegador |
| `.pptx` | JSZip lê `ppt/slides/slideN.xml`, separado por slide | no navegador |
| `.xlsx` `.xls` `.csv` | SheetJS, cada aba como CSV | no navegador |
| `.txt` `.md` | leitura direta | no navegador |
| imagens | transcrição por modelo de visão | modelo de visão |

Só imagens e PDFs escaneados consomem crédito da API — os demais são processados inteiramente no seu navegador, sem enviar o arquivo a lugar nenhum. As bibliotecas são carregadas do CDN cdnjs sob demanda, apenas quando um formato que precisa delas é anexado.

**PDF escaneado:** quando uma página tem quase nenhum texto extraível, ela é rasterizada e mandada para OCR automaticamente. Isso significa uma chamada de API por página — cuidado com PDFs longos em modelos gratuitos, que têm limite de requisições por minuto.

**Modelos de visão:** aparecem no seletor com 🖼. Se o modelo escolhido não lê imagens, a aplicação toma emprestado um modelo com visão da lista (respeitando o filtro de gratuitos) só para a transcrição; resumo e quiz continuam no modelo selecionado.

**Formatos antigos** (`.doc`, `.ppt`, binários pré-2007) não são suportados — salve como `.docx` / `.pptx`.

Limite de 12 arquivos por vez. Cada linha da lista mostra o resultado individual: quantidade de caracteres extraídos ou o motivo da falha.

Transcrição automática erra em letra cursiva, foto torta ou com pouca luz. Sempre revise o texto extraído antes de gerar o quiz — um erro na transcrição vira uma pergunta errada.

## Por que cada pessoa usa a própria chave

Um site estático não tem onde esconder um segredo: tudo que está no `index.html` é público. Se uma chave fosse embutida no arquivo, qualquer visitante a leria no código-fonte e gastaria o crédito de quem a colocou.

Se você precisa de uma chave única para todos os usuários, é necessário um backend que guarde a chave e faça as chamadas — não dá para resolver dentro do GitHub Pages.

## Estrutura

```
index.html   aplicação inteira (HTML + CSS + JS)
.nojekyll    impede o processamento Jekyll do Pages
```

## Modelos

O seletor lista todos os modelos disponíveis na sua conta OpenRouter assim que a chave é informada. Modelos com suporte a saída JSON produzem quizzes mais consistentes; se um modelo devolver perguntas malformadas, a aplicação tenta novamente com temperatura maior antes de reportar erro.
