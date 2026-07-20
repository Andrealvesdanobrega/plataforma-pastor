# Estado Atual

**Fase:** Sprint 7 — Modelo de Comunicação Assistida concluída

**Atualizado em:** 20 de julho de 2026

## Resumo executivo

A filosofia do produto está consolidada: a plataforma é um **Sistema Operacional de Comunicação Assistida**, não um publicador de redes sociais. Sua missão é transformar uma intenção humana em comunicação digital organizada.

O Tutor passa a ser explicitamente o núcleo da plataforma. A arquitetura funcional agora segue **Intenção → Conteúdo → Versão para Destino → Presença Digital → Destino de Publicação → Publicação e Resultado**. Instagram, Facebook, YouTube, WhatsApp, Telegram e futuros serviços são somente possíveis provedores de Destinos; nenhum foi escolhido nesta Sprint.

A baseline técnica ENG-01 a ENG-15 permanece inalterada. O Tutor da primeira versão segue determinístico e sem IA generativa. A estratégia assíncrona, confirmação explícita, uma Publicação por destino, idempotência, reconciliação, segurança e ambientes aprovados continuam vigentes.

## Princípios vigentes

- reduzir decisões técnicas;
- explicar recomendações;
- preservar a Intenção original;
- adaptar automaticamente a comunicação quando seguro;
- manter o Usuário no controle final;
- ensinar no contexto da tarefa, sem parecer curso;
- ser simples para iniciantes e poderosa para Usuários avançados;
- tratar conhecimento absorvido com finalidade, controle e minimização;
- manter a complexidade tecnológica no sistema, nunca no Usuário.

## Conceitos consolidados

| Conceito | Estado atual |
|---|---|
| Intenção | Origem humana rastreável de todo Conteúdo |
| Tutor | Núcleo que compreende, pergunta o mínimo, conduz, explica e ensina |
| Conteúdo | Fonte editorial organizada e independente de canal |
| Conhecimento do Usuário | Contexto autorizado, corrigível e minimizado |
| Presença Digital | Conjunto dos ambientes digitais conectados ao Projeto |
| Destino de Publicação | Ponto concreto que recebe uma versão; provedor é detalhe de integração |
| Versão para Destino | Adaptação rastreável que preserva a fonte e exige revisão |

## Primeiro fluxo vigente

```text
Entrar
→ expressar a Intenção de comunicar um aviso
→ Tutor recuperar contexto e perguntar somente o necessário
→ confirmar entendimento
→ criar Conteúdo textual
→ receber recomendação determinística explicada
→ aceitar, ajustar ou rejeitar
→ gerar versão para Destino simulado
→ comparar adaptação e motivo
→ confirmar preservação da Intenção
→ guardar e compreender o próximo passo
```

O fluxo simulado pode orientar a implementação do núcleo. Publicação real permanece bloqueada até a escolha e a prova de um Destino e o atendimento integral a ENG-15.

## Decisões revistas

- A escolha antecipada de Telegram foi superada pela nova filosofia de produto.
- A evidência documental anterior permanece histórica, mas não autoriza POC ou adaptador.
- O primeiro recorte de Usuário e o aviso textual continuam hipóteses de validação, agora desacopladas de canal.
- Nenhuma decisão técnica aprovada foi alterada.

## Situação da documentação

Os nove documentos autorizados refletem a filosofia e o novo recorte. `DATA-MODEL.md`, `TECHNICAL-ARCHITECTURE.md`, `SECURITY.md`, `OPEN-DECISIONS.md` e `BACKLOG.md` foram apenas lidos e permanecem sem alteração por restrição desta Sprint. Por isso, ainda contêm vocabulário e pendências centrados em Canal que precisarão de sincronização em uma Sprint autorizada, sem mudar ENG-01 a ENG-15.

## Evidências ainda necessárias

- entrevistas sobre dor, linguagem e valor do modelo assistido;
- teste do comportamento do Tutor e da quantidade de perguntas;
- teste de compreensão de Presença Digital e Destino;
- avaliação de quais conhecimentos podem ser reutilizados com confiança;
- teste de preservação da Intenção nas adaptações determinísticas;
- critérios e POC para escolher posteriormente um único Destino real.

## Riscos atuais

| Risco | Impacto | Tratamento |
|---|---|---|
| Categoria parecer abstrata | Usuário não compreender o valor | Testar linguagem com tarefas reais |
| Tutor perguntar demais | Jornada vira formulário ou curso | Medir necessidade e reutilizar contexto válido |
| Conhecimento incorreto ou invasivo | Recomendações ruins e perda de confiança | Origem, confirmação, correção, exclusão e retenção |
| Adaptação alterar a mensagem | Perda da autoria e da Intenção | Comparação, explicação e confirmação |
| Simulador ocultar limites externos | Falsa prontidão | POC obrigatória antes do adaptador real |
| Documentos não autorizados permanecerem divergentes | Implementação ambígua | Sincronizar antes da modelagem física |
| Primeiro recorte não ter dor suficiente | Baixa adoção | Entrevistas comparativas |

## Critério para avançar

**Núcleo com simulador:** pode avançar após detalhar contratos funcionais, dados mínimos e controles de privacidade do conhecimento.

**Destino real:** somente depois de evidência de uso, documentação oficial, POC completa e decisão aprovada.

**Piloto real:** somente após todos os critérios cumulativos de ENG-15 e do MVP, sem perda, duplicidade, ação não confirmada ou risco crítico sem tratamento.
