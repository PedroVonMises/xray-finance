# Discovery

Goal: confirm the problem with three real investors and collect their statements as local test fixtures. Thirty minutes each. Questions are asked in Portuguese and kept verbatim here.

## Before the call

- Ask them to open the B3 Área do Investidor and export "Movimentação" for the full available period.
- Get explicit consent (script below). No consent, no file.

## Consent script

> "Vou usar o seu extrato só no meu computador, para testar o cálculo. Ele não vai para o repositório público nem para nenhum servidor. Eu removo nome, CPF e conta antes de usar. Você pode pedir para eu apagar a qualquer momento. Tudo bem?"

## Interview

Ask about past behavior, not opinions about the product.

1. "Quando foi a última vez que você quis saber se a sua carteira estava indo bem? O que você fez?"
2. "Você sabe dizer quanto a sua carteira rendeu no último ano? Como chegou nesse número?"
3. "Você já comparou com o CDI ou com outro índice? Como?"
4. "Que ferramenta você usa hoje para acompanhar? O que te irrita nela?"
5. "Você já tomou alguma decisão por causa de um número de rentabilidade? Qual?"
6. "Você subiria o seu extrato em um site? O que te faria desistir?"
7. "Você tem ativos fora da B3 ou comprados antes de 2020?"

Then show the headline sentence on paper ("Sua carteira rendeu X% ao ano. No CDI, o mesmo dinheiro teria rendido Y%.") and ask:

8. "O que você entende dessa frase? O que faria depois de ler?"

## What to record

| Field | Notes |
| --- | --- |
| Current tool and workaround | |
| Whether they know their own return | Number and how they got it |
| Reaction to the headline | Their words |
| Reason to refuse upload | |
| Asset types and movement types in the statement | Drives parser and fixture coverage |
| Statement quirks | Transfers, subscriptions, splits, ticker changes |

## Kill signals

Stop and rethink if two of the three:

- already get this number from a tool they are happy with;
- refuse to upload the statement under any condition;
- read the headline and see nothing to do with it.
