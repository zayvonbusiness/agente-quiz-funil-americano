# Agente de Quiz (Funil Americano)

Um agente para o Claude que cria o seu quiz de qualificação de leads. O quiz filtra sozinho, sem IA no atendimento, e só quem tem o perfil chega ao seu WhatsApp. Quem não tem o perfil é barrado e não vira lead.

## Como usar

1. Crie um Projeto no Claude (claude.ai) ou abra uma pasta no Claude Code.
2. Coloque os dois arquivos abaixo no projeto:
   - `agente-quiz-funil-americano.md`: as instruções do agente.
   - `quiz-modelo.html`: o modelo que o agente adapta.
3. Peça: "Crie um quiz de funil americano para o meu negócio".
4. Responda a entrevista, aprove a tabela de perguntas e receba o quiz pronto.

Antes de publicar o seu quiz, troque `WHATSAPP` e `WEBHOOK` no topo do `quiz-modelo.html`.
