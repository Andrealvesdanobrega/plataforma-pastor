# Decisões Abertas

**Fase:** após Sprint 5 — Decisões de Engenharia do MVP

**Atualizado em:** 20 de julho de 2026

As escolhas fundamentais de linguagem, frameworks, banco, ORM, identidade, fila, armazenamento próprio, Publicação, IA inicial e ambientes estão aprovadas em `APPROVED-DECISIONS.md`. Os itens abaixo permanecem abertos porque dependem de evidência de produto, contrato externo, governança ou medição; fechá-los por preferência técnica aumentaria o risco sem liberar trabalho adicional seguro.

## Bloqueiam o piloto, mas não o início da implementação do núcleo

| ID | Decisão adiada | Motivo do adiamento | Evidência para fechar | Trabalho permitido enquanto aberta |
|---|---|---|---|---|
| OD-01 | Segmento, caso de uso e tipo de Conteúdo inicial | Ainda não há síntese de pesquisa | Entrevistas e escolha registrada por dor, frequência, acesso e adoção | Identidade, Projeto, estados genéricos, Tutor e Biblioteca |
| OD-02 | Primeiro Canal, formato e métricas essenciais | Define permissões, mídia, publicação e resultado | Prova oficial de autorização, envio, estado, webhook, métricas e revogação | Contrato do adaptador, simulador, Publicação e reconciliação |
| OD-03 | Primeiro armazenamento conectado | Depende do hábito do segmento e do formato | Pesquisa e prova de seleção, leitura, revogação e arquivo removido | Contrato do adaptador e S3 para objetos pequenos/temporários |
| OD-04 | Limites de arquivo pequeno, grande e temporário | Custo e duração dependem do Canal e formato | Medições de tamanho, memória, transferência, tempo e custo | Limites conservadores configuráveis, sem prometer formato final |
| OD-05 | Política de retenção, exclusão, fundamento e subprocessadores | Exige inventário e revisão qualificada | Política operacional e jurídica aprovada | Modelar categorias, impedir coleta desnecessária e usar dados fictícios |
| OD-06 | Metas de disponibilidade, RPO, RTO, capacidade e orçamento | Devem refletir impacto e escala do piloto | Análise de impacto, custo e teste de restauração | Instrumentação, backups e parâmetros configuráveis |
| OD-07 | Responsáveis por segurança, privacidade, suporte e incidentes | É decisão organizacional, não de framework | Nomes, papéis, escalação e procedimentos aprovados | Preparar auditoria, alertas e exercícios em homologação |
| OD-08 | Limiar quantitativo de validação do piloto | Não existe linha de base de usabilidade | Testes moderados e definição de amostra, metas e regra de decisão | Coletar eventos minimizados e aplicar critérios qualitativos de prontidão |

## Adiadas para depois da primeira versão utilizável

| ID | Decisão adiada | Motivo | Gatilho de revisão |
|---|---|---|---|
| OD-09 | Provedor, modelo e política de IA generativa | A primeira versão usa Tutor determinístico | Ganho de valor demonstrado, avaliação de português, privacidade, segurança e custo aprovada |
| OD-10 | Aplicativo Mobile | Web responsivo é o cliente primário | Evidência de que instalação, recursos do aparelho ou compartilhamento justificam um segundo cliente |
| OD-11 | Broker dedicado, como SQS | PostgreSQL atende à fila inicial | Idade da fila, volume, contenção, disponibilidade ou isolamento excederem limites aprovados |
| OD-12 | Múltiplos Canais e lote multicanal | Fora do recorte de um Canal | Primeiro ciclo validado e demanda por segundo Canal comprovada |
| OD-13 | Agendamento, Campanha e colaboração | Fora do caminho crítico do MVP | Pesquisa demonstrar necessidade prioritária após validar publicação imediata |
| OD-14 | Motor de busca, cache distribuído e armazém analítico | PostgreSQL cobre busca e relatórios iniciais | Medição de latência, volume ou consulta justificar infraestrutura adicional |
| OD-15 | Microserviços e múltiplas regiões ativas | Monólito modular e uma região reduzem operação | Necessidade observada de escala, isolamento, equipe, disponibilidade ou conformidade |

## Regra de acompanhamento

Cada decisão deve ser fechada no documento de decisões aprovadas com motivo, benefícios, riscos, reversibilidade e evidência que alterou seu estado. Nenhum item adiado autoriza ampliar o escopo do MVP.
