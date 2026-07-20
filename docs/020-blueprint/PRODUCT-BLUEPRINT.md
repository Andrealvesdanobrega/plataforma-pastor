# Blueprint do Produto

**Versão:** 0.3

**Fase:** Sprint 6 — Primeiro Fluxo Vertical do MVP

**Atualizado em:** 20 de julho de 2026

## 1. Propósito deste documento

Traduzir a missão e a visão em um modelo de produto testável. Este blueprint orienta descoberta, experiência, arquitetura funcional, definição do MVP e backlog. Hipóteses são explicitadas para não serem confundidas com decisões validadas.

## 2. Problema

Pessoas que precisam comunicar ideias, mensagens, serviços ou conhecimento online encontram uma jornada fragmentada. Elas precisam criar contas, aprender regras de canais, produzir formatos diferentes, guardar arquivos, publicar corretamente e interpretar métricas. Para usuários iniciantes, isso aumenta esforço, insegurança, erros e dependência de terceiros.

## 3. Proposta de valor

Uma plataforma única que conduz o usuário como um tutor e transforma a publicação digital em um processo compreensível: configurar, criar, organizar, publicar e acompanhar.

### Promessa central

“Você traz a mensagem; a plataforma ajuda a preparar, publicar e aprender com ela.”

### Diferenciais pretendidos

- jornada completa no mesmo ambiente;
- orientação contextual, e não apenas funções isoladas;
- conteúdo como fonte central para organização e distribuição;
- linguagem pensada para iniciantes;
- resultados traduzidos em próximos passos;
- aprovação humana e transparência em ações importantes.

## 4. Público inicial

### Segmentos candidatos

| Segmento | Necessidade provável | Contexto a investigar |
|---|---|---|
| Pastores e líderes de comunidades | Compartilhar mensagens e manter comunicação recorrente | Rotina, voluntários, formatos longos e curtos, sensibilidade do conteúdo |
| Pequenos empreendedores | Divulgar produtos, serviços e relacionamento | Tempo disponível, conversão esperada, sazonalidade e canais usados |
| Influenciadores iniciantes | Construir consistência e entender desempenho | Frequência, identidade, experimentação e ansiedade por métricas |
| Criadores iniciantes | Transformar ideias em publicações organizadas | Processo criativo, formatos, bloqueios e reaproveitamento |

### Características comuns presumidas

- baixa ou média confiança no uso de ferramentas digitais;
- pouco tempo ou apoio especializado;
- dificuldade em manter consistência;
- uso de mais de uma ferramenta ou canal;
- receio de cometer erros públicos;
- dificuldade para interpretar resultados.

Essas características devem ser verificadas. Para a Sprint 6, o primeiro recorte de validação é um pastor ou líder que cuida pessoalmente da comunicação de uma pequena igreja, administra uma Página do Facebook e precisa publicar avisos textuais. Essa hipótese não substitui pesquisa nem prova da Integração.

## 5. Jobs to be done

### Trabalho principal

Quando tenho uma mensagem ou objetivo para comunicar, quero receber orientação para transformá-lo em conteúdo adequado, publicá-lo com segurança e entender o resultado, para manter minha presença digital sem depender de conhecimento técnico especializado.

### Trabalhos funcionais

- preparar contas e permissões necessárias;
- estruturar uma ideia em conteúdo;
- adaptar forma e mídia ao destino;
- localizar e saber o estado de cada conteúdo;
- revisar e controlar a publicação;
- verificar o que aconteceu e decidir o próximo passo.

### Trabalhos emocionais e sociais

- sentir segurança antes de tornar algo público;
- evitar parecer despreparado ou pouco profissional;
- manter autenticidade e propriedade sobre a mensagem;
- sentir progresso e autonomia;
- comunicar-se com consistência com sua comunidade ou clientes.

## 6. Princípios de experiência

1. **Uma ação principal por contexto:** o próximo passo deve ser evidente.
2. **Explicar antes de pedir:** permissões, dados e consequências aparecem antes da confirmação.
3. **Progresso persistente:** o usuário pode interromper e retomar sem perda silenciosa.
4. **Revisão antes da ação externa:** publicar, desconectar ou remover exige confirmação proporcional ao impacto.
5. **Erros recuperáveis:** toda falha preserva o trabalho e informa como avançar.
6. **Sugestão não é decisão:** conteúdo sugerido só é adotado mediante ação do usuário.
7. **Complexidade progressiva:** opções avançadas aparecem quando necessárias.
8. **Linguagem concreta:** termos técnicos dos canais são traduzidos ou explicados.

## 7. Objeto central: conteúdo

Um **Conteúdo** representa a intenção editorial principal do usuário. Ele reúne:

- título ou identificação interna;
- objetivo e público;
- mensagem ou corpo principal;
- tipo e mídias associadas;
- autoria e datas;
- versões editáveis;
- versões específicas por canal;
- destinos de publicação;
- estado editorial;
- publicações e tentativas;
- métricas e aprendizados associados.

### Estados editoriais iniciais

| Estado | Significado | Próxima ação típica |
|---|---|---|
| Ideia | Registro inicial ainda não estruturado | Começar criação |
| Rascunho | Conteúdo em elaboração | Continuar editando |
| Precisa de revisão | Há campos, regras ou decisões pendentes | Corrigir pendências |
| Pronto | Conteúdo validado para o destino escolhido | Revisar publicação |
| Em publicação | Envio solicitado e ainda não concluído | Aguardar ou consultar estado |
| Publicado | Ao menos uma publicação foi confirmada | Acompanhar resultados |
| Com falha | Uma tentativa não foi concluída | Corrigir ou tentar novamente |
| Arquivado | Removido do fluxo ativo sem ser apagado | Restaurar se necessário |

O estado editorial resume a situação do conteúdo; cada destino possui também seu próprio estado de publicação. Essa separação evita tratar uma falha em um canal como falha de todos os destinos.

## 8. Jornada de ponta a ponta

### Etapa 1 — Configurar

**Objetivo do usuário:** ficar pronto para usar a plataforma e publicar.

**Tutor:** explica o processo, coleta objetivo e público, orienta a conexão e confirma pendências.

**Saída:** perfil mínimo e ao menos um canal apto.

### Etapa 2 — Criar

**Objetivo do usuário:** transformar uma ideia em conteúdo.

**Tutor:** pergunta intenção, sugere estrutura e aponta requisitos do destino.

**Saída:** rascunho salvo e editável.

### Etapa 3 — Organizar

**Objetivo do usuário:** localizar, preparar e planejar conteúdos.

**Tutor:** apresenta estado, pendências, versões e ação seguinte.

**Saída:** conteúdo pronto para publicação ou guardado para retomada.

### Etapa 4 — Publicar

**Objetivo do usuário:** enviar o conteúdo correto ao destino correto.

**Tutor:** valida requisitos, mostra a prévia, identifica conta e canal e solicita confirmação.

**Saída:** publicação confirmada ou falha recuperável, sem perda do conteúdo.

### Etapa 5 — Acompanhar

**Objetivo do usuário:** entender o resultado e melhorar.

**Tutor:** informa período e limitações, traduz métricas e sugere uma ação prática.

**Saída:** aprendizado registrado e próximo passo compreendido.

## 9. Capacidades do produto

| Domínio | Responsabilidade |
|---|---|
| Identidade e perfil | Acesso, preferências, objetivo, público e consentimentos |
| Tutoria | Progresso, orientação contextual, pendências e próximos passos |
| Canais | Conexão, permissões, capacidades e saúde das contas externas |
| Conteúdo | Criação, edição, estados, versões, requisitos e biblioteca |
| Mídia | Upload, validação, associação e preparação de arquivos |
| Organização | Busca, filtros, calendário e retomada |
| Publicação | Prévia, confirmação, envio, idempotência, estado e recuperação |
| Resultados | Coleta, atualização, explicação e recomendações |
| Notificações | Avisos úteis sobre pendências, falhas e resultados, com preferências |
| Governança | Auditoria, privacidade, segurança, moderação e políticas |

## 10. Escopo proposto do MVP

### Incluído

- um pastor convidado e proprietário de um espaço de igreja;
- uma Página do Facebook conectada, condicionada à prova técnica;
- um tipo de Conteúdo: aviso textual curto;
- entrada por código de e-mail e configuração mínima;
- Tutor determinístico com a intenção **Publicar um aviso**;
- criação, edição, salvamento e retomada do Rascunho;
- Biblioteca simples para listar, abrir e arquivar;
- prévia, confirmação explícita e Publicação imediata;
- sucesso, falha recuperável e resultado incerto;
- confirmação, link e no máximo uma métrica externa comprovada;
- um próximo passo simples;
- instrumentação do ciclo principal.

### Fora do MVP

- cadastro público irrestrito;
- tipos de Conteúdo além de aviso textual;
- mídia, upload e armazenamento conectado;
- geração ou adaptação de texto por IA;
- publicação simultânea em vários canais;
- calendário editorial e agendamento avançados;
- colaboração em equipe e aprovações multinível;
- busca, filtros avançados, etiquetas, duplicação e ações em massa;
- notificações externas;
- edição profissional de mídia;
- automações autônomas de publicação;
- métricas avançadas, comparação, análise preditiva ou garantia de desempenho;
- mídia paga, comércio eletrônico ou CRM completo.

## 11. Regras de negócio iniciais

- Um conteúdo pode existir sem canal conectado e nunca é publicado por ser apenas criado ou salvo.
- Toda publicação se refere a uma versão imutável do conteúdo naquele momento.
- O usuário deve identificar a conta e o canal de destino antes de confirmar.
- Uma nova tentativa não pode criar duplicidade sem que o sistema verifique o estado anterior ou peça decisão.
- Desconectar um canal impede novos envios, mas não apaga conteúdos ou histórico local.
- Métricas devem informar fonte, período e última atualização.
- Recomendações não podem afirmar causalidade sem evidência nem prometer resultados.
- Credenciais externas não podem ser expostas ao usuário, aos logs ou a componentes sem necessidade.

## 12. Métricas candidatas

### Valor e ativação

- taxa de conclusão do onboarding;
- taxa de conexão bem-sucedida do primeiro canal;
- tempo até o primeiro rascunho;
- taxa e tempo até a primeira publicação confirmada.

### Qualidade da experiência

- conclusão sem ajuda externa;
- abandono por etapa;
- erros e recuperação por etapa;
- compreensão do estado e do próximo passo;
- percepção de confiança antes da publicação.

### Retenção e valor contínuo

- retorno para criar um segundo conteúdo;
- conteúdos publicados por usuário ativo;
- retorno para consultar resultados;
- adoção de recomendações;
- redução de tempo ou ferramentas percebida pelo usuário.

As definições exatas e metas dependem da linha de base do piloto. Métricas de vaidade, como volume bruto de conteúdo, não devem ser usadas isoladamente como sucesso.

## 13. Hipóteses e experimentos

| Hipótese | Experimento inicial | Sinal favorável |
|---|---|---|
| A fragmentação é uma dor relevante | Entrevistas baseadas em comportamento passado | Padrão recorrente de troca de ferramentas, erro ou abandono |
| A tutoria aumenta autonomia | Teste de tarefa com protótipo | Conclusão com pouca ajuda e próximo passo compreendido |
| Conteúdo é um modelo mental claro | Card sorting e teste da biblioteca | Usuários localizam itens e explicam estados corretamente |
| Usuários confiam na conexão guiada | Teste da tela de permissões | Benefício e permissões são compreendidos antes de aceitar |
| Acompanhamento gera recorrência | Protótipo de resultado e entrevista | Usuários identificam decisão prática e intenção de retorno |
| Há base comum entre segmentos | Síntese comparativa das entrevistas | Necessidades centrais semelhantes e diferenças administráveis |

## 14. Requisitos não funcionais de produto

- fluxo principal acessível por teclado e compatível com tecnologias assistivas;
- linguagem em português simples e mensagens que indiquem correção;
- proteção de dados pessoais e credenciais por padrão;
- rastreabilidade de publicação sem armazenar segredos em logs;
- resiliência a indisponibilidade e limites de canais externos;
- salvamento confiável de rascunhos;
- desempenho adequado a conexões comuns do público-alvo;
- observabilidade do funil e de falhas críticas;
- possibilidade de revogar acesso e solicitar exclusão conforme política aplicável.

## 15. Dependências e restrições

- APIs, políticas, revisão e limites dos canais;
- formatos e métricas diferentes entre destinos;
- consentimento e tratamento de dados pessoais;
- direitos sobre textos e mídias enviados ou sugeridos;
- custo e qualidade de serviços de assistência automatizada;
- disponibilidade de participantes representativos para pesquisa.

## 16. Critérios para revisar este blueprint

Este documento deve ser atualizado quando uma hipótese central for validada ou rejeitada, quando segmento ou canal prioritário mudar, quando o MVP for aprovado ou quando uma limitação técnica alterar a jornada. Toda mudança que afete capacidades deve provocar revisão da arquitetura funcional, do backlog e do roadmap.

## 17. Diretriz geral da experiência

A experiência deve funcionar como uma conversa guiada, não como um painel que exige conhecimento prévio. Em cada momento, o usuário precisa conseguir responder a três perguntas:

1. Onde estou?
2. O que preciso fazer agora?
3. O que acontecerá depois?

O Tutor sempre inicia uma interação de ajuda com a pergunta exata:

> O que você quer fazer hoje?

Essa pergunta aparece na primeira entrada, na tela principal e sempre que o usuário abre o Tutor. Depois dela, o sistema oferece poucas opções relacionadas ao contexto, sem exigir que a pessoa saiba o nome de uma função.

### 17.1 Regras de simplicidade

- Apresentar uma decisão principal por tela ou etapa.
- Oferecer no máximo três ou quatro caminhos principais por vez.
- Recomendar uma opção quando houver escolha complexa e explicar por que ela é adequada.
- Manter configurações avançadas escondidas até que sejam necessárias.
- Usar verbos cotidianos: “Criar”, “Continuar”, “Revisar”, “Publicar” e “Ver resultados”.
- Evitar siglas e termos como API, token, credencial, payload, sincronização e autenticação na interface.
- Trocar “erro de autenticação” por “Sua conta precisa ser conectada novamente”.
- Trocar “falha no processamento” por “Não conseguimos concluir agora. Seu conteúdo está salvo”.
- Nunca depender apenas de cor para comunicar estado.
- Mostrar progresso em passos nomeados e indicar quando algo foi salvo.
- Permitir voltar sem perder dados e sair com uma indicação clara de como retomar.

### 17.2 Estrutura de navegação

A navegação principal do MVP possui cinco destinos com nomes simples:

| Destino | Finalidade | Ação principal |
|---|---|---|
| Início | Mostrar situação atual e próximo passo | Continuar a jornada |
| Criar | Iniciar um novo Conteúdo | Contar uma ideia |
| Biblioteca | Encontrar e organizar Conteúdos | Continuar ou revisar |
| Resultados | Entender o que aconteceu após publicar | Ver um resultado |
| Ajuda | Conversar com o Tutor | Dizer o que deseja fazer |

“Contas conectadas” e dados do perfil ficam em Configurações, acessíveis pelo cabeçalho. Durante o onboarding, a navegação principal pode permanecer reduzida para evitar desvio antes da configuração mínima.

### 17.3 Estrutura padrão de uma etapa

Cada etapa guiada contém:

- título que descreve a tarefa;
- frase curta explicando por que ela é necessária;
- indicador de progresso, como “Passo 2 de 4”;
- campos ou escolhas essenciais;
- uma ação principal destacada;
- uma ação secundária segura, como “Voltar” ou “Continuar depois”;
- acesso ao Tutor;
- confirmação de salvamento, sucesso ou ação necessária.

## 18. Fluxo de onboarding

### 18.1 Objetivo

Receber uma pessoa que ainda não conhece o produto, explicar o benefício sem sobrecarregá-la e levá-la até um acesso criado ou recuperado.

### 18.2 Sequência completa

#### Tela 1 — Boas-vindas

- Mostrar a promessa: “Crie, organize e publique seus conteúdos com ajuda em cada passo”.
- Exibir uma ilustração ou exemplo simples do ciclo.
- Ação principal: **Começar**.
- Ação secundária: **Já tenho uma conta**.
- Tutor inicia com: “O que você quer fazer hoje?”
- Opções sugeridas: “Conhecer a plataforma”, “Criar meu acesso” e “Entrar na minha conta”.

#### Tela 2 — Como funciona

- Apresentar três blocos curtos: “Você conta sua ideia”, “Nós ajudamos a preparar” e “Você revisa e publica”.
- Informar que nada será publicado sem confirmação.
- Ação principal: **Continuar**.
- Permitir ouvir ou reler a explicação quando recursos de acessibilidade estiverem disponíveis.

#### Tela 3 — Criar acesso

- Pedir apenas nome e forma de acesso necessária ao piloto.
- Explicar senhas ou códigos com exemplos e requisitos visíveis antes do envio.
- Marcar campos obrigatórios em texto.
- Validar um campo por vez, junto ao local que precisa de correção.
- Ação principal: **Criar meu acesso**.

#### Tela 4 — Confirmação e consentimento

- Explicar em frases curtas quais dados são usados e para quê.
- Permitir abrir detalhes sem bloquear a compreensão básica.
- Consentimentos opcionais começam desmarcados.
- Ação principal: **Confirmar e continuar**.

#### Tela 5 — Entrada na configuração assistida

- Celebrar sem exagero: “Tudo certo com seu acesso”.
- Explicar: “Agora vamos preparar a plataforma para você”.
- Mostrar duração aproximada apenas se validada em testes.
- Ação principal: **Configurar agora**.
- Ação secundária: **Continuar depois**.

### 18.3 Retomada e exceções

- Se a pessoa fechar o App, voltar ao primeiro passo incompleto.
- Se já existir acesso, oferecer “Entrar” ou “Recuperar acesso” sem revelar se outra pessoa possui aquele dado.
- Se estiver sem conexão, explicar que o acesso não pôde ser concluído e preservar o que puder ser guardado com segurança.
- Após tentativas repetidas, oferecer ajuda do Tutor ou suporte apropriado.

### 18.4 Conclusão

O onboarding termina quando o Usuário possui acesso válido, compreende a promessa, sabe que controla a Publicação e entra na configuração assistida.

## 19. Fluxo de configuração assistida

### 19.1 Objetivo

Preparar o contexto mínimo para que as sugestões façam sentido e uma conta possa ser conectada sem exigir conhecimento sobre configurações técnicas.

### 19.2 Visão de progresso

Mostrar uma lista curta e persistente:

1. Sobre você.
2. O que você quer alcançar.
3. Para quem você fala.
4. Onde você quer publicar.

O usuário pode voltar a um passo concluído. Cada resposta é salva ao avançar.

### 19.3 Sequência completa

#### Passo 1 — Sobre você

- Confirmar como o Usuário quer ser chamado.
- Perguntar que tipo de trabalho realiza com opções exemplificadas e “Ainda não sei”.
- Criar o Projeto principal nos bastidores a partir desse contexto.
- Na interface, usar “Seu espaço” se “Projeto” não for compreendido nos testes.

#### Passo 2 — Objetivo

- Perguntar: “O que você mais quer conseguir com seus conteúdos?”
- Oferecer opções como “Compartilhar mensagens”, “Divulgar meu trabalho”, “Ensinar algo” e “Criar uma presença constante”.
- Permitir escrever uma resposta própria.
- Escolher uma opção preenche uma recomendação inicial, mas pode ser alterada.

#### Passo 3 — Público

- Perguntar: “Com quem você quer falar?”
- Oferecer exemplos coerentes com o segmento escolhido.
- Permitir “Ainda não tenho certeza”. Nesse caso, o Tutor ajuda com uma pergunta adicional, não bloqueia a jornada.

#### Passo 4 — Canal

- Perguntar: “Onde você quer publicar primeiro?”
- No MVP, mostrar somente o Canal suportado ou explicar de forma honesta se a opção desejada ainda não estiver disponível.
- Apresentar o benefício da conexão e o que a plataforma poderá fazer.
- Ação principal: **Conectar minha conta**.

#### Passo 5 — Permissão externa

- Antes de sair para o serviço externo, explicar: “Você será levado até [nome do Canal] para confirmar. A plataforma não verá sua senha”.
- Após retornar, mostrar o nome e a imagem pública da conta, quando disponíveis.
- Perguntar: “É esta a conta que você quer usar?”
- Ações: **Sim, continuar** e **Usar outra conta**.

#### Passo 6 — Pronto para começar

- Mostrar um resumo editável: objetivo, público e conta conectada.
- Informar claramente qualquer item pendente.
- Ação principal: **Criar meu primeiro conteúdo**.
- Ação secundária: **Ir para o início**.

### 19.4 Falhas e retomada

- Cancelamento no serviço externo retorna à explicação, sem marcar a conta como conectada.
- Permissão insuficiente é explicada como ação concreta: “Para publicar, precisamos que você permita…”.
- Conta expirada aparece como “Conecte novamente”, preservando Conteúdos.
- O usuário pode pular a conexão para experimentar a criação, mas não poderá Publicar até concluir esse passo.

## 20. Fluxo da tela principal

### 20.1 Objetivo

Responder rapidamente o que está acontecendo e conduzir para a melhor ação seguinte.

### 20.2 Primeira abertura do dia ou da sessão

O Tutor abre a experiência com:

> O que você quer fazer hoje?

Logo abaixo, mostrar ações adaptadas ao contexto:

- **Criar um conteúdo**;
- **Continuar de onde parei**, quando houver Rascunho;
- **Publicar um conteúdo**, quando houver item Pronto;
- **Ver resultados**, quando houver Publicação com dados;
- **Conectar minha conta**, quando houver pendência.

Se não houver histórico, priorizar “Criar meu primeiro conteúdo”. Se houver uma falha que impeça Publicação, mostrá-la antes de sugerir publicar.

### 20.3 Componentes da tela

1. **Saudação e pergunta do Tutor:** linguagem pessoal sem fingir intimidade.
2. **Próximo passo recomendado:** um cartão principal com motivo curto e uma ação.
3. **Seus conteúdos:** até três itens recentes com estado e ação correspondente.
4. **Publicações recentes:** sucesso, em andamento ou atenção necessária.
5. **Resultados disponíveis:** convite simples, sem excesso de números.
6. **Navegação principal:** Início, Criar, Biblioteca, Resultados e Ajuda.

### 20.4 Estados da tela principal

| Situação | Mensagem principal | Ação |
|---|---|---|
| Novo Usuário | “Vamos criar seu primeiro conteúdo?” | Começar |
| Configuração incompleta | “Falta uma etapa para deixar tudo pronto” | Continuar configuração |
| Rascunho existente | “Você pode continuar o conteúdo que começou” | Continuar |
| Conteúdo pronto | “Seu conteúdo está pronto para revisão” | Revisar e publicar |
| Publicação em andamento | “Estamos enviando seu conteúdo” | Ver andamento |
| Publicação com falha | “Não foi possível publicar. Seu conteúdo está salvo” | Resolver |
| Resultado disponível | “Já há resultados para você ver” | Ver resultados |

## 21. Fluxo para criação de Conteúdo

### 21.1 Entrada

O fluxo pode começar pela tela principal, pelo botão Criar ou por uma recomendação do Tutor. Antes do primeiro campo, explicar: “Vamos fazer uma etapa de cada vez. Você poderá revisar tudo antes de publicar”.

### 21.2 Sequência completa

#### Passo 1 — Título e aviso

- Pedir um título interno para localização.
- Perguntar: “Qual aviso você quer publicar?”
- Aceitar texto simples, sem formatação rica ou arquivo.
- Ação principal: **Continuar**.

#### Passo 2 — Contexto predefinido

- Usar **Publicar avisos da igreja** como objetivo e **comunidade da igreja** como público.
- Não pedir essas informações novamente no primeiro fluxo.
- Permitir correção posterior nas Configurações, sem bloquear o aviso.

#### Passo 3 — Salvamento

- Criar o Rascunho quando o primeiro texto não vazio for confirmado.
- Mostrar “Salvo” somente depois da persistência.
- Não gerar, reescrever ou sugerir texto.

#### Passo 4 — Editar

- Mostrar um editor simples com texto legível e ações essenciais.
- Evitar barras com muitas ferramentas.
- Salvar continuamente e mostrar “Salvo” após confirmação.
- Destacar uma pendência por vez.
- Tutor explica a etapa e permite voltar à última versão confirmada; não reescreve o aviso.

#### Passo 5 — Validar texto

- Verificar texto obrigatório e limite comprovado do Canal.
- Mostrar como corrigir texto vazio ou excessivo.
- Mídia, upload e armazenamento não aparecem no primeiro fluxo.

#### Passo 6 — Revisão do Conteúdo

- Exibir o texto e a Página escolhida sem simular detalhes que o Canal não garante.
- Organizar pendências em: “Precisa corrigir” e “Você pode melhorar”.
- Correções obrigatórias bloqueiam o avanço e levam ao ponto correto.
- Melhorias opcionais nunca impedem salvar ou publicar.
- Ações: **Está pronto** e **Continuar editando**.

#### Passo 7 — Próximo destino

- Ao marcar como Pronto, oferecer **Publicar agora** ou **Guardar na Biblioteca**.
- Se não houver Conta Conectada, oferecer **Conectar para publicar** ou **Guardar por enquanto**.

### 21.3 Retomada

Ao voltar a um Rascunho, mostrar a última etapa concluída, a hora do último salvamento e uma ação **Continuar**. Se houver mudanças em conflito, preservar ambas as versões e pedir uma escolha explicada.

## 22. Fluxo para organização da Biblioteca

### 22.1 Objetivo

Permitir encontrar e entender Conteúdos sem exigir conhecimento sobre pastas, arquivos ou estados internos.

### 22.2 Tela da Biblioteca

- Título: “Seus conteúdos”.
- Ação principal: **Criar novo**.
- Ordenação padrão: atualização mais recente.
- Cada cartão mostra título, pequena prévia, última atualização, estado em linguagem simples e uma única ação sugerida.

### 22.3 Ações sobre um Conteúdo

| Situação | Ação principal | Ações secundárias |
|---|---|---|
| Ideia ou Rascunho | Continuar | Arquivar |
| Precisa de revisão | Corrigir | Ver pendências ou arquivar |
| Pronto | Revisar e publicar | Editar ou arquivar |
| Em publicação | Ver andamento | Voltar à Biblioteca |
| Publicado | Ver publicação | Arquivar |
| Com falha | Resolver | Ver o que aconteceu ou arquivar |
| Arquivado | Ver histórico | Nenhuma no primeiro fluxo |

### 22.4 Organização segura

- Arquivar exige confirmação simples e explica: “O conteúdo sairá da lista principal, mas não será apagado”.
- Excluir, quando disponível, explica o impacto sobre histórico antes de confirmar.
- Biblioteca vazia explica o valor e oferece **Criar meu primeiro conteúdo**.
- Busca, filtros, renomeação, duplicação, restauração, exclusão e ações em massa ficam fora do primeiro fluxo.

## 23. Fluxo para Publicação

### 23.1 Entrada

O fluxo começa em um Conteúdo Pronto, na Biblioteca ou pela ação recomendada na tela principal.

### 23.2 Sequência completa

#### Passo 1 — Verificar se está tudo pronto

- Conferir Conta Conectada, campos e mídia.
- Mostrar uma pendência por vez com ação **Corrigir agora**.
- Se a conta precisar de nova conexão, preservar a versão e guiar a correção.

#### Passo 2 — Escolher o destino

- Mostrar nome do Canal, nome e imagem pública da conta.
- Perguntar: “Onde você quer publicar?”
- No MVP, selecionar previamente o único destino ativo, permitindo confirmá-lo.

#### Passo 3 — Prévia final

- Mostrar texto e mídia da versão que será enviada.
- Exibir avisos sobre ajustes feitos para o Canal.
- Informar: “Depois de publicar, mudanças feitas aqui não alteram o que já foi enviado”.
- Ações: **Continuar para publicar** e **Voltar para editar**.

#### Passo 4 — Confirmação

- Resumir “O que”, “Onde” e “Quando”.
- Usar a pergunta: “Você quer publicar este conteúdo agora?”
- Ação principal inequívoca: **Sim, publicar agora**.
- Ação secundária: **Ainda não**.
- Fechar ou voltar nunca confirma a Publicação.

#### Passo 5 — Envio

- Mostrar “Publicando seu conteúdo…” e permitir sair sem cancelar silenciosamente.
- Explicar que o andamento ficará disponível na tela principal e na Biblioteca.
- Evitar exibir porcentagem quando não houver progresso real mensurável.

#### Passo 6 — Resultado

- Sucesso: “Seu conteúdo foi publicado”, com **Ver publicação** e **Voltar ao início**.
- Aguardando confirmação: “O envio foi feito. Estamos confirmando a publicação”.
- Falha conhecida: “Não conseguimos publicar. Seu conteúdo está salvo”, motivo simples e **Tentar novamente** ou **Corrigir**.
- Resultado incerto: “Ainda não sabemos se o Canal concluiu. Vamos verificar antes de tentar de novo”.

### 23.3 Regras de confiança

- Nunca enviar duas vezes por repetição de toque.
- Não prometer que cancelar localmente remove algo já publicado.
- Não esconder adaptações feitas no Conteúdo.
- Manter acesso à versão enviada e ao destino confirmado.

## 24. Fluxo para acompanhamento dos resultados

### 24.1 Entrada

Resultados pode ser aberto pela navegação, pelo Conteúdo Publicado, pela tela de sucesso ou por aviso de novos dados.

### 24.2 Entrada no resultado

- Abrir o detalhe pelo sucesso, Início ou Conteúdo.
- Mostrar primeiro Página, horário, estado confirmado e link externo.
- Não implementar lista geral, período selecionável ou filtros no primeiro fluxo.

### 24.3 Detalhe de um Conteúdo

1. Mostrar a Publicação escolhida e onde ela ocorreu.
2. Explicar quando os dados foram atualizados.
3. Apresentar no máximo uma métrica externa comprovada, com nome cotidiano e explicação.
4. Mostrar ausência de dados como “Ainda não há informações suficientes”, não como zero quando isso não for verdade.
5. Separar fatos de recomendações.

Exemplos de tradução:

| Nome apresentado | Explicação |
|---|---|
| Pessoas alcançadas | Quantas pessoas receberam ou viram o conteúdo, conforme o Canal informa |
| Visualizações | Quantas vezes o conteúdo foi visto; a mesma pessoa pode contar mais de uma vez |
| Interações | Ações como curtidas, comentários ou compartilhamentos disponíveis |

### 24.4 Aprendizado

Depois dos fatos, o Tutor apresenta uma leitura curta:

- “O que aconteceu”: Publicação confirmada, falhou ou ainda está sendo verificada.
- “O que ainda não sabemos”: atraso, ausência ou limite da informação.
- “Próximo passo”: abrir a Publicação, corrigir a falha ou voltar ao Início.

Recomendação editorial baseada em desempenho não entra no primeiro fluxo.

### 24.5 Estados especiais

- Sem Publicações: explicar que resultados aparecem depois de Publicar e oferecer **Criar conteúdo**.
- Dados ainda não disponíveis: informar que alguns Canais demoram e permitir voltar depois.
- Atualização falhou: manter o último valor com data e dizer que não foi possível atualizar agora.
- Publicações múltiplas futuras: manter cada destino separado antes de qualquer total consolidado.

## 25. Fluxo de ajuda do Tutor

### 25.1 Regra de abertura

Em toda nova abertura do Tutor, independentemente da tela, a primeira mensagem é exatamente:

> O que você quer fazer hoje?

O Tutor pode então mostrar sugestões relacionadas à tela atual, sempre mantendo a possibilidade de o Usuário escrever com suas próprias palavras.

### 25.2 Sugestões por contexto

| Contexto | Opções iniciais |
|---|---|
| Primeiro acesso | Conhecer a plataforma; criar meu acesso; entrar |
| Configuração | Entender esta etapa; conectar minha conta; continuar depois |
| Início | Criar conteúdo; continuar um conteúdo; publicar; ver resultados |
| Criação | Organizar minha ideia; melhorar o texto; entender uma pendência |
| Biblioteca | Encontrar um conteúdo; entender os estados; recuperar um arquivado |
| Publicação | Conferir o destino; corrigir uma pendência; entender o que aconteceu |
| Resultados | Entender um número; saber o que fazer depois; escolher outro conteúdo |

### 25.3 Forma de condução

1. Reconhecer o objetivo com uma frase curta.
2. Fazer somente uma pergunta por vez quando faltar informação.
3. Reutilizar o contexto já informado e não pedir o mesmo dado novamente sem motivo.
4. Explicar a ação antes de encaminhar o Usuário.
5. Oferecer uma ação principal e, quando necessário, uma alternativa segura.
6. Confirmar conclusão e informar o próximo passo.

### 25.4 Limites

O Tutor pode:

- explicar telas, estados, permissões e resultados;
- levar o Usuário até a etapa correta;
- sugerir estruturas, melhorias e próximos passos;
- recuperar o contexto de uma tarefa interrompida;
- registrar aceitação, adaptação ou rejeição de uma recomendação.

O Tutor não pode:

- conectar uma conta sem o Usuário concluir a autorização;
- aceitar consentimentos;
- substituir silenciosamente o Conteúdo;
- publicar, reagendar, remover ou desconectar por conta própria;
- inventar resultados ou esconder limitações;
- prometer alcance, vendas ou crescimento.

### 25.5 Quando o Tutor não entende

- Dizer: “Não entendi o que você precisa ainda”.
- Reformular com opções simples, sem culpar o Usuário.
- Após nova dificuldade, oferecer voltar à tela anterior ou acessar suporte.
- Preservar tudo o que já foi salvo.

## 26. Padrões de mensagens e estados

### 26.1 Mensagem de erro

Toda mensagem deve responder:

- o que aconteceu em linguagem simples;
- se o trabalho está salvo;
- o que a pessoa pode fazer agora.

Exemplo: “Sua conta precisa ser conectada novamente. Seu conteúdo está salvo. Conecte a conta para continuar a publicação.”

### 26.2 Confirmação

A confirmação deve nomear a ação e o objeto. Evitar botões genéricos como “OK” em ações importantes. Preferir “Arquivar conteúdo”, “Desconectar conta” ou “Sim, publicar agora”.

### 26.3 Espera

Durante espera, informar a tarefa real. Se o Usuário puder sair, dizer isso. Se não houver estimativa confiável, não inventar tempo ou percentual.

### 26.4 Estado vazio

Explicar por que a área está vazia e oferecer uma ação útil. Nunca apresentar apenas uma tela em branco ou “Nenhum registro encontrado”.

### 26.5 Acessibilidade e confiança

- Usar texto legível, alvos de toque confortáveis e contraste suficiente.
- Manter botões no mesmo lugar ao longo de etapas semelhantes.
- Associar instruções e erros aos campos correspondentes.
- Permitir navegação por teclado e leitura por tecnologia assistiva.
- Evitar contagens regressivas e pressão artificial.
- Não usar culpa, urgência ou recompensa enganosa para incentivar Publicação.

## 27. Critério de experiência completa

A experiência está completa quando um Usuário iniciante consegue entrar, configurar seu contexto, conectar uma conta, criar e encontrar um Conteúdo, revisar e confirmar uma Publicação, entender o resultado e escolher um próximo passo sugerido pelo Tutor, sempre sabendo o que foi salvo, o que aconteceu e como voltar ou pedir ajuda.

## 28. Primeiro fluxo vertical da Sprint 6

Esta seção especializa o caminho do MVP. Capacidades mais amplas descritas nas seções anteriores são evolução e não entram no primeiro fluxo quando não estiverem listadas abaixo.

### 28.1 Usuário, dor e objetivo

- **Primeiro Usuário:** pastor ou líder de pequena igreja, responsável direto pela comunicação, com baixa ou média confiança digital e acesso administrativo legítimo a uma Página do Facebook.
- **Dor:** ele já possui o aviso, mas teme perder o texto, escolher a conta errada, publicar sem perceber ou repetir o envio quando a ferramenta não confirma o resultado.
- **Primeiro objetivo:** publicar imediatamente um aviso textual curto na Página correta e saber com confiança se e onde ele foi publicado.

O segmento, o comportamento e o uso do Facebook Pages permanecem hipóteses até entrevista, teste com representante e prova oficial da Integração.

### 28.2 Menor fluxo de valor

1. Receber convite e entrar no Web com código por e-mail.
2. Confirmar nome, nome da igreja e consentimentos essenciais.
3. Abrir o Tutor, que pergunta “O que você quer fazer hoje?”, e escolher **Publicar um aviso**.
4. Ler permissões, autorizar o Canal e confirmar uma Página elegível.
5. Informar título interno e texto do aviso; receber confirmação de salvamento.
6. Reabrir o Rascunho pela Biblioteca simples quando houver interrupção.
7. Revisar texto e Página e escolher **Sim, publicar agora**.
8. Acompanhar o envio assíncrono sem precisar permanecer na tela.
9. Ver sucesso, falha recuperável ou resultado incerto; em sucesso, abrir o link externo.
10. Ver o próximo passo do Tutor e, se a API comprovar disponibilidade, uma única métrica simples com fonte e atualização.

### 28.3 Telas participantes

- Acesso por convite e código.
- Configuração mínima e conexão da Página.
- Início / Tutor.
- Criar aviso.
- Biblioteca simples.
- Revisão e confirmação.
- Andamento da Publicação.
- Resultado da Publicação.

### 28.4 Entidades participantes

- Usuário e participação de proprietário.
- Projeto.
- Biblioteca de Conteúdos como projeção.
- Conteúdo e Versão de Conteúdo.
- Canal, Conta Conectada e Integração.
- Publicação e Tentativa de Publicação.
- Métrica de Publicação e Relatório simples, somente na profundidade disponível.
- Interação e recomendação determinística do Tutor.
- Eventos de auditoria e trabalho durável relacionados ao fluxo.

Mídia, Agendamento e Campanha não participam.

### 28.5 Integrações necessárias

- **Amazon Cognito:** entrada e sessão, conforme decisão já aprovada.
- **Facebook Pages:** autorização da Página, Publicação textual, consulta/reconciliação de estado, revogação e obtenção de link; leitura de uma métrica somente se a prova oficial confirmar necessidade, permissão e disponibilidade.

Webhook não é obrigatório se consulta de estado for suficiente. Armazenamento conectado, IA, notificações, analytics externo e qualquer segundo Canal não são necessários.

### 28.6 Informações armazenadas

- referência de identidade, nome, e-mail normalizado quando necessário, consentimentos e estado do acesso;
- nome, objetivo padrão, público padrão e fuso do Projeto;
- Canal, identificador e nome público da Página, permissões, saúde, vencimento e referência protegida da credencial;
- título interno, texto, estado, versão atual e instantes do Conteúdo;
- Versão congelada, confirmação, chave de idempotência, estado, tentativas, erro sanitizado, identificador, link e horários da Publicação;
- fonte, período, valor e coleta de uma métrica externa, se adotada;
- etapa da jornada, próximo passo e resposta à recomendação do Tutor;
- eventos mínimos de auditoria, correlação e funil, sem corpo do aviso, código, token ou texto livre em logs.

### 28.7 Métricas de sucesso

- primeira Publicação real confirmada na Página correta: sim ou não;
- conclusão do fluxo sem intervenção humana durante a tarefa;
- tempo da primeira entrada até a confirmação da Publicação;
- conclusão, abandono e erro em entrada, conexão, salvamento, confirmação e envio;
- Rascunho recuperado sem perda depois de interrupção;
- compreensão correta das permissões e da Página antes de confirmar;
- compreensão correta do estado, link e próximo passo depois de publicar;
- intenção declarada de usar o fluxo no próximo aviso;
- Publicações sem intenção, duplicadas ou destinadas à Página errada: meta obrigatória zero.

### 28.8 Primeira versão utilizável

A primeira versão é utilizável quando um pastor convidado completa esse fluxo real sem intervenção durante a tarefa, encontra a Publicação na Página correta e explica o que aconteceu e o próximo passo. Também precisa atender integralmente ENG-15: prova oficial do Canal, acessibilidade, proteção de sessão e token, idempotência, reconciliação, auditoria, privacidade, restauração, alertas, suporte e ausência de risco crítico sem tratamento.
