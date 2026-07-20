# Arquitetura Funcional

**Versão:** 0.1

**Fase:** Product Discovery

**Atualizado em:** 19 de julho de 2026

## 1. Objetivo

Definir as responsabilidades funcionais, os limites entre domínios e os fluxos do produto sem prescrever tecnologia. Esta arquitetura deriva do blueprint e deve ser refinada depois que segmento, canal, formato e escopo do MVP forem validados.

## 2. Princípio estrutural

O **Conteúdo** é o agregado funcional central. Perfil, mídias, versões por canal, publicações e resultados se relacionam a ele, enquanto a **Tutoria** coordena a jornada e apresenta ao usuário o próximo passo possível.

```text
Usuário
  │
  ▼
Experiência única + Tutor
  │
  ├── Identidade e Perfil
  ├── Canais Conectados
  ├── Conteúdo ── Mídias e Versões
  ├── Organização
  ├── Publicação ── Conectores Externos
  └── Resultados  ── Coleta Externa
          │
          ▼
   Orientação de próximo passo
```

## 3. Atores

| Ator | Responsabilidade ou interesse |
|---|---|
| Usuário proprietário | Configura perfil, conecta contas, cria, aprova, publica e acompanha conteúdo |
| Tutor do sistema | Explica, valida pendências, recomenda e coordena o avanço da jornada |
| Canal externo | Autoriza acesso, recebe publicações e fornece estados e métricas |
| Suporte autorizado | Ajuda a diagnosticar problemas sem acessar credenciais ou agir como o usuário sem autorização |
| Operação da plataforma | Monitora integrações, políticas, incidentes e qualidade do serviço |

Papéis colaborativos adicionais não pertencem ao MVP e exigirão um modelo próprio de autorização.

## 4. Contextos funcionais

### 4.1 Identidade e Perfil

**Responsabilidades:** autenticar o usuário, manter perfil, objetivo, público, preferências, consentimentos e progresso inicial.

**Fornece:** identidade do proprietário e contexto para tutoria e criação.

**Não deve:** armazenar credenciais de canais como campos comuns de perfil.

### 4.2 Tutoria e Jornada

**Responsabilidades:** calcular etapa, pendências e próxima ação; fornecer orientação contextual; registrar conclusão ou abandono de marcos.

**Consome:** perfil, saúde dos canais, estado do conteúdo, publicação e resultados.

**Fornece:** uma visão simples e acionável do progresso.

**Regra:** pode recomendar e navegar, mas não publicar nem alterar conteúdo sem ação autorizada.

### 4.3 Canais e Contas Conectadas

**Responsabilidades:** iniciar autorização, guardar referência protegida de credencial, identificar conta externa, conhecer capacidades, renovar ou revogar acesso e informar saúde da conexão.

**Fornece:** destinos aptos e suas restrições.

**Regra:** uma conexão pertence ao usuário e pode estar ativa, requerer reconexão, estar inválida ou desconectada.

### 4.4 Conteúdo

**Responsabilidades:** criar e editar a fonte editorial, manter objetivo, público, corpo, tipo, estado, autoria e versões.

**Fornece:** fonte para organização, adaptação, publicação e análise.

**Regra:** salvar conteúdo nunca implica publicar.

### 4.5 Mídias e Adaptação

**Responsabilidades:** receber arquivos, validar metadados e restrições, associar mídias ao conteúdo e produzir versões específicas para destinos quando necessário.

**Fornece:** versão candidata compatível com um canal.

**Regra:** adaptações relevantes devem ser exibidas e aprovadas antes da publicação.

### 4.6 Organização

**Responsabilidades:** listar, buscar, filtrar e, após o MVP, planejar conteúdos no calendário.

**Consome:** metadados e estados do conteúdo.

**Regra:** organização não duplica a fonte; apresenta visões sobre o mesmo conteúdo.

### 4.7 Publicação

**Responsabilidades:** validar prontidão, congelar a versão aprovada, solicitar confirmação, criar tentativa, enviar pelo conector, reconciliar estado e permitir recuperação.

**Consome:** versão do conteúdo, mídia e conexão apta.

**Fornece:** registro da publicação e referência externa.

**Regra:** cada destino tem ciclo independente e operações repetidas devem ser idempotentes sempre que possível.

### 4.8 Resultados e Aprendizado

**Responsabilidades:** coletar métricas permitidas, registrar período e atualização, apresentar explicações e gerar recomendações limitadas pela evidência.

**Consome:** publicações confirmadas e dados dos canais.

**Fornece:** visão de desempenho relacionada ao conteúdo e insumos para a tutoria.

**Regra:** métricas de canais diferentes não são equivalentes por padrão.

### 4.9 Notificações

**Responsabilidades:** avisar sobre ação requerida, falha, conclusão ou resultado relevante segundo preferências do usuário.

**Regra:** notificações informam e direcionam; não confirmam ações de alto impacto em nome do usuário.

### 4.10 Governança e Operação

**Responsabilidades:** auditoria, consentimento, retenção, moderação aplicável, observabilidade, suporte e conformidade com políticas externas.

**Regra:** dados operacionais devem minimizar conteúdo pessoal e excluir credenciais.

## 5. Modelo funcional de informação

| Entidade | Descrição | Relações principais |
|---|---|---|
| Usuário | Pessoa que utiliza e controla a conta | Possui perfil, conexões e conteúdos |
| Perfil | Objetivo, público, preferências e progresso | Pertence a um usuário |
| Conexão de canal | Vínculo autorizado com uma conta externa | Pertence a um usuário; habilita destinos |
| Conteúdo | Fonte editorial central | Pertence a usuário; possui versões, mídias e destinos |
| Versão de conteúdo | Registro editável ou instantâneo aprovado | Pertence a conteúdo; pode alimentar uma publicação |
| Mídia | Arquivo e seus metadados | Associada a conteúdo ou versão |
| Destino | Combinação de canal e conta escolhida | Usa uma conexão; recebe uma versão preparada |
| Publicação | Representa a intenção aprovada de publicar | Liga conteúdo, versão e destino |
| Tentativa de publicação | Execução individual e seu resultado | Pertence a uma publicação |
| Métrica | Valor obtido de uma fonte em um período | Relaciona-se a publicação e conteúdo |
| Recomendação | Próximo passo explicado ao usuário | Deriva de contexto e resultados conhecidos |
| Marco da jornada | Estado de avanço e pendências | Relaciona usuário ao fluxo guiado |

## 6. Estados e transições

### 6.1 Conteúdo

```text
Ideia → Rascunho → Precisa de revisão → Pronto
            ↑              │              │
            └──────────────┘              ▼
                                  Em publicação
                                    │       │
                                    ▼       ▼
                               Publicado  Com falha
                                    ▲       │
                                    └───────┘

Qualquer estado editável → Arquivado → estado editável anterior
```

- Editar depois de uma publicação cria nova versão e não altera o que já foi enviado.
- “Com falha” preserva a versão e o diagnóstico seguro da tentativa.
- Em cenário multicanal, o estado do conteúdo é um resumo; a publicação por destino é a fonte precisa.

### 6.2 Conexão de canal

```text
Não conectada → Autorizando → Ativa
                     │          │
                     ▼          ▼
                   Falhou   Requer reconexão
                                  │
                                  └→ Ativa

Ativa ou Requer reconexão → Desconectada
```

### 6.3 Publicação por destino

```text
Preparação → Aguardando confirmação → Na fila → Processando → Publicada
                    │                    │           │
                    └→ Cancelada         └───────────┴→ Falhou → Nova tentativa
```

Depois que o canal confirma a publicação, cancelamento local não significa remoção externa. Qualquer remoção futura deve ser uma operação explícita, com suporte confirmado pelo canal.

## 7. Fluxos principais

### 7.1 Primeiro acesso e configuração

1. Usuário cria acesso e recebe explicação do ciclo.
2. Informa contexto mínimo de objetivo e público.
3. Tutor recomenda a conexão necessária para o caso de uso.
4. Canais solicita autorização externa e registra somente o vínculo protegido.
5. Plataforma verifica identidade, capacidades e saúde da conta.
6. Tutor mostra conclusão ou ação de correção e permite retomada.

### 7.2 Criação e organização

1. Usuário inicia a partir de ideia, modelo ou conteúdo existente.
2. Conteúdo registra objetivo, público e mensagem.
3. Tutor solicita apenas as informações necessárias à etapa.
4. Sugestões aceitas tornam-se edição explícita; sugestões rejeitadas não alteram a fonte.
5. Mídias são validadas conforme o destino quando ele existir.
6. Conteúdo é salvo e aparece na biblioteca com estado e próximo passo.

### 7.3 Publicação

1. Usuário escolhe conteúdo, conexão e destino.
2. Sistema valida conexão, campos, formato e mídia.
3. Sistema prepara uma versão específica e apresenta prévia, conta e canal.
4. Usuário confirma a ação.
5. Publicação congela a versão e cria uma tentativa identificável.
6. Conector envia e retorna aceitação, falha ou estado pendente.
7. Sistema reconcilia o resultado, evita duplicidade e informa o usuário.

### 7.4 Acompanhamento

1. Resultados seleciona publicações elegíveis.
2. Conector consulta dados autorizados respeitando limites externos.
3. Métricas são registradas com fonte, período e instante de atualização.
4. Interface traduz nomes e limitações sem inventar equivalências.
5. Tutor apresenta uma leitura simples e um próximo passo justificável.

## 8. Contrato funcional dos conectores

Cada conector de canal deve declarar:

- método de autorização e permissões;
- tipos, campos, mídias e limites aceitos;
- capacidade de publicar, consultar estado e evitar duplicidade;
- estados e erros traduzíveis;
- métricas disponíveis, definições e janelas de atualização;
- limites de uso, política de nova tentativa e indisponibilidade;
- expiração, renovação e revogação de credenciais;
- requisitos de revisão, auditoria e conformidade do canal.

O núcleo do produto não deve assumir que todos os canais suportam agendamento, remoção, edição posterior ou as mesmas métricas.

## 9. Regras de autorização e segurança

- O usuário acessa apenas seu perfil, suas conexões, seus conteúdos e seus resultados no MVP.
- Credenciais externas são usadas somente pelo contexto de canais e conectores autorizados.
- Suporte não visualiza tokens e qualquer acesso excepcional a dados deve ser restrito e auditado.
- Publicação exige confirmação associada a usuário, versão, conta e destino.
- Operações sensíveis produzem registro de auditoria sem segredo e com minimização de conteúdo.
- Revogar ou desconectar bloqueia novas operações externas imediatamente após a propagação possível.

## 10. Tratamento funcional de falhas

| Situação | Comportamento esperado |
|---|---|
| Credencial expirada | Marcar conexão como requer reconexão, preservar conteúdo e orientar o usuário |
| Canal indisponível | Manter tentativa rastreável, informar atraso e aplicar nova tentativa segura conforme política |
| Resultado de envio desconhecido | Consultar estado antes de reenviar para reduzir duplicidade |
| Conteúdo incompatível | Bloquear confirmação, indicar requisito e levar ao campo correto |
| Limite externo excedido | Explicar quando possível tentar novamente sem prometer horário inexato |
| Falha na coleta de métricas | Manter último dado com data e indicar indisponibilidade da atualização |
| Interrupção do usuário ou rede | Preservar rascunho e nunca interpretar interrupção como confirmação |

## 11. Eventos funcionais observáveis

Para medir o funil sem registrar conteúdo desnecessário, o produto deve distinguir ao menos:

- onboarding iniciado e concluído;
- conexão iniciada, concluída, falhou e foi revogada;
- conteúdo criado, considerado pronto e arquivado;
- prévia aberta e publicação confirmada;
- tentativa iniciada, publicada ou falhou;
- resultados consultados;
- recomendação apresentada e adotada;
- etapa abandonada e erro recuperado.

Cada evento deve ter finalidade, retenção e propriedades mínimas definidas antes da implementação.

## 12. Recorte arquitetural do MVP

O MVP implementa todos os contextos necessários ao ciclo completo, porém com profundidade limitada: um papel de usuário, um conector, um formato principal, biblioteca simples, publicação imediata e conjunto mínimo de métricas. Calendário, colaboração, múltiplos conectores e recomendações avançadas permanecem extensões, não pré-requisitos estruturais artificiais.

## 13. Decisões pendentes

- identidade e recuperação de acesso apropriadas ao público;
- segmento, canal e formato iniciais;
- fonte e política das sugestões de criação;
- semântica final dos estados editoriais;
- estratégia de consistência e idempotência do conector escolhido;
- frequência e retenção de métricas;
- notificações indispensáveis ao piloto;
- regras legais de consentimento, exclusão e suporte;
- limites entre dados operacionais, analíticos e conteúdo do usuário.

## 14. Critérios de validação da arquitetura funcional

Esta arquitetura estará pronta para orientar a arquitetura técnica quando:

- o protótipo confirmar que usuários compreendem objetos, estados e ações;
- o MVP definir canal, formato e métricas;
- a investigação do conector confirmar capacidades e restrições;
- regras de autorização, consentimento e publicação forem aprovadas;
- fluxos de falha críticos tiverem comportamento aceito;
- entidades e eventos necessários às métricas do piloto estiverem definidos.

Mudanças neste documento exigem revisão do blueprint, backlog, roadmap e, quando existente, arquitetura técnica e modelo de dados.
