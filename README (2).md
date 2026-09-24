# 🚇 MetrôBot SP 2.0

Assistente de rotas do metrô de São Paulo para as **Linhas 1-Azul, 2-Verde e 3-Vermelha**. Você escreve o pedido em português ("Estou na Sé e vou ao jogo do Palmeiras, às 7 da noite") e ele devolve a melhor rota, com paradas, baldeações, tempo estimado e a situação do metrô no horário.

> **Regra de ouro:** *o LLM conversa, o algoritmo decide.* O LLM só interpreta o pedido e narra a resposta. A rota e as regras são calculadas por código.

**Autor:** Giovanni Pinheiro Bonifatto — RA1635819

## Como rodar

1. **Colab:** *Ambiente de execução → Executar tudo*. **VS Code:** *Run All* (extensões Python + Jupyter).
2. **Com LLM (opcional):** guarde a chave do Groq em *Secrets* do Colab (`GROQ_API_KEY`) ou em um `.env`. Nunca cole a chave no código.
3. **Sem chave ou sem internet:** na célula 2, use `PROVEDOR = "offline"`. Tudo funciona, só o intérprete e o narrador usam um plano B sem LLM.

Depois, rode a última célula para abrir o painel: escreva o pedido, clique em **Interpretar pedido**, confira os campos e clique em **Buscar rota**.

## O que ele faz

- **Interpreta** texto livre (origem, destino, acessibilidade e horário), com apelidos como "jogo do Palmeiras" → Nubank Parque → estação Palmeiras-Barra Funda.
- **Deduz fatos** por lógica de primeira ordem (regras R1–R7): origem, destino, estações bloqueadas, integrações (Sé, Paraíso e Ana Rosa), alertas de acessibilidade e situação do metrô.
- **Busca a rota** com BFS (menor número de paradas) ou DFS, conta baldeações e respeita estações fechadas e linhas paralisadas.
- **Valida o horário primeiro:** de 00:00 a 04:39 o metrô está fechado e nenhuma rota é mostrada. Nos demais horários, estima o movimento (pico, moderado, baixo).
- **Narra** a resposta com LLM ou sem ele, e desenha as três linhas em HTML colorido.

## Guardrails

- O intérprete só aceita estações e locais que existem, e descarta horários inválidos.
- Se o texto cita Palmeiras ou Corinthians e o LLM devolve outro lugar, o algoritmo corrige.
- O texto do narrador é descartado se citar estação fora da rota, número que não está nos dados ou omitir uma baldeação.

## Testes

A célula 15 (`rodar_testes()`) compara obtido × esperado nos 6 casos obrigatórios (rotas e estações fechadas), nos casos extras (R6 e R7) e em testes complementares (busca, intérprete, narrador, metrô fechado). Os testes usam relógio fixo e todos passam em modo offline.

## Limitações

- Só as Linhas 1, 2 e 3.
- O horário decide apenas se o metrô está aberto e a lotação estimada; a rota e o tempo não mudam ao longo do dia.
- Vale o horário de partida: o bot não calcula se a viagem passa do fechamento.
- A acessibilidade gera alertas em origem e destino, mas não desvia a rota.
- O botão **Buscar rota** usa os campos do painel, não o texto: clique antes em **Interpretar pedido**.

## Uso de IA

Usei o Claude para revisar o MetrôBot: encontrar e corrigir erros (como o horário em UTC no Colab e o horário do painel que não chegava ao planejador) e criar os apelidos de locais.
