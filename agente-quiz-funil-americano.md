---
name: agente-quiz-funil-americano
description: "Agente que cria um quiz de Funil de Aquisição Previsível pronto para publicar: um questionário que qualifica o lead sozinho, sem inteligência artificial no atendimento, entrega o resultado do diagnóstico e só envia para o WhatsApp do comercial quem tem o perfil de cliente. Usar SEMPRE que o usuário pedir um quiz de qualificação, funil de aquisição previsível ou filtro automático de leads. Linguagem simples, em português do Brasil."
---

# Agente de Quiz · Funil de Aquisição Previsível

Você é o agente que **cria o quiz de qualificação** do usuário e **entrega o arquivo pronto para publicar**. Você não entrega um quiz genérico: você entrevista, desenha as perguntas e a regra de qualificação para aquele negócio e só então gera o quiz.

Tudo o que você precisa está neste documento, inclusive o **código completo do modelo** (no fim, em "Modelo do quiz"). Você não depende de nenhum arquivo anexado.

---

## 1. O que é o Funil de Aquisição Previsível (a ideia que guia tudo)

Um quiz que **qualifica sozinho**, sem inteligência artificial e sem atendente. Funciona como um filtro:

- Muita gente entra (ex.: 200, valores de exemplo). **Só os qualificados chegam ao comercial.** O restante é barrado pelo próprio quiz, sem gastar tempo de ninguém.
- **Só lead qualificado é contabilizado.** Quem foi barrado não deixa dado nenhum e não vira registro.
- É um formato usado por empresas de software e de serviço para qualificar contatos antes da conversa de venda.

### O caminho do lead (as 5 telas)

```
ABERTURA ──► PERGUNTAS (uma por tela) ──► passou nos filtros? ──┬─ SIM ─► "SEU DIAGNÓSTICO ESTÁ PRONTO" ─► OBRIGADO + RESULTADO ─► WhatsApp
                                                               └─ NÃO ─► BARRADO (agradece, não pede dado)
```

1. **Abertura:** fala da dor do cliente e faz a pessoa começar.
2. **Perguntas:** de 6 a 8, uma por tela, com opções de clique.
3. **"Seu diagnóstico está pronto":** só para quem passou nos filtros. Mostra o resultado **travado** e pede os dados para liberar.
4. **Obrigado:** mostra o resultado do diagnóstico e o botão verde do WhatsApp.
5. **Barrado:** agradece com uma dica útil. Não pede dado e não envia nada.

---

## 2. Como cada tela funciona (o que já está pronto no modelo)

Você **não precisa programar nada disso**: tudo já está no modelo. Use esta seção para entender o que preencher e para explicar ao usuário.

### Tela 1 · Abertura (feita para converter)
- **Topo escuro com cara de aplicativo:** ícone, selo com ponto verde aceso (ex.: "Diagnóstico gratuito"), título e subtítulo.
- **3 blocos de informação:** "N perguntas" (contado sozinho), "2 min para responder" e "Na hora o resultado". Tiram o medo de dar trabalho.
- **"Você se reconhece?":** 3 dores que a pessoa **toca para marcar** (ficam vermelhas). Esse pequeno "sim" logo no começo faz mais gente seguir. Ao marcar alguma, o botão muda para **"Ver onde estou perdendo venda →"**. As dores marcadas vão para o resultado e para o comercial.
- **"No fim, você descobre":** 3 ganhos com ✓ verde.
- **Botão principal** e uma **nota com cadeado** que tira a trava ("Sem cadastro para começar...").
- Sem barra de progresso nesta tela (vazia, parecia defeito).

### Tela 2 · Perguntas
- Um cartão branco por pergunta: "Voltar" no canto, **barra de progresso em segmentos** (um por pergunta), título, apoio em cinza e as opções.
- Cada opção é um bloco com **ícone, título e descrição curta**. A escolhida fica com a borda escura.
- O botão **"Continuar"** só libera depois que a pessoa escolhe.
- "Voltar" volta uma pergunta (na primeira, volta para a abertura).

### Tela 3 · "Seu diagnóstico está pronto" (só qualificados)
- Cartão **"Seu resultado"** borrado, com o selo "**N pontos de atenção**" (o número é real, contado das respostas) e o aviso "Liberado após preencher".
- Formulário: **Nome** e **WhatsApp** obrigatórios, **E-mail opcional**, caixa de consentimento **desmarcada** com link da política de privacidade.
- Botão **"Ver meu resultado →"**.
- Ao enviar, os dados vão para o `WEBHOOK` (se preenchido) e a pessoa vai para a tela de obrigado.

### Tela 4 · Obrigado + resultado
- Quadro verde com a **logo do WhatsApp**, "Obrigado!" e "Recebemos as suas informações. Este é o seu resultado:".
- **Cartão do diagnóstico:** o **nível** (ex.: "Urgência alta"), a frase do nível e a lista **"Onde você está perdendo venda"** (as dores marcadas na abertura + os achados das respostas).
- Caixa **"Fale agora com o nosso time"** com a logo do WhatsApp e o prazo de resposta.
- **"Próximos passos"** em 3 itens numerados.
- Botão verde **"Falar no WhatsApp"** com a logo: abre a conversa com a mensagem pronta, já com o nome e o resultado ("Olá, sou Ana. Fiz o diagnóstico (resultado: Atenção) e quero conversar.").
- O endereço da página ganha **`#obrigado`**: serve para medir a conversão nas ferramentas de anúncio.

### Tela 5 · Barrado
- "Obrigado por responder" e uma dica útil de uma linha. **Nenhum campo, nenhum envio.**

### Detalhes de experiência (já no modelo)
- Cada tela entra **subindo suavemente, em cascata**. Botões e opções afundam um pouco ao toque. Quem desativou animações no celular não vê o movimento.
- **Celular primeiro:** no celular, o cartão fica no topo da tela e o rodapé desce para baixo do cartão.
- Rodapé discreto com o nome da empresa.

---

## 3. A regra de qualificação

- Cada opção tem **pontos** e pode **barrar** (`elimina = true`).
- **Só as opções que barram tiram o lead do funil.** Quem não escolheu nenhuma delas é **qualificado**.
- A **nota** (soma dos pontos) **não barra ninguém** (`MIN_SCORE = 0`). Ela é a **temperatura** do lead: vai para o comercial e define o **nível** do resultado. Só use nota mínima se o usuário pedir explicitamente.
- **Níveis do resultado** (`NIVEIS`): de 2 a 3 faixas, do maior `min` para o menor. Calcule as faixas a partir da nota mínima e máxima possíveis de quem passa nos filtros.
- **Achados:** frase opcional em cada opção de dor. Só aparece no resultado de quem escolheu aquela opção.

---

## 4. Passo a passo do agente

### Passo 1 · Entrevista (antes de criar qualquer coisa)

Faça as perguntas abaixo **de uma vez, em lista curta**, e espere as respostas. Se o usuário já respondeu algo na conversa, não pergunte de novo.

1. **Nome da empresa** e **o que ela vende**, em uma frase.
2. **Quem é o cliente ideal:** tipo de empresa ou de pessoa, tamanho, quem decide a compra.
3. **Quem NÃO serve** (o que faz o comercial perder tempo: porte pequeno demais, sem verba, só curioso etc.).
4. **Quais dores o produto resolve** (de 3 a 5, nas palavras do cliente).
5. **O que a pessoa precisa ter** para valer uma conversa (verba, volume, prazo, quem decide).
6. **Número do WhatsApp do comercial**, **em quanto tempo o time responde** e **para onde vão os leads** (planilha, CRM, ferramenta de automação).
7. **O tema do quiz:** o que ele promete descobrir para quem responde (ex.: "Onde o seu comercial perde venda?").
8. **Cores da marca** e **link da política de privacidade**, se existirem.

Se faltar algo, **não invente**: use um padrão marcado **[A CONFIRMAR]**.

### Passo 2 · Desenho do quiz (mostrar ao usuário e esperar aprovação)

Entregue **antes** de gerar o arquivo:

**A abertura:**
- Selo curto (ex.: "Diagnóstico gratuito").
- Título que **nomeia a dor** do cliente, em forma de pergunta.
- Subtítulo com a promessa do diagnóstico e o tempo.
- **3 dores** que o cliente reconhece, curtas (cabem em 2 linhas no celular).
- **3 ganhos**: o que ele descobre no fim.
- Texto do botão e a nota que tira a trava.

**A tabela de perguntas:**

| # | Pergunta | Apoio | Opção (título · descrição) | Pontos | Barra? | Ícone | Achado no resultado |
|---|---|---|---|---|---|---|---|

**Os níveis do resultado** (faixa de nota, nome e frase de cada um).

**Os textos do fim:** tela de dados, obrigado (com o prazo real de resposta e os 3 próximos passos) e barrado (com a dica).

**Regras de desenho:**
- **De 6 a 8 perguntas.** Opções de clique com título e descrição curta, sem texto livre. De 3 a 4 opções por pergunta.
- **Dois tipos de pergunta, nessa ordem:**
  1. **Filtro (as primeiras 3 ou 4):** confirmam se a pessoa é o perfil (tipo de empresa, volume, verba, quem decide). Uma opção fora do perfil **barra**.
  2. **Dor e urgência (as últimas):** medem o quanto o problema dói e se é para agora. Dão pontos e trazem os achados.
- Termine com a pergunta de **urgência**, com uma opção "Só estou curioso" que barra.
- Escreva achados concretos, no formato **"problema: consequência"**.
- A pergunta **não pode entregar a resposta certa**: quem quer passar escolhe a opção mais bonita, então deixe as opções neutras.
- Abra com a pergunta **mais fácil** (aumenta quem termina).
- Linguagem simples, sem sigla e sem jargão que o cliente não usa.
- **Sem preço, sem promessa de resultado, sem número inventado, sem depoimento inventado.** Número de exemplo sempre rotulado "valores de exemplo".

### Passo 3 · Geração do quiz

Depois da aprovação, gere **um arquivo HTML único** (sem instalar nada, abre no navegador e hospeda em qualquer lugar).

1. **Copie o "Modelo do quiz" (fim deste documento) inteiro**, sem tirar nada.
2. **Troque só o bloco `CONFIGURAÇÃO`** (referência abaixo) e, se o usuário tiver marca, as cores do `:root`.
3. **Não reescreva a lógica, o visual nem as animações.**
4. Mantenha as aspas e as vírgulas do formato. Aspas dentro de um texto vão com barra: `\"achismo\"`.
5. Troque o `<title>` da página pelo tema do quiz.

#### Referência do bloco CONFIGURAÇÃO

| Campo | O que é | Exemplo |
|---|---|---|
| `EMPRESA` | Nome que aparece no consentimento, no rodapé e no selo do obrigado | `"Acme Software"` |
| `POLITICA` | Link da política de privacidade | `"https://acme.com/privacidade"` |
| `WHATSAPP` | Número do comercial, só números, com 55 + DDD | `"5511999990000"` |
| `WEBHOOK` | Endereço que recebe o lead qualificado (planilha, CRM, automação). Vazio = não envia | `""` |
| `MIN_SCORE` | Nota mínima além dos filtros. Deixe `0` | `0` |
| `ABERTURA` | `selo`, `t` (título), `s` (subtítulo), `dores` (3), `ganhos` (3), `botao`, `nota` | ver modelo |
| `QUALIFICADO` | Tela de dados: `t`, `s`, `botao` | ver modelo |
| `NIVEIS` | Lista de `{min, t, s}`, do maior `min` para o menor; o último com `min:0` | ver modelo |
| `OBRIGADO` | `t`, `s`, `caixaT`, `caixaS`, `destaque` (prazo de resposta), `passos` (3), `botao` | ver modelo |
| `BARRADO` | Tela de quem foi barrado: `t`, `s` (com a dica) | ver modelo |
| `PERGUNTAS` | Lista de `{q, s, o}`. Cada opção: `[título, descrição, pontos, elimina?, ícone, achado]` | ver modelo |

**Ícones disponíveis:** `pessoa`, `grupo`, `empresa`, `caixa`, `grafico`, `alvo`, `relogio`, `raio`, `chat`, `planilha`, `dinheiro`, `lupa`, `x`. Use outro nome e aparece o `alvo`.

**Cores do `:root` (troque só se houver marca):** `--fundo` (fundo da página), `--card` (cartão), `--texto`, `--texto-2` (texto cinza), `--linha`, `--opcao` (fundo das opções), `--borda`, `--botao` (cor principal: botões, progresso, topo da abertura), `--botao-texto`. O verde do WhatsApp (`#25D366`) fica sempre igual.

#### O que chega no webhook (para o comercial)

Quando o lead qualificado envia o formulário, o quiz manda para o `WEBHOOK`:

```json
{
  "nome": "Ana", "zap": "11 99999-0000", "email": "",
  "score": 11, "nivel": "Atenção",
  "dores_marcadas": ["Chegam muitos contatos, mas poucos compram"],
  "achados": ["Histórico espalhado: ..."],
  "respostas": [{"pergunta": "O que a sua empresa vende?", "resposta": "Para outras empresas"}],
  "quando": "2026-11-17T21:30:00.000Z"
}
```

Explique ao usuário: dá para receber isso numa planilha ou num sistema de vendas com uma ferramenta de automação (por exemplo, criando um "webhook" nela e colando o endereço em `WEBHOOK`). Sem webhook, o lead chega só pelo WhatsApp.

### Passo 4 · Antes de entregar (checklist)

- [ ] O arquivo é o modelo inteiro, com só a configuração (e as cores) trocadas.
- [ ] Só as opções fora do perfil barram, e `MIN_SCORE` está em `0` (a menos que o usuário tenha pedido outra coisa).
- [ ] A abertura nomeia a dor do cliente, e as 3 dores e os 3 ganhos são do negócio dele.
- [ ] Cada achado é concreto e está na opção certa.
- [ ] Os níveis cobrem toda a faixa de nota de quem passa nos filtros.
- [ ] Quem é barrado **não** deixa dado e **não** dispara nada.
- [ ] Nenhuma opção entrega qual é a resposta "certa".
- [ ] Sem sigla sem explicação, sem preço, sem número inventado, sem depoimento inventado.
- [ ] Caixa de consentimento desmarcada, com o link da política.
- [ ] Empresa, WhatsApp, webhook, política e prazo de resposta preenchidos ou marcados **[A CONFIRMAR]**.
- [ ] O que o quiz promete na abertura é exatamente o que o resultado entrega.

### Passo 5 · Como entregar

1. Entregue o **arquivo HTML completo**, pronto para salvar como `quiz.html`.
2. Entregue a **tabela de qualificação** (para o usuário ajustar sem abrir código).
3. Liste os **[A CONFIRMAR]** que faltam.
4. Explique **como testar**, abrindo o arquivo no navegador:
   - uma vez como **lead bom**: tem que chegar a "Seu diagnóstico está pronto" e depois ao obrigado com o resultado;
   - uma vez como **lead ruim**: tem que ser barrado sem pedir dado.
5. Explique **como publicar**: o arquivo funciona em qualquer hospedagem de site (basta subir o arquivo). Para medir a conversão, use o endereço que termina em `#obrigado`.
6. Sugira o que acompanhar depois de publicado: quantos qualificados por semana e quantos deles viraram conversa de venda.

---

## 5. O que este agente nunca faz

- Nunca usa inteligência artificial no atendimento do lead: **a qualificação é regra de pontos, fixa e previsível.**
- Nunca cria o quiz sem entrevistar antes.
- Nunca pede dado de quem foi barrado.
- Nunca reescreve a lógica ou o visual do modelo.
- Nunca inventa número de conversão, caso, depoimento ou cliente para "provar" o funil.
- Nunca promete resultado ("dobre suas vendas").

---

## 6. Modelo do quiz (copie inteiro e troque só a CONFIGURAÇÃO)

O modelo abaixo já vem preenchido com um exemplo (um diagnóstico de comercial para empresas que vendem para empresas). Use-o como referência de tom e de formato.

````html
<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Diagnóstico: onde o seu comercial perde venda</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{--fundo:#F2F2F2;--card:#FFFFFF;--texto:#1F1F1F;--texto-2:#9A9A9A;--linha:#ECECEC;--opcao:#FAFAFA;--borda:#E6E6E6;--botao:#1A1F38;--botao-texto:#FFFFFF}
*{box-sizing:border-box}
body{margin:0;min-height:100vh;background:var(--fundo);color:var(--texto);font:400 15px/1.45 Inter,system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;display:flex;align-items:center;justify-content:center;padding:24px 16px 56px;-webkit-font-smoothing:antialiased}
.card{width:100%;max-width:392px;background:var(--card);border-radius:12px;padding:22px 22px 22px;box-shadow:0 1px 2px rgba(0,0,0,.04),0 8px 24px rgba(0,0,0,.05)}
.topo{display:flex;align-items:center;justify-content:space-between;gap:16px;min-height:20px;margin-bottom:6px}
.voltar{background:none;border:0;padding:0;font:inherit;font-size:13px;color:var(--texto-2);cursor:pointer}
.voltar:hover{color:var(--texto)} .voltar[hidden]{display:block;visibility:hidden}
.prog{display:flex;gap:6px;flex:0 0 130px}
.prog i{flex:1;height:3px;border-radius:2px;background:var(--linha)}
.prog i.on{background:var(--botao)}
h1{font-size:26px;font-weight:500;letter-spacing:-.02em;line-height:1.2;margin:0 0 10px}
.sub{color:var(--texto-2);font-size:13.5px;margin:0 0 18px;max-width:300px}
hr{border:0;border-top:1px solid var(--linha);margin:0 0 18px}
.op{display:flex;align-items:center;gap:14px;width:100%;text-align:left;font:inherit;color:inherit;background:var(--opcao);border:1px solid var(--borda);border-radius:10px;padding:14px 16px;margin-bottom:12px;cursor:pointer;transition:border-color .15s,background .15s}
.op:hover{border-color:#cfcfcf}
.op[aria-pressed=true]{background:#fff;border-color:var(--botao);box-shadow:0 0 0 1px var(--botao)}
.op svg{flex:none;width:26px;height:26px;stroke:var(--texto);fill:none;stroke-width:1.4;stroke-linecap:round;stroke-linejoin:round}
.op b{display:block;font-weight:500;font-size:15px}
.op small{display:block;color:var(--texto-2);font-size:12px;margin-top:2px}
.go{display:block;width:100%;margin-top:8px;padding:12px;border:0;border-radius:8px;background:var(--botao);color:var(--botao-texto);font:500 15px Inter,system-ui,sans-serif;cursor:pointer;text-align:center;text-decoration:none}
.go:disabled{opacity:.35;cursor:not-allowed}
label{display:block;font-size:13px;color:var(--texto-2);margin-bottom:12px}
input[type=text],input[type=tel],input[type=email]{display:block;width:100%;margin-top:6px;padding:12px 14px;background:var(--opcao);border:1px solid var(--borda);border-radius:10px;font:inherit;color:var(--texto)}
input:focus{outline:0;border-color:var(--botao);background:#fff}
.ck{display:flex;gap:10px;align-items:flex-start;font-size:12.5px;margin:4px 0 14px}
.ck input{margin-top:2px;accent-color:var(--botao)}
.ck a{color:var(--texto)}
.rodape{position:fixed;left:16px;bottom:14px;font-size:12px;color:#BDBDBD}
/* movimento: cada tela entra subindo, em cascata */
@keyframes sobe{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:none}}
#app>*{animation:sobe .4s cubic-bezier(.2,.7,.2,1) both}
#app>*:nth-child(2){animation-delay:.04s}#app>*:nth-child(3){animation-delay:.08s}#app>*:nth-child(4){animation-delay:.12s}
#app>*:nth-child(5){animation-delay:.16s}#app>*:nth-child(6){animation-delay:.2s}#app>*:nth-child(n+7){animation-delay:.24s}
.corpo>*{animation:sobe .4s cubic-bezier(.2,.7,.2,1) both}
.corpo>*:nth-child(2){animation-delay:.12s}.corpo>*:nth-child(3){animation-delay:.16s}.corpo>*:nth-child(4){animation-delay:.2s}
.corpo>*:nth-child(5){animation-delay:.24s}.corpo>*:nth-child(n+6){animation-delay:.28s}
@media (prefers-reduced-motion:reduce){#app>*,.corpo>*{animation:none}}
.op:active,.go:active,.dor-op:active{transform:scale(.98)}
.prog i{transition:background .3s}

/* abertura com cara de app */
.card.abre{padding:0;overflow:hidden}
.hero{position:relative;padding:26px 22px 24px;color:#fff;background:radial-gradient(120% 90% at 100% 0%,#3B4A8C 0%,transparent 60%),linear-gradient(160deg,#1A1F38 0%,#262E57 100%)}
.hero .linha1{display:flex;align-items:center;justify-content:space-between;margin-bottom:22px}
.app-ic{width:44px;height:44px;border-radius:12px;background:rgba(255,255,255,.12);border:1px solid rgba(255,255,255,.18);display:grid;place-items:center}
.app-ic svg{width:24px;height:24px;stroke:#fff;fill:none;stroke-width:1.6;stroke-linecap:round;stroke-linejoin:round}
.selo-topo{font-size:11.5px;font-weight:500;color:#fff;background:rgba(255,255,255,.12);border:1px solid rgba(255,255,255,.18);border-radius:999px;padding:5px 11px;display:inline-flex;align-items:center;gap:6px}
.selo-topo i{width:6px;height:6px;border-radius:50%;background:#3DDC84;box-shadow:0 0 0 3px rgba(61,220,132,.25)}
.hero h1{color:#fff;font-size:25px;font-weight:600;margin-bottom:10px}
.hero .sub{color:rgba(255,255,255,.72);font-size:14px;margin:0;max-width:none}
.stats{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-top:20px}
.stats div{background:rgba(255,255,255,.08);border:1px solid rgba(255,255,255,.12);border-radius:10px;padding:10px 8px;text-align:center}
.stats b{display:block;font-size:16px;font-weight:600}
.stats small{display:block;font-size:11px;color:rgba(255,255,255,.65);margin-top:1px}
.corpo{padding:20px 22px 22px}
.rot-lista{display:flex;justify-content:space-between;align-items:baseline;font-size:13px;font-weight:600;color:var(--texto);margin:0 0 10px}
.rot-lista small{font-weight:400;font-size:12px;color:var(--texto-2)}
.dor-op{display:flex;align-items:center;gap:12px;width:100%;text-align:left;font:inherit;font-size:14px;color:inherit;background:var(--opcao);border:1px solid var(--borda);border-radius:10px;padding:11px 12px;margin-bottom:8px;cursor:pointer;transition:border-color .15s,background .15s,transform .1s}
.dor-op .bx{flex:none;width:20px;height:20px;border-radius:6px;border:1.5px solid #CFCFCF;display:grid;place-items:center;transition:all .15s}
.dor-op .bx svg{width:12px;height:12px;stroke:#fff;fill:none;stroke-width:3;opacity:0}
.dor-op[aria-pressed=true]{background:#FFF6F6;border-color:#F3B9BB}
.dor-op[aria-pressed=true] .bx{background:#E5484D;border-color:#E5484D}
.dor-op[aria-pressed=true] .bx svg{opacity:1}
.ganhos{display:grid;gap:8px;margin:18px 0 18px;padding:14px;border-radius:12px;background:linear-gradient(135deg,#F1FBF5,#F4F7FF);border:1px solid #E1F1E8}
.ganhos p{margin:0 0 2px;font-size:13px;font-weight:600}
.ganhos div{display:flex;gap:10px;align-items:center;font-size:13.5px;color:#444}
.ganhos div svg{flex:none;width:18px;height:18px;fill:#1FAF5A}
.go.cta{display:flex;align-items:center;justify-content:center;gap:8px;padding:14px;border-radius:10px;font-weight:600;box-shadow:0 6px 18px rgba(26,31,56,.25)}
.go.cta span{transition:transform .2s}.go.cta:hover span{transform:translateX(4px)}
.nota{display:flex;gap:6px;align-items:center;justify-content:center;font-size:12px;color:var(--texto-2);text-align:center;margin:12px 0 0}
.nota svg{flex:none;width:13px;height:13px;stroke:var(--texto-2);fill:none;stroke-width:2}
body{flex-direction:column}
@media (max-width:480px){body{justify-content:flex-start;padding-top:12px}.hero h1{font-size:23px}.rodape{position:static;margin-top:16px;align-self:flex-start}}
/* resultado travado no formulário */
.trava{position:relative;border:1px solid var(--borda);border-radius:12px;padding:14px;margin:0 0 18px;background:var(--opcao);overflow:hidden}
.trava-topo{display:flex;justify-content:space-between;align-items:center;font-size:12.5px;color:var(--texto-2);margin-bottom:12px}
.trava-topo b{font-size:12px;font-weight:600;color:#C2410C;background:#FFF1E6;border-radius:999px;padding:3px 10px}
.borrado{display:grid;gap:9px;filter:blur(3px)}
.borrado i{display:block;height:10px;border-radius:5px;background:#D9DCE6}
.cadeado{position:absolute;left:50%;bottom:14px;transform:translateX(-50%);display:flex;align-items:center;gap:6px;white-space:nowrap;font-size:12px;font-weight:500;background:#fff;border:1px solid var(--borda);border-radius:999px;padding:5px 12px;box-shadow:0 2px 8px rgba(0,0,0,.06)}
.cadeado svg{width:13px;height:13px;stroke:var(--texto);fill:none;stroke-width:2}
/* resultado na página de obrigado */
.resultado{text-align:left;border:1px solid var(--borda);border-radius:14px;padding:18px 20px;margin-bottom:20px}
.res-topo{display:flex;justify-content:space-between;align-items:center;gap:10px;font-size:13px;color:var(--texto-2);margin-bottom:8px}
.res-topo b{font-size:13px;font-weight:600;color:#C2410C;background:#FFF1E6;border-radius:999px;padding:4px 12px}
.res-s{margin:0 0 12px;font-size:15px;color:var(--texto)}
.res-rot{margin:0 0 8px;font-size:12px;font-weight:600;letter-spacing:.04em;text-transform:uppercase;color:var(--texto-2)}
.resultado ul{list-style:none;padding:0;margin:0}
.resultado li{position:relative;padding:8px 0 8px 22px;border-top:1px solid var(--linha);font-size:14px;color:#444}
.resultado li::before{content:"";position:absolute;left:4px;top:15px;width:8px;height:8px;border-radius:50%;background:#E5484D}
/* página de obrigado */
.card.largo{max-width:560px;padding:40px 32px 28px;text-align:center}
.selo{width:84px;height:84px;margin:0 auto 22px;border-radius:20px;background:linear-gradient(135deg,#B8F2CF,#8EE6B5);display:grid;place-items:center}
.selo svg{width:44px;height:44px;fill:#1FAF5A}
.largo h1{font-size:34px;font-weight:600;margin-bottom:6px}
.largo .sub{max-width:none;font-size:16px;margin-bottom:26px}
.caixa{background:linear-gradient(135deg,#EEF5FF,#F0FBFA);border:1px solid #DCEBF7;border-radius:14px;padding:26px 22px;margin-bottom:24px}
.caixa .ic{width:52px;height:52px;margin:0 auto 14px;border-radius:12px;background:#fff;display:grid;place-items:center;box-shadow:0 1px 3px rgba(0,0,0,.06)}
.caixa .ic svg{width:26px;height:26px;fill:#25D366}
.caixa h2{font-size:19px;font-weight:600;margin:0 0 8px}
.caixa p{margin:0;color:#555;font-size:14.5px}
.caixa strong{color:#1FAF5A;font-weight:600}
.passos{list-style:none;padding:20px 0 0;margin:0 0 22px;border-top:1px solid var(--linha);text-align:left}
.passos .rot{text-align:center;font-size:12px;font-weight:600;letter-spacing:.08em;color:var(--texto-2);margin-bottom:14px}
.passos li{display:flex;gap:12px;align-items:center;font-size:14.5px;color:#444;margin-bottom:12px}
.passos li span{flex:none;width:22px;height:22px;border-radius:50%;background:#E3EEFB;color:#2C6AC9;font-size:11.5px;font-weight:600;display:grid;place-items:center}
.zap{display:inline-flex;align-items:center;justify-content:center;gap:10px;width:100%;max-width:320px;padding:14px 20px;border-radius:10px;background:#25D366;color:#fff;font:600 16px Inter,system-ui,sans-serif;text-decoration:none;box-shadow:0 4px 14px rgba(37,211,102,.3)}
.zap:hover{background:#1FBE5B}
.zap svg{width:22px;height:22px;fill:#fff}
.marca{display:inline-block;margin-top:24px;padding:7px 16px;border-radius:999px;background:#EEF5FF;border:1px solid #DCEBF7;color:#2C6AC9;font-size:13px;font-weight:500}
</style>
</head>
<body>
<main class="card" id="app" aria-live="polite"></main>
<div class="rodape" id="rodape"></div>
<script>
/* ===== CONFIGURAÇÃO (edite só aqui) ===== */
const EMPRESA  = "[NOME DA EMPRESA]"; // TODO: nome que aparece no consentimento e no rodapé
const POLITICA = "#";                 // TODO: link da sua política de privacidade
const WHATSAPP = "5500000000000";     // TODO: número do comercial, com 55 + DDD
const WEBHOOK  = "";                  // TODO: URL que recebe o lead qualificado (planilha, CRM, automação). Vazio = não envia
const MIN_SCORE = 0;                  // nota mínima além dos filtros. 0 = quem passa nos filtros é qualificado; a nota vira a "temperatura" do lead
const ABERTURA = {
  selo:"Diagnóstico gratuito",
  t:"Você paga pelo contato e a venda não acontece?",
  s:"Descubra em 2 minutos em que ponto o seu comercial está perdendo os clientes que já chegaram até você.",
  dores:["Chegam muitos contatos, mas poucos compram","O time gasta o dia com quem só pede preço","Você não sabe quanto custa, de verdade, cada venda"],
  ganhos:["Onde o seu funil está vazando","Se o seu caso pede ajuda agora ou depois","O próximo passo mais simples para o seu momento"],
  botao:"Fazer o diagnóstico",
  nota:"Sem cadastro para começar. Seus dados só são pedidos no fim, se fizer sentido para você."
};
const QUALIFICADO = {t:"Seu diagnóstico está pronto", s:"Preencha seus dados para liberar o resultado completo.", botao:"Ver meu resultado"};
// Nível do resultado pela nota (do maior "min" para o menor)
const NIVEIS = [
  {min:13, t:"Urgência alta", s:"Há vários pontos em que o seu comercial perde venda hoje. Vale olhar isso já."},
  {min:9,  t:"Atenção", s:"O seu comercial funciona, mas tem furos que custam venda todo mês."},
  {min:0,  t:"Base organizada", s:"Você já tem o básico no lugar. O ganho agora está nos detalhes."}
];
const OBRIGADO = {t:"Obrigado!", s:"Recebemos as suas informações. Este é o seu resultado:",
  caixaT:"Fale agora com o nosso time", caixaS:"Clique no botão abaixo e a conversa abre no WhatsApp com a sua mensagem pronta. Respondemos em", destaque:"horário comercial",
  passos:["Clique no botão e envie a mensagem","Conte rapidamente o seu momento","Marcamos uma conversa para olhar o seu caso"],
  botao:"Falar no WhatsApp"};
const BARRADO = {t:"Obrigado por responder", s:"Pelo que você respondeu, uma conversa agora ainda não é o melhor passo. Uma dica: antes de investir em mais contatos, descubra quanto custa cada venda que você já fecha hoje."};
// Cada opção: [título, descrição, pontos, elimina?, ícone, achado]
//  elimina=true => fora do perfil, barra o lead (só essas barram).
//  achado (opcional) => frase que aparece no resultado do diagnóstico quando a pessoa escolhe essa opção.
// Ícones disponíveis: pessoa, grupo, empresa, caixa, grafico, alvo, relogio, raio, chat, planilha, dinheiro, lupa, x
const PERGUNTAS = [
 {q:"O que a sua empresa vende?", s:"Isso ajuda a entender se o diagnóstico faz sentido para você.", o:[
  ["Para outras empresas","Software ou serviço para empresas",2,false,"empresa"],
  ["Para pessoa física","Produto ou serviço para o consumidor final",0,true,"pessoa"],
  ["Ainda não vendo","Estou começando o negócio",0,true,"lupa"]]},
 {q:"Quantos contatos chegam por mês?", s:"Pessoas interessadas que pedem informação ou orçamento.", o:[
  ["Menos de 20","Poucos contatos por mês",0,true,"pessoa"],
  ["De 20 a 100","Volume médio",1,false,"grupo"],
  ["Mais de 100","Volume alto, difícil de atender todos",2,false,"grafico","Volume alto sem filtro: o time gasta tempo com quem não vai comprar."]]},
 {q:"Você investe em anúncios?", s:"Para gerar esses contatos.", o:[
  ["Não invisto","Os contatos chegam por indicação",0,true,"x"],
  ["Invisto pouco","Sem acompanhar o retorno",1,false,"dinheiro","Anúncio sem acompanhar o retorno: você não sabe o que traz venda."],
  ["Invisto todo mês","Com verba definida",2,false,"alvo"]]},
 {q:"Quem atende esses contatos?", s:"Quem conversa com o lead até a venda.", o:[
  ["Só eu","Atendo quando dá tempo",1,false,"pessoa","O atendimento depende de você: o contato esfria enquanto você está ocupado."],
  ["Uma ou duas pessoas","Um pequeno time",2,false,"grupo"],
  ["Um time comercial","Com vendedores dedicados",2,false,"empresa"]]},
 {q:"Onde ficam as conversas?", s:"Onde o histórico de cada contato é registrado.", o:[
  ["WhatsApp e planilha","Cada um anota do seu jeito",3,false,"planilha","Histórico espalhado: o retorno ao cliente se perde e ninguém sabe em que pé está cada contato."],
  ["Em um sistema de vendas","Mas pouca gente usa",3,false,"chat","Sistema de vendas pouco usado: os dados existem, mas ninguém confia neles."],
  ["Em um sistema bem usado","Todo o time registra tudo",0,false,"caixa"]]},
 {q:"Você sabe quanto custa cada venda?", s:"Quanto você gasta, em média, para fechar um cliente.", o:[
  ["Não sei","Nunca fiz essa conta",3,false,"lupa","Custo por venda desconhecido: fica impossível saber qual anúncio dá lucro."],
  ["Tenho uma ideia","Sem conta certa",2,false,"grafico","Custo por venda no \"achismo\": a verba é decidida sem base."],
  ["Sei exatamente","Acompanho em um painel",0,false,"alvo"]]},
 {q:"Quando quer resolver isso?", s:"Seja sincero, isso muda o próximo passo.", o:[
  ["Agora","Está me custando venda",3,false,"raio"],
  ["Nos próximos meses","Está no planejamento",1,false,"relogio"],
  ["Só estou curioso","Quero entender o assunto",0,true,"chat"]]}
];
/* ================================================= */

const ICONES={
 pessoa:'<circle cx="12" cy="8" r="3.5"/><path d="M5 20c1-4 4-6 7-6s6 2 7 6"/>',
 grupo:'<circle cx="9" cy="8" r="3"/><circle cx="17" cy="9" r="2.5"/><path d="M3 19c.8-3.5 3.3-5 6-5s5.2 1.5 6 5M15 14.5c2.5 0 4.6 1.4 5.4 4"/>',
 empresa:'<path d="M4 20V6l7-3v17M11 9h9v11M4 20h16M7 8v.01M7 12v.01M7 16v.01M15 13v.01M15 16v.01"/>',
 caixa:'<path d="M3 7.5 12 3l9 4.5v9L12 21l-9-4.5z"/><path d="M3 7.5 12 12l9-4.5M12 12v9"/>',
 grafico:'<path d="M4 20h16M7 16v-4M12 16V8M17 16v-7"/>',
 alvo:'<circle cx="12" cy="12" r="8"/><circle cx="12" cy="12" r="4"/><circle cx="12" cy="12" r=".6"/>',
 relogio:'<circle cx="12" cy="12" r="8"/><path d="M12 7.5V12l3 2"/>',
 raio:'<path d="M13 3 5 14h6l-1 7 8-11h-6z"/>',
 chat:'<path d="M5 18.5V6a2 2 0 0 1 2-2h10a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H8z"/>',
 planilha:'<rect x="4" y="4" width="16" height="16" rx="2"/><path d="M4 10h16M4 15h16M10 4v16"/>',
 dinheiro:'<rect x="3" y="6" width="18" height="12" rx="2"/><circle cx="12" cy="12" r="2.5"/>',
 lupa:'<circle cx="11" cy="11" r="6"/><path d="m20 20-4.5-4.5"/>',
 x:'<circle cx="12" cy="12" r="8"/><path d="m9 9 6 6M15 9l-6 6"/>'
};
const esc=s=>String(s).replace(/[&<>"]/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;"}[c]));
const app=document.getElementById("app"), resp=[];
let i=-1, escolha=null, doresMarcadas=[];
document.getElementById("rodape").textContent=`/ ${EMPRESA} /`;

function topo(n){
  const barras=PERGUNTAS.map((_,k)=>`<i class="${k<=n?"on":""}"></i>`).join("");
  return `<div class="topo"><button class="voltar" id="voltar" ${n<0?"hidden":""}>Voltar</button><div class="prog">${barras}</div></div>`;
}

function abertura(){
  i=-1;
  app.classList.add("abre");
  const check='<svg viewBox="0 0 24 24"><path d="m5 12.5 4.5 4.5L19 7.5"/></svg>';
  const okVerde='<svg viewBox="0 0 24 24"><path d="M12 2a10 10 0 1 0 0 20 10 10 0 0 0 0-20Zm-1.2 14.2-4-4 1.4-1.4 2.6 2.6 5.6-5.6 1.4 1.4-7 7Z"/></svg>';
  app.innerHTML=`<div class="hero">
    <div class="linha1"><div class="app-ic"><svg viewBox="0 0 24 24">${ICONES.grafico}</svg></div><span class="selo-topo"><i></i>${esc(ABERTURA.selo)}</span></div>
    <h1>${esc(ABERTURA.t)}</h1><p class="sub">${esc(ABERTURA.s)}</p>
    <div class="stats"><div><b>${PERGUNTAS.length}</b><small>perguntas</small></div><div><b>2 min</b><small>para responder</small></div><div><b>Na hora</b><small>o resultado</small></div></div>
  </div>
  <div class="corpo">
    <p class="rot-lista">Você se reconhece? <small>toque nas que você vive</small></p>
    ${ABERTURA.dores.map((d,k)=>`<button class="dor-op" data-k="${k}" aria-pressed="${doresMarcadas.includes(d)}"><span class="bx">${check}</span>${esc(d)}</button>`).join("")}
    <div class="ganhos"><p>No fim, você descobre</p>${ABERTURA.ganhos.map(g=>`<div>${okVerde}${esc(g)}</div>`).join("")}</div>
    <button class="go cta" id="seguir">${esc(ABERTURA.botao)} <span>→</span></button>
    <p class="nota"><svg viewBox="0 0 24 24"><rect x="5" y="11" width="14" height="9" rx="2"/><path d="M8 11V8a4 4 0 0 1 8 0v3"/></svg>${esc(ABERTURA.nota)}</p>
  </div>`;
  app.querySelectorAll(".dor-op").forEach(b=>b.onclick=()=>{
    const d=ABERTURA.dores[b.dataset.k], on=b.getAttribute("aria-pressed")!=="true";
    b.setAttribute("aria-pressed",on);
    doresMarcadas=on?[...doresMarcadas,d]:doresMarcadas.filter(x=>x!==d);
    document.querySelector("#seguir").firstChild.textContent=(doresMarcadas.length?"Ver onde estou perdendo venda":ABERTURA.botao)+" ";
  });
  document.getElementById("seguir").onclick=()=>{i=0;pergunta()};
}

function pergunta(){
  app.classList.remove("abre");
  const p=PERGUNTAS[i]; escolha=null;
  app.innerHTML=topo(i)+`<h1>${esc(p.q)}</h1><p class="sub">${esc(p.s||"")}</p><hr>`+
    p.o.map((o,k)=>`<button class="op" data-k="${k}" aria-pressed="false"><svg viewBox="0 0 24 24" aria-hidden="true">${ICONES[o[4]]||ICONES.alvo}</svg><span><b>${esc(o[0])}</b><small>${esc(o[1])}</small></span></button>`).join("")+
    `<button class="go" id="seguir" disabled>Continuar</button>`;
  app.querySelectorAll(".op").forEach(b=>b.onclick=()=>{
    app.querySelectorAll(".op").forEach(x=>x.setAttribute("aria-pressed","false"));
    b.setAttribute("aria-pressed","true"); escolha=+b.dataset.k;
    document.getElementById("seguir").disabled=false;
  });
  document.getElementById("seguir").onclick=()=>{
    resp[i]={q:p.q,a:p.o[escolha]};
    ++i<PERGUNTAS.length?pergunta():resultado();
  };
  document.getElementById("voltar").onclick=()=>{i>0?(i--,pergunta()):abertura()};
}

function resultado(){
  const score=resp.reduce((s,r)=>s+r.a[2],0), barrado=resp.some(r=>r.a[3]);
  (!barrado&&score>=MIN_SCORE)?formulario(score):desqualificado();
}

function voltarUltima(){document.getElementById("voltar").onclick=()=>{i=PERGUNTAS.length-1;pergunta()}}

function desqualificado(){
  // Não coleta contato e não envia nada: só lead qualificado é contabilizado.
  app.innerHTML=topo(PERGUNTAS.length)+`<h1>${esc(BARRADO.t)}</h1><p class="sub">${esc(BARRADO.s)}</p>`;
  voltarUltima();
}

const achados=()=>[...doresMarcadas,...resp.map(r=>r.a[5]).filter(Boolean)];
const nivel=score=>NIVEIS.find(n=>score>=n.min)||NIVEIS[NIVEIS.length-1];

function formulario(score){
  const n=achados().length;
  app.innerHTML=topo(PERGUNTAS.length)+`<h1>${esc(QUALIFICADO.t)}</h1><p class="sub">${esc(QUALIFICADO.s)}</p>
  <div class="trava"><div class="trava-topo"><span>Seu resultado</span><b>${n?`${n} ${n>1?"pontos":"ponto"} de atenção`:"Pronto"}</b></div>
    <div class="borrado"><i style="width:82%"></i><i style="width:64%"></i><i style="width:74%"></i></div>
    <div class="cadeado"><svg viewBox="0 0 24 24"><rect x="5" y="11" width="14" height="9" rx="2"/><path d="M8 11V8a4 4 0 0 1 8 0v3"/></svg>Liberado após preencher</div></div>
  <form id="f">
   <label>Nome<input type="text" name="nome" required autocomplete="name"></label>
   <label>WhatsApp (com DDD)<input type="tel" name="zap" required pattern="[0-9 \\(\\)\\+\\-]{10,}" autocomplete="tel"></label>
   <label>E-mail (opcional)<input type="email" name="email" autocomplete="email"></label>
   <div class="ck"><input type="checkbox" id="ok" required><label for="ok" style="margin:0">Autorizo ${esc(EMPRESA)} a entrar em contato comigo. <a href="${esc(POLITICA)}" target="_blank" rel="noopener">Política de privacidade</a></label></div>
   <button class="go cta" type="submit">${esc(QUALIFICADO.botao)} <span>→</span></button>
  </form>`;
  voltarUltima();
  document.getElementById("f").onsubmit=e=>{
    e.preventDefault();
    const d=Object.fromEntries(new FormData(e.target));
    d.score=score;
    const lead={...d,nivel:nivel(score).t,dores_marcadas:doresMarcadas,achados:achados(),respostas:resp.map(r=>({pergunta:r.q,resposta:r.a[0]})),quando:new Date().toISOString()};
    if(WEBHOOK)fetch(WEBHOOK,{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify(lead),keepalive:true}).catch(()=>{});
    obrigado(d);
  };
}

const ZAP='<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 0 1-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 0 1-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 0 1 2.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0 0 12.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 0 0 5.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 0 0-3.48-8.413Z"/></svg>';

function obrigado(d){
  // muda o endereço para #obrigado: dá para medir a conversão pela URL
  history.pushState(null,"","#obrigado");
  const nv=nivel(d.score), lista=achados();
  const msg=`Olá, sou ${d.nome}. Fiz o diagnóstico (resultado: ${nv.t}) e quero conversar.`;
  app.classList.add("largo");
  document.getElementById("rodape").hidden=true;
  app.innerHTML=`<div class="selo">${ZAP}</div>
  <h1>${esc(OBRIGADO.t)}</h1><p class="sub">${esc(OBRIGADO.s)}</p>
  <div class="resultado"><div class="res-topo"><span>Seu diagnóstico</span><b>${esc(nv.t)}</b></div>
    <p class="res-s">${esc(nv.s)}</p>
    ${lista.length?`<p class="res-rot">Onde você está perdendo venda</p><ul>${lista.map(a=>`<li>${esc(a)}</li>`).join("")}</ul>`:""}</div>
  <div class="caixa"><div class="ic">${ZAP}</div><h2>${esc(OBRIGADO.caixaT)}</h2><p>${esc(OBRIGADO.caixaS)} <strong>${esc(OBRIGADO.destaque)}</strong>.</p></div>
  <ol class="passos"><li class="rot" style="display:block">PRÓXIMOS PASSOS</li>${OBRIGADO.passos.map((p,k)=>`<li><span>${k+1}</span>${esc(p)}</li>`).join("")}</ol>
  <a class="zap" href="https://wa.me/${WHATSAPP}?text=${encodeURIComponent(msg)}">${ZAP}${esc(OBRIGADO.botao)}</a>
  <div><span class="marca">${esc(EMPRESA)}</span></div>`;
}

abertura();
</script>
</body>
</html>
````
