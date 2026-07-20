# Backlog do Produto

## Convenções

- **P0:** necessário para validar o primeiro ciclo completo.
- **P1:** necessário para recorrência e consolidação após o MVP.
- **P2:** oportunidade futura, dependente de evidência.
- Itens marcados como **Descoberta** reduzem incerteza; itens de **Produto** entregam capacidade ao usuário; itens **Habilitadores** sustentam segurança, operação ou integração.
- A ordem dentro de cada prioridade é indicativa e deve ser revista após a pesquisa.

## Recorte vigente da Sprint 7

O backlog P0 é interpretado pelo primeiro fluxo **pastor convidado → aviso textual → Canal do Telegram → confirmação e link**. Itens amplos que não participam desse caminho não entram na implementação do MVP, mesmo que permaneçam registrados para evolução. Em especial, CNT-02, MED-01, métrica externa de RES-02, múltiplos Canais, armazenamento conectado, Agendamento, IA e colaboração estão adiados.

## P0 — Descoberta e definição do MVP

| ID | Tipo | Item | Critério de aceite |
|---|---|---|---|
| DISC-01 | Descoberta | Validar problemas dos segmentos candidatos | Entrevistas sintetizadas mostram tarefas, dores, frequência, alternativas atuais e diferenças entre segmentos; coerência documental não conta como validação |
| DISC-02 | Descoberta | Confirmar ou rejeitar pastor responsável pela comunicação | Escolha registrada com critérios de intensidade da dor, frequência, responsabilidade real, acesso e potencial de adoção |
| DISC-03 | Descoberta | Validar aviso textual e Telegram como recorte inicial | Entrevistas confirmam valor e adoção; POC-01 a POC-05 confirmam o contrato técnico; exclusões permanecem explícitas |
| DISC-04 | Descoberta | Testar proposta de valor e linguagem | Usuários-alvo explicam corretamente o benefício e o próximo passo sem instrução externa |
| DISC-05 | Descoberta | Testar protótipo de ponta a ponta | Caminho principal e erros críticos observados; problemas priorizados por severidade |
| DISC-06 | Habilitador | Avaliar Integração Telegram | Vínculo, permissão mínima, publicação, timeout, webhook, revogação, link, limites e ausência de métrica obrigatória documentados e provados |
| DISC-07 | Descoberta | Definir métricas do piloto | Cada métrica de produto possui definição, evento minimizado, fonte e interpretação; não depende de engajamento externo |

## P0 — Portões de validação antes da Integração real

### Entrevistas com Usuários

| ID | Hipótese | Critério de aceite |
|---|---|---|
| INT-01 | Pastor ou líder é o responsável real pela Publicação | Evidência de comportamento passado identifica quem publicou os últimos avisos e com qual ajuda |
| INT-02 | Insegurança, dependência ou dúvida de resultado é dor relevante | Episódios recentes, frequência, consequência e alternativa atual estão sintetizados, sem pergunta hipotética como única evidência |
| INT-03 | Aviso textual é suficiente para o primeiro valor | Avisos reais demonstram tarefa útil sem mídia, ou a hipótese é rejeitada sem ampliar o MVP automaticamente |
| INT-04 | Telegram é adotável pelo recorte | Uso atual ou disposição concreta para Canal e bot administrador é observado; audiência e responsável são identificados |
| INT-05 | O fluxo mínimo é compreensível | Protótipo é concluído sem intervenção na tarefa e destino, confirmação, estado e link são explicados corretamente |
| INT-06 | Confirmação e link resolvem o primeiro objetivo | Participante demonstra menor insegurança, verifica a mensagem e explica se uma métrica seria ou não necessária |

### Provas de conceito técnicas

| ID | Prova | Critério de aceite |
|---|---|---|
| POC-01 | Vincular Canal e Projeto | Adição do bot com `post_messages`, evento `my_chat_member`, identidade do Canal e correlação da sessão são inequívocos e revogáveis |
| POC-02 | Publicar texto e persistir resultado | `sendMessage` devolve `message_id`, Canal e horário; limite e erros são normalizados |
| POC-03 | Tratar timeout sem duplicar | Estado incerto bloqueia repetição e existe reconciliação ou decisão operacional comprovadamente segura |
| POC-04 | Proteger e deduplicar webhook | Segredo do webhook, `update_id`, repetição, remoção do bot e perda de direito são verificados |
| POC-05 | Abrir o resultado mínimo | Link da mensagem e comportamento de Canal público/privado são comprovados; nenhuma métrica externa bloqueia o fluxo |

**Regra de passagem:** implementação do núcleo com simulador pode iniciar; o adaptador Telegram real só entra no caminho do MVP após POC-01 a POC-05. O piloto só inicia depois de INT-01 a INT-06, ENG-15, privacidade e operação.

## P0 — Primeiro ciclo completo

### Entrada, perfil e tutoria

| ID | História | Critérios de aceite essenciais |
|---|---|---|
| ONB-01 | Como novo usuário, quero entender o que a plataforma fará para decidir começar | A apresentação explica as cinco etapas, informa o que será necessário e permite sair ou continuar |
| ONB-02 | Como usuário, quero criar meu acesso e perfil com instruções simples | Campos obrigatórios são claros; erros são específicos; progresso pode ser retomado |
| ONB-03 | Como usuário, quero informar meu objetivo e público para receber orientação relevante | Objetivo e público podem ser definidos, revisados e alterados |
| TUT-01 | Como usuário, quero ver meu progresso e o próximo passo | Cada etapa mostra estado, pendências, motivo e uma ação principal inequívoca |
| TUT-02 | Como usuário, quero ajuda contextual sem perder o que já fiz | Ajuda é acessível no contexto e não apaga nem publica conteúdo |

### Contas e canais

| ID | História | Critérios de aceite essenciais |
|---|---|---|
| CHN-01 | Como usuário, quero conectar uma conta de canal com segurança | Benefício e permissões são explicados antes da autorização; sucesso ou falha ficam visíveis |
| CHN-02 | Como usuário, quero saber se meu canal está pronto | Estado da conexão, conta conectada, capacidades e pendências são exibidos |
| CHN-03 | Como usuário, quero reconectar ou remover um canal | O impacto é informado; a ação exige confirmação; conteúdos locais são preservados |
| CHN-04 | Como sistema, preciso proteger credenciais do canal | Tokens não aparecem na interface ou em logs, têm acesso restrito e podem ser revogados |

### Criação e organização

| ID | História | Critérios de aceite essenciais |
|---|---|---|
| CNT-01 | Como usuário, quero criar um conteúdo a partir de uma ideia | É possível informar objetivo, público e mensagem; o rascunho é salvo sem publicar |
| CNT-02 | Como usuário, quero receber assistência na elaboração | **Adiado:** o primeiro aviso é escrito manualmente; nenhuma sugestão altera o Conteúdo |
| CNT-03 | Como usuário, quero editar e revisar o conteúdo | Alterações podem ser salvas; campos pendentes e requisitos do canal são indicados |
| CNT-04 | Como usuário, quero encontrar meus conteúdos | Biblioteca lista conteúdos com título, atualização, tipo e estado; permite abrir e filtrar por estado |
| CNT-05 | Como usuário, quero entender o estado de um conteúdo | Estados usam linguagem consistente e indicam a ação seguinte possível |
| MED-01 | Como usuário, quero adicionar a mídia necessária | **Adiado:** mídia não participa do primeiro fluxo e só retorna com evidência e novo recorte aprovado |

### Publicação

| ID | História | Critérios de aceite essenciais |
|---|---|---|
| PUB-01 | Como usuário, quero visualizar o resultado antes de publicar | Prévia mostra conteúdo, canal, conta de destino e eventuais adaptações ou pendências |
| PUB-02 | Como usuário, quero confirmar conscientemente uma publicação | Publicar exige ação explícita; destino e conteúdo são identificados; duplicidade acidental é evitada |
| PUB-03 | Como usuário, quero acompanhar o envio | Estado distingue aguardando, processando, publicado e falhou, com atualização visível |
| PUB-04 | Como usuário, quero recuperar uma falha | Mensagem usa linguagem simples, preserva o conteúdo e oferece correção ou nova tentativa segura |
| PUB-05 | Como sistema, preciso registrar a operação externa | Tentativas guardam horário, destino, estado, identificador externo e erro sanitizado |

### Resultados

| ID | História | Critérios de aceite essenciais |
|---|---|---|
| RES-01 | Como usuário, quero confirmar onde e quando o conteúdo foi publicado | Link ou identificador externo, canal, conta e horário ficam associados ao conteúdo |
| RES-02 | Como usuário, quero consultar resultados essenciais | O primeiro resultado mostra Canal, mensagem, link, horário e atualização; métrica de engajamento fica adiada |
| RES-03 | Como usuário, quero entender o que fazer depois | O sistema explica limitações dos dados e apresenta um próximo passo acionável sem prometer resultado |

### Qualidade transversal

| ID | Tipo | Item | Critério de aceite |
|---|---|---|---|
| QLT-01 | Habilitador | Acessibilidade do fluxo principal | Navegação por teclado, foco, contraste, rótulos e mensagens são verificados no ciclo completo |
| QLT-02 | Habilitador | Privacidade e consentimento | Coleta, finalidade, retenção, remoção e acesso a dados são definidos antes do piloto |
| QLT-03 | Habilitador | Observabilidade | Falhas de conexão e publicação podem ser diagnosticadas sem expor conteúdo ou credenciais |
| QLT-04 | Habilitador | Instrumentação do funil | Início, conclusão, abandono e erro das cinco etapas são mensuráveis |
| QLT-05 | Habilitador | Salvamento e recuperação | Interrupção ou falha de rede não provoca publicação nem perda silenciosa do rascunho |

## P1 — Organização e recorrência

| ID | Item | Resultado esperado |
|---|---|---|
| ORG-01 | Calendário editorial | Visualizar conteúdos por data e estado |
| ORG-02 | Agendamento | Escolher horário futuro, confirmar e cancelar antes do envio |
| ORG-03 | Duplicação e reaproveitamento | Criar novo conteúdo sem alterar o original |
| ORG-04 | Histórico de versões | Identificar e recuperar versões relevantes |
| ORG-05 | Modelos guiados por objetivo | Reduzir o esforço para iniciar conteúdos recorrentes |
| TUT-03 | Lembretes e retomada | Retomar tarefas incompletas com contexto |
| RES-04 | Comparação de resultados | Comparar conteúdos equivalentes com período e limitações claros |
| RES-05 | Recomendações baseadas no histórico | Sugerir melhoria explicável e sujeita à aprovação do usuário |

## P2 — Expansão

| ID | Item | Dependência principal |
|---|---|---|
| MCH-01 | Conectar canais adicionais | Evidência de demanda e viabilidade de cada API |
| MCH-02 | Criar versões por canal | Modelo de conteúdo e regras de adaptação validados |
| MCH-03 | Publicação coordenada multicanal | Conectores confiáveis e estados independentes por destino |
| MCH-04 | Resultados consolidados | Normalização responsável de métricas diferentes |
| COL-01 | Papéis e colaboração | Necessidade confirmada de equipes |
| AST-01 | Assistência multimodal avançada | Qualidade, custo, direitos e segurança validados |
| ECO-01 | Integrações complementares | Casos de uso recorrentes e modelo de extensão definido |

## Fora do backlog comprometido

- automação de publicação sem aprovação configurável e transparente;
- compra e gestão de mídia paga;
- edição profissional completa de áudio, vídeo ou imagem;
- garantia de alcance, seguidores, vendas ou engajamento;
- rede social própria;
- marketplace de influenciadores.

## Definição de pronto para desenvolvimento

Um item só entra em desenvolvimento quando possui usuário e resultado claros, critérios de aceite verificáveis, dependências conhecidas, estados de erro relevantes, requisitos de dados e segurança avaliados e vínculo com uma métrica ou hipótese.

## Definição de concluído

Um item está concluído quando atende aos critérios de aceite, possui testes proporcionais ao risco, respeita acessibilidade e privacidade aplicáveis, está instrumentado quando necessário, tem comportamento de erro compreensível e sua documentação afetada foi atualizada.
