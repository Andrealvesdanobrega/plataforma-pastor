# Decisões Abertas

**Fase:** Sprint 7 — Validação das Hipóteses do MVP

**Atualizado em:** 20 de julho de 2026

As escolhas fundamentais de engenharia permanecem aprovadas. A Sprint 7 recomendou Telegram como primeiro Canal com base em documentação oficial, mas separou viabilidade documental de adoção e comportamento real. Os itens abaixo permanecem abertos porque dependem de entrevistas, prova de conceito, governança ou medição.

## Bloqueiam o piloto, mas não o início da implementação do núcleo

| ID | Decisão adiada | Motivo do adiamento | Evidência para fechar | Trabalho permitido enquanto aberta |
|---|---|---|---|---|
| OD-01 | Confirmar pastor responsável, dor e aviso textual | A coerência documental não substitui comportamento observado | Entrevistas comparativas mostram responsável real, frequência, alternativa, falha, intensidade e valor do texto | Implementar núcleo com dados fictícios; não recrutar piloto real como se a hipótese estivesse validada |
| OD-02 | Aprovar operacionalmente Telegram, link e resultado mínimo | O Canal foi recomendado por documentação, mas correlação, timeout, revogação e link ainda não foram provados | Provas POC-01 a POC-05 abaixo, incluindo critérios de reprovação | Contrato do adaptador, simulador e spike isolado em homologação; não liberar Publicação real |
| OD-05 | Política de retenção, exclusão, fundamento e subprocessadores | Exige inventário e revisão qualificada | Política operacional e jurídica aprovada | Modelar categorias, impedir coleta desnecessária e usar dados fictícios |
| OD-06 | Metas de disponibilidade, RPO, RTO, capacidade e orçamento | Devem refletir impacto e escala do piloto | Análise de impacto, custo e teste de restauração | Instrumentação, backups e parâmetros configuráveis |
| OD-07 | Responsáveis por segurança, privacidade, suporte e incidentes | É decisão organizacional, não de framework | Nomes, papéis, escalação e procedimentos aprovados | Preparar auditoria, alertas e exercícios em homologação |
| OD-08 | Limiar quantitativo de validação do piloto | Não existe linha de base de usabilidade | Testes moderados e definição de amostra, metas e regra de decisão | Coletar eventos minimizados e aplicar critérios qualitativos de prontidão |

## Provas de conceito que bloqueiam a Integração real

| ID | Hipótese técnica | Evidência necessária para aprovar | Critério de reprovação |
|---|---|---|---|
| POC-01 | O Usuário consegue vincular o Canal correto com permissão mínima | Em conta oficial de teste, adicionar o bot por link com `post_messages`, correlacionar `my_chat_member` ao Projeto e confirmar `chat_id`, título e direito efetivo | Não for possível provar vínculo inequívoco entre sessão, Projeto e Canal, especialmente após cancelamento ou concorrência |
| POC-02 | Texto pode ser publicado e identificado | `sendMessage` publica texto controlado, retorna `message_id`, data e Canal corretos; limites e erros são normalizados | A mensagem exigir mídia, permissão mais ampla ou não devolver referência persistível |
| POC-03 | Timeout não causa duplicidade insegura | Interromper resposta em pontos controlados, demonstrar política de estado incerto e reconciliação ou bloqueio seguro antes de qualquer repetição | A única recuperação possível for reenviar sem saber se a primeira mensagem existe |
| POC-04 | Revogação e webhook são seguros | Validar `my_chat_member`, repetição por `update_id`, cabeçalho `X-Telegram-Bot-Api-Secret-Token`, remoção do bot e bloqueio de novos envios | Webhook não puder ser autenticado/deduplicado ou remoção não impedir nova Publicação |
| POC-05 | Confirmação e link atendem ao resultado mínimo | Abrir link de mensagem em Canal de teste, provar comportamento público/privado e decidir explicitamente que nenhuma métrica externa é necessária | O Usuário não conseguir verificar o destino ou o link exigir ampliar escopo; métrica não bloqueia aprovação |

## Hipóteses que exigem entrevistas

| ID | Hipótese | Pergunta comportamental necessária |
|---|---|---|
| INT-01 | O pastor é quem realmente publica | Quem publicou os últimos avisos, em qual dispositivo e com que ajuda? |
| INT-02 | A dor é frequente e relevante | Quando houve perda, destino errado, envio implícito, duplicidade ou dúvida de resultado pela última vez? |
| INT-03 | Aviso textual isolado entrega valor | Quais avisos recentes foram publicados sem imagem e o que tornaria texto insuficiente? |
| INT-04 | Telegram é adotável | A igreja já possui Canal; quem o administra; o público o acompanha; o responsável aceitaria adicionar um bot com direito de publicar? |
| INT-05 | O fluxo está pequeno e compreensível | Convite, código, configuração, conexão, criação, revisão e resultado podem ser concluídos sem ajuda? |
| INT-06 | Confirmação e link resolvem o primeiro objetivo | Saber o estado e abrir a mensagem reduz a insegurança; existe necessidade real de métrica no primeiro uso? |

## Adiadas para depois da primeira versão utilizável

| ID | Decisão adiada | Motivo | Gatilho de revisão |
|---|---|---|---|
| OD-03 | Primeiro armazenamento conectado | Aviso textual no Telegram não usa arquivo | Evidência de que um novo tipo aprovado exige mídia externa |
| OD-04 | Limites de arquivo pequeno, grande e temporário | Nenhum arquivo participa do primeiro fluxo | Canal ou formato futuro aprovado exigir transferência ou derivado |
| OD-09 | Provedor, modelo e política de IA generativa | A primeira versão usa Tutor determinístico | Ganho de valor demonstrado, avaliação de português, privacidade, segurança e custo aprovada |
| OD-10 | Aplicativo Mobile | Web responsivo é o cliente primário | Evidência de que instalação, recursos do aparelho ou compartilhamento justificam um segundo cliente |
| OD-11 | Broker dedicado, como SQS | PostgreSQL atende à fila inicial | Idade da fila, volume, contenção, disponibilidade ou isolamento excederem limites aprovados |
| OD-12 | Facebook Pages, Instagram, WhatsApp Business, YouTube e lote multicanal | Telegram é o único Canal do recorte; os demais não melhoram o primeiro fluxo sem dependência ou escopo adicional | Primeiro ciclo validado, demanda pelo segundo Canal e nova prova oficial comprovadas |
| OD-13 | Agendamento, Campanha e colaboração | Fora do caminho crítico do MVP | Pesquisa demonstrar necessidade prioritária após validar publicação imediata |
| OD-14 | Motor de busca, cache distribuído e armazém analítico | PostgreSQL cobre busca e relatórios iniciais | Medição de latência, volume ou consulta justificar infraestrutura adicional |
| OD-15 | Microserviços e múltiplas regiões ativas | Monólito modular e uma região reduzem operação | Necessidade observada de escala, isolamento, equipe, disponibilidade ou conformidade |

## Regra de acompanhamento

Cada decisão deve ser fechada no documento de decisões aprovadas com motivo, benefícios, riscos, reversibilidade e evidência que alterou seu estado. Nenhum item adiado autoriza ampliar o escopo do MVP.
