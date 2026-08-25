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
