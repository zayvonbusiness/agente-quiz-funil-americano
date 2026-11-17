# Agente de Quiz (Funil Americano)

Um agente para o Claude que cria o seu quiz de qualificação de leads. O quiz filtra sozinho, sem IA no atendimento, e só quem tem o perfil chega ao seu WhatsApp. Quem não tem o perfil é barrado e não vira lead.

## Como o quiz funciona

1. **Abertura:** fala da dor do cliente, que toca nas dores que vive.
2. **Perguntas:** uma por tela. As primeiras filtram o perfil, as últimas medem a dor.
3. **"Seu diagnóstico está pronto":** quem passou nos filtros deixa nome, WhatsApp e e-mail (opcional) para liberar o resultado.
4. **Obrigado:** mostra o resultado e um botão para falar no WhatsApp com a mensagem pronta.

Quem fica fora do perfil vê um agradecimento e não deixa dado nenhum.

## Como usar

1. Crie um Projeto no Claude (claude.ai) ou abra uma pasta no Claude Code.
2. Coloque os dois arquivos no projeto:
   - `agente-quiz-funil-americano.md`: as instruções do agente.
   - `quiz-modelo.html`: o modelo que o agente adapta.
3. Peça: "Crie um quiz de funil americano para o meu negócio".
4. Responda a entrevista, aprove a abertura e a tabela de perguntas e receba o quiz pronto.

Para ver o modelo funcionando, abra o `quiz-modelo.html` no navegador. Antes de publicar o seu quiz, preencha `EMPRESA`, `POLITICA`, `WHATSAPP` e `WEBHOOK` no topo do arquivo.
