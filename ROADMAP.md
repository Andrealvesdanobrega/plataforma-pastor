# Roadmap do Produto

**Atualizado em:** 20 de julho de 2026

**Estado de engenharia:** Sprint 5 concluída; baseline técnica aprovada e implementação ainda não iniciada.

## Propósito

Este roadmap organiza resultados desejados e critérios de passagem. Ele não representa compromisso com datas antes da validação das hipóteses e dependências externas.

## Princípios de planejamento

- Aprender antes de ampliar o escopo.
- Entregar ciclos completos antes de aprofundar funções isoladas.
- Validar um canal antes de expandir para vários.
- Medir autonomia e confiança, não apenas volume de publicações.
- Manter o usuário no controle de ações externas.

## Marco de engenharia — iniciar o núcleo do MVP

**Resultado alcançado na Sprint 5:** cliente Web responsivo, linguagem, frameworks, banco, ORM, identidade, fila, armazenamento próprio, Publicação, estratégia de IA e ambientes foram aprovados. O núcleo pode ser implementado com adaptadores simulados e dados fictícios.

**Sequência de entrega:**

1. Identidade, Projeto e autorização.
2. Tutor determinístico e Orquestrador.
3. Conteúdo, versões e Biblioteca.
4. Conta Conectada e contrato do Canal.
5. Publicação, outbox, worker e reconciliação.
6. Métricas, Relatório e próximo passo simples.
7. Armazenamento conectado e mídia do formato escolhido.

**Critério de saída para piloto:** todos os critérios de primeira versão utilizável em ENG-15 de `APPROVED-DECISIONS.md`, incluindo provas reais das Integrações, privacidade, operação, segurança, acessibilidade e teste com o público, estão atendidos.

## Horizonte 1 — Descobrir

**Objetivo:** confirmar para quem o produto deve começar e qual problema merece prioridade.

**Resultados esperados:**

- segmento inicial selecionado com base em evidência;
- jornada e principais barreiras compreendidas;
- proposta de valor testada;
- canal e tipo de conteúdo candidatos definidos;
- riscos de integração conhecidos;
- protótipo do ciclo completo validado.

**Critério de saída:** usuários do segmento prioritário compreendem a proposta, concluem o protótipo com ajuda mínima e a integração escolhida é considerada viável.

## Horizonte 2 — Validar o primeiro ciclo completo

**Objetivo:** permitir que um grupo piloto vá da configuração ao acompanhamento em um canal.

**Capacidades:**

- acesso e perfil básico;
- onboarding guiado;
- conexão segura de um canal;
- criação assistida de um tipo de conteúdo;
- biblioteca e estados do conteúdo;
- revisão e publicação com confirmação;
- status e tratamento de falha;
- consulta de métricas essenciais;
- recomendação simples de próximo passo.

**Critério de saída:** o piloto completa o ciclo com segurança, a primeira publicação acontece sem suporte especializado e existe sinal de retorno para criar ou acompanhar novo conteúdo.

## Horizonte 3 — Consolidar organização e recorrência

**Objetivo:** tornar o produto útil de forma repetida, não apenas na primeira publicação.

**Capacidades candidatas:**

- calendário editorial;
- rascunhos, duplicação e histórico de versões;
- modelos por objetivo;
- lembretes e retomada de tarefas;
- comparação simples de resultados;
- recomendações baseadas no histórico.

**Critério de saída:** usuários retornam, mantêm conteúdos organizados e realizam novos ciclos com menor esforço.

## Horizonte 4 — Expandir para multicanal

**Objetivo:** reutilizar uma fonte de conteúdo com versões adequadas a mais de um canal.

**Capacidades candidatas:**

- novos conectores;
- adaptação por regras de cada canal;
- pré-visualização e aprovação por destino;
- publicação coordenada e status independente por canal;
- resultados consolidados e comparáveis.

**Critério de saída:** a expansão reduz trabalho repetitivo sem aumentar erros, dúvidas ou perda de controle.

## Horizonte 5 — Evoluir a tutoria

**Objetivo:** oferecer orientação contextual e personalizada ao longo de toda a jornada.

**Capacidades candidatas:**

- planejamento guiado por objetivo e público;
- sugestões de temas, formatos e frequência;
- assistência multimodal;
- explicação das causas prováveis dos resultados;
- colaboração simples e papéis adicionais;
- integrações com ferramentas complementares.

**Critério de saída:** recomendações são compreendidas, aceitas quando úteis e melhoram resultados definidos pelo usuário sem comprometer confiança.

## Dependências transversais

Segurança, privacidade, acessibilidade, observabilidade, suporte a erros, políticas dos canais e qualidade da linguagem devem ser tratados em todos os horizontes.

Segmento, Canal, formato, métricas, armazenamento conectado, retenção, metas operacionais e responsáveis permanecem dependências para o Horizonte 2. IA generativa, Mobile, broker dedicado, multicanal e serviços distribuídos não são dependências da primeira versão utilizável.

## Revisão do roadmap

O roadmap deve ser revisto ao fim de cada ciclo de descoberta ou entrega. Uma capacidade candidata somente entra em compromisso depois de apresentar evidência de valor, viabilidade e prioridade.
