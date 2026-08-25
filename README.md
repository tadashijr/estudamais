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

## Modelos gratuitos

A caixa **"Mostrar apenas modelos gratuitos"** vem marcada. O filtro considera gratuito o modelo cujo id termina em `:free` ou cujo preço de entrada e saída é zero na resposta da API — ou seja, é lido da própria OpenRouter, não de uma lista fixa que envelhece.

Desmarque para ver o catálogo inteiro. Modelos gratuitos costumam ter limites de requisições por minuto mais apertados; se um quiz falhar por rate limit, troque de modelo ou aguarde.

## Fotos do material

Além de colar texto, dá para anexar imagens — foto de página de livro, quadro, caderno, slide. Clique na área tracejada, arraste os arquivos ou cole com Ctrl+V.

Ao clicar em **Extrair texto das imagens**, cada foto é enviada a um modelo com visão, que transcreve o conteúdo; o texto é acrescentado à caixa de estudo para você revisar antes de gerar o resumo e o quiz.

- Modelos com visão aparecem no seletor com 🖼
- Se o modelo escolhido não lê imagens, a aplicação usa automaticamente um modelo com visão da lista (respeitando o filtro de gratuitos) só para essa etapa; o resumo e o quiz continuam no modelo que você selecionou
- As imagens são reduzidas para no máximo 1600px antes do envio, para economizar tokens
- Limite de 8 imagens por vez

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
