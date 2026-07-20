# Produto Mínimo Viável

**Versão:** 0.4

**Fase:** Sprint 6 — Primeiro Fluxo Vertical do MVP

**Atualizado em:** 20 de julho de 2026

## 1. Objetivo do MVP

Validar se um pastor responsável pela comunicação de uma pequena igreja, com pouca familiaridade tecnológica e sem apoio especializado, consegue publicar com segurança um aviso textual em uma Página do Facebook que já administra e confirmar o que aconteceu.

Esse é o menor fluxo que realiza uma ação externa útil, preserva o controle do Usuário e testa a proposta central. Facebook Pages é a hipótese operacional da Sprint 6; a liberação para piloto depende da prova oficial da Integração e não fecha por si só OD-01 ou OD-02.

## 2. Hipótese principal

Se o Tutor conduzir esse pastor desde a entrada até a confirmação de uma única Publicação, mostrando o texto e a Página de destino antes da ação, então ele conseguirá publicar um aviso real sem ajuda durante a tarefa, sem duplicidade e sabendo onde verificar o resultado.

## 3. Princípio do escopo

O **Conteúdo é a entidade central do MVP**. A configuração prepara o contexto do Conteúdo; a Biblioteca o organiza; o Canal e a Conta Conectada permitem sua distribuição; a Publicação registra o envio; o Relatório apresenta seu resultado; e o Tutor transforma esse resultado em orientação.

### 3.1 Baseline de engenharia

A primeira versão utilizável será Web responsiva. Usará TypeScript e Node.js 24 LTS, Next.js no Frontend, NestJS na API e no worker, PostgreSQL com Prisma, Amazon Cognito para identidade, outbox e fila no PostgreSQL, e serviços gerenciados da AWS na região de São Paulo. IA generativa e aplicativo Mobile ficam fora dessa versão. As justificativas, riscos e reversibilidade estão em `APPROVED-DECISIONS.md`.

## 4. Primeiro Usuário

O primeiro Usuário é um **pastor ou líder de uma pequena igreja que cuida pessoalmente da comunicação**, possui baixa ou média confiança digital, já administra uma Página do Facebook da igreja e precisa publicar avisos recorrentes sem depender de um profissional ou voluntário técnico.

Esse recorte é uma hipótese de produto a validar com entrevistas e teste real. O primeiro participante deve ter autorização legítima sobre a Página e aceitar publicar um aviso controlado.

### 4.1 Dor exata

Ele já sabe o que precisa comunicar, mas precisa alternar entre ferramentas e configurações que não domina. Isso gera medo de publicar o texto errado, usar a conta errada, perder o rascunho ou repetir a Publicação ao não receber retorno claro.

### 4.2 Primeiro objetivo

Publicar agora um aviso textual curto na Página correta da igreja e receber confirmação confiável, sem precisar conhecer permissões, API, token, formato técnico ou estado interno.

## 5. Primeiro fluxo vertical

```text
Acessar por convite e entrar com código por e-mail
→ Confirmar nome e espaço da igreja
→ Escolher “Publicar um aviso” no Tutor
→ Conectar e confirmar uma Página do Facebook
→ Escrever título interno e texto do aviso
→ Salvar e revisar a prévia
→ Confirmar “Sim, publicar agora”
→ Acompanhar o envio
→ Ver confirmação, link e resultado disponível
```

### 5.1 Acessar e configurar o mínimo

**Entrega mínima:** acesso Web por convite e código de uso único enviado por e-mail; confirmação do nome; criação automática de um Projeto com o nome da igreja; consentimentos essenciais.

**Resultado esperado:** Usuário entra, entende que nada será publicado sem confirmação e chega ao Tutor.

### 5.2 Escolher a intenção

**Entrega mínima:** Tutor abre com “O que você quer fazer hoje?” e oferece **Publicar um aviso** como ação principal.

**Resultado esperado:** a jornada registra uma intenção permitida e mostra as etapas restantes.

### 5.3 Conectar a Página

**Entrega mínima:** explicar o benefício e as permissões; conduzir a autorização oficial; listar somente Páginas administradas elegíveis; pedir confirmação da Página escolhida; tratar cancelamento, falha, revogação e reconexão.

**Resultado esperado:** existe uma Conta Conectada ativa e o Usuário reconhece a Página que receberá o aviso.

### 5.4 Criar e salvar o aviso

**Entrega mínima:** pedir apenas um título interno e o texto do aviso; salvar um Rascunho textual; permitir editar, sair e retomar; validar somente campos e limite do Canal.

**Resultado esperado:** existe uma Versão editável e recuperável, sem mídia, sugestão gerada ou Publicação implícita.

### 5.5 Revisar e confirmar

**Entrega mínima:** mostrar o texto exato, o nome da Página e que a Publicação será imediata; permitir voltar à edição; exigir **Sim, publicar agora**.

**Resultado esperado:** a confirmação vincula Usuário, Projeto, Conteúdo, Versão e Conta Conectada; fechar ou voltar não publica.

### 5.6 Publicar e acompanhar

**Entrega mínima:** congelar a Versão, criar Publicação e outbox na mesma transação, processar pelo worker e apresentar aguardando, publicando, publicada, falhou ou resultado incerto.

**Resultado esperado:** Publicação real confirmada ou falha compreensível e recuperável, sem duplicidade nem perda do Conteúdo.

### 5.7 Ver o resultado

**Entrega mínima:** mostrar Página, horário, estado confirmado e link externo. Quando a prova do Canal confirmar uma métrica simples disponível, mostrar somente essa métrica com fonte, período e última atualização; ausência ou atraso nunca aparece como zero.

**Resultado esperado:** Usuário sabe se o aviso foi publicado, onde encontrá-lo e qual é o próximo passo seguro.

### 5.8 Retomar

**Entrega mínima:** Início e Biblioteca simples mostram o aviso e sua única ação válida: continuar, revisar, ver andamento, resolver ou ver publicação.

**Resultado esperado:** interrupção não perde o texto nem provoca nova Publicação.

## 6. Entidades no MVP

| Entidade | Uso no MVP | Profundidade inicial |
|---|---|---|
| Usuário | Identidade e entrada por convite | Um papel: proprietário |
| Projeto e participação | Espaço da igreja e isolamento | Um Projeto criado automaticamente |
| Conteúdo | Aviso textual central | Título interno, texto e estado |
| Versão de Conteúdo | Rascunho e texto congelado | Uma versão editável e versões congeladas por Publicação |
| Biblioteca de Conteúdos | Retomada e localização | Lista simples, abertura e arquivo |
| Canal | Hipótese Facebook Pages | Um Canal sujeito à prova técnica |
| Conta Conectada | Página autorizada | Uma Página ativa no caminho principal |
| Integração | Autorização, publicação, estado e resultado | Cognito e um adaptador Facebook Pages |
| Publicação | Envio e resultado por destino | Publicação imediata e tentativas rastreáveis |
| Métrica e Relatório | Confirmação e resultado mínimo | Uma visão por Publicação; uma métrica somente se provada |
| Tutor | Orientação do ciclo | Intenções e mensagens determinísticas, sem IA generativa |

Mídia, Agendamento e Campanha não participam do fluxo vertical.

## 7. Capacidades obrigatórias

### Experiência e tutoria

- entrada por convite, progresso e próximo passo;
- linguagem simples e ajuda contextual;
- salvamento e retomada;
- estados vazios, carregamento, sucesso e erro compreensíveis;
- acessibilidade do caminho principal.

### Conteúdo e Biblioteca

- criar, editar, salvar, reabrir, revisar e arquivar aviso textual;
- preservar versão usada em Publicação;
- validar texto obrigatório e limite confirmado pelo Canal;
- listar Conteúdos por atualização, sem busca ou filtros avançados;
- impedir que salvar ou marcar Pronto publique algo.

### Canal, Conta e Integração

- explicar permissões antes da autorização;
- conectar, verificar, reconectar e desconectar;
- proteger tokens e demais segredos;
- conhecer texto, limites, estados e resultado suportados;
- traduzir falhas técnicas em ações possíveis.

### Publicação e resultado

- mostrar Conteúdo, versão, Canal e conta na prévia;
- exigir confirmação explícita;
- usar chave de idempotência e consultar envios incertos;
- guardar tentativas e referência externa;
- coletar no máximo uma métrica essencial comprovada;
- informar período, fonte e atualização;
- recomendar sem prometer causalidade ou desempenho.

## 8. Cenário de sucesso do piloto

1. Um pastor convidado acessa o Web e entra com código por e-mail sem ajuda durante a tarefa.
2. Confirma seu nome e o espaço da igreja e escolhe **Publicar um aviso**.
3. Compreende as permissões, conecta e reconhece a Página correta.
4. Escreve e salva um aviso textual curto.
5. Sai ou navega à Biblioteca e reabre o mesmo Rascunho sem perda.
6. Revisa texto e destino e confirma conscientemente uma Publicação real.
7. Recebe estado confirmado e abre o link da Publicação, sem duplicidade.
8. Entende o próximo passo apresentado pelo Tutor.

## 9. Critérios de aceite do ciclo

- Nenhuma etapa exige que o Usuário conheça termos internos de API ou integração.
- Interrupções antes da confirmação não geram Publicação.
- Rascunho é preservado em falhas de navegação ou rede cobertas pelo piloto.
- Conta e permissões são identificadas antes de publicar.
- A versão publicada pode ser auditada e não muda com edições posteriores.
- Resultado de envio incerto não causa repetição automática insegura.
- Falha mostra uma ação de recuperação sem expor dados sensíveis.
- Relatório mostra fonte, período e atualização das métricas.
- Tutor explica e recomenda, mas não publica nem altera Conteúdo sozinho.
- Usuário pode desconectar a Conta sem perder Conteúdos e histórico local.

## 10. Métricas do piloto

### Ativação

- primeiro Usuário entra, conecta a Página, salva o Rascunho e conclui uma Publicação real: sim ou não;
- taxa de convidados que concluem cada marco sem intervenção durante a tarefa;
- tempo entre a primeira entrada e a Publicação confirmada;
- abandono e erro em entrada, conexão, salvamento, confirmação e envio.

### Autonomia e confiança

- conclusão de cada etapa sem ajuda externa;
- intervenção humana necessária durante a tarefa e erros por etapa;
- compreensão de permissões e destino antes da confirmação;
- percepção de confiança para publicar;
- compreensão correta do estado, link publicado e próximo passo;
- número de Publicações duplicadas ou realizadas sem intenção, cuja meta obrigatória é zero.

### Valor contínuo

- retorno para abrir a confirmação ou o resultado;
- intenção de usar o fluxo para o próximo aviso;
- redução percebida de insegurança e dependência de terceiros;
- recomendação simples do Tutor compreendida e aceita ou recusada conscientemente.

Metas numéricas serão definidas após testes moderados criarem uma linha de base. Volume bruto de publicações não comprova, sozinho, autonomia ou valor.

## 11. Requisitos não funcionais mínimos

- proteção de dados pessoais e credenciais por padrão;
- consentimento e revogação compreensíveis;
- navegação por teclado, foco visível, rótulos e contraste adequados;
- resposta adequada às condições de rede do público do piloto;
- observabilidade de conexão, salvamento e Publicação sem registrar segredos;
- trilha de auditoria para autorização e ações externas;
- recuperação de falhas sem perda silenciosa;
- instrumentação dos marcos do fluxo com minimização de dados.

## 12. Fora do MVP

- cadastro público irrestrito;
- tipos de Conteúdo além de aviso textual;
- imagem, áudio, vídeo, documento, upload e armazenamento conectado;
- geração, reescrita ou adaptação de texto por IA;
- Campanhas;
- colaboração e múltiplos papéis;
- múltiplos Projetos por Usuário na interface;
- múltiplos Canais e distribuição simultânea;
- calendário editorial completo e recorrência;
- Agendamento;
- busca, filtros avançados, etiquetas, duplicação e ações em massa na Biblioteca;
- notificações externas;
- edição profissional de imagem, áudio ou vídeo;
- geração ou publicação totalmente autônoma;
- métricas avançadas, comparação e relatório multicanal;
- mídia paga, CRM, comércio eletrônico ou garantia de desempenho.

## 13. Dependências e riscos

| Dependência ou risco | Tratamento antes do piloto |
|---|---|
| Web responsivo não atender ao contexto do público | Testar primeiro acesso, código por e-mail e retorno de autorização nos dispositivos reais |
| Hipótese de Facebook Pages ou texto sem mídia ser inválida | Entrevistar o público e provar a Integração antes do piloto |
| Revisão e limites da API | Fazer prova de autorização, publicação, estado e métricas |
| Confiança para conectar contas | Testar explicação de permissões e revogação |
| Tutor determinístico não compreender texto livre | Voltar a opções guiadas, medir falhas e não ampliar regras sem evidência |
| Métricas demorarem ou variarem | Definir expectativa, atualização e estado parcial do Relatório |
| Publicação duplicada | Implementar idempotência e reconciliação antes do piloto real |
| Dificuldade com acesso ou retorno ao Web | Testar distribuição, favoritos, sessão e primeiro acesso com dispositivos reais |

## 14. Critérios de entrada em desenvolvimento

A implementação do núcleo pode começar com as decisões da Sprint 5. Os primeiros incrementos devem usar adaptadores simulados e dados fictícios enquanto as decisões dependentes de descoberta permanecem abertas.

- baseline técnica aceita e refletida no backlog de implementação;
- monólito modular dividido em Web, API e worker;
- contratos de identidade, Canal, armazenamento e assistência substituíveis por simuladores;
- autorização por Projeto, idempotência, auditoria, logs seguros e acessibilidade incluídos nos critérios de cada incremento;
- desenvolvimento local reproduzível sem credencial ou dado produtivo;
- estados e erros do domínio usados como fonte única para a experiência.

Validação do pastor e do aviso textual, prova de Facebook Pages, resultado mínimo, retenção e metas operacionais não bloqueiam o fluxo simulado. Eles bloqueiam a conexão real e a liberação para piloto.

## 15. Critérios para a primeira versão utilizável

“Primeira versão utilizável” significa uma versão liberável para piloto controlado, e não apenas uma demonstração técnica. Todos os critérios são cumulativos:

- a hipótese de pastor, aviso textual e Facebook Pages foi validada com representante real e prova oficial do Canal;
- acesso Web por código de e-mail, sessão protegida e encerramento testados;
- convite, configuração mínima, Tutor determinístico, criação textual, retomada e Biblioteca simples funcionam no ciclo completo;
- conexão, verificação, revogação e reconexão de uma Página real controlada foram comprovadas;
- prévia mostra Versão, Canal e conta, e somente a confirmação explícita cria a Publicação;
- Publicação real é assíncrona, idempotente, auditável e reconciliável em sucesso, timeout e falha;
- Resultado mostra Página, link, estado, fonte, horário, atualização e próximo passo; métrica externa só aparece se comprovada;
- tokens, segredos, logs e isolamento por Projeto atendem à baseline de segurança;
- acessibilidade do caminho principal, migração, restauração, alertas e revogação de token foram verificados em homologação;
- privacidade, retenção, exclusão, suporte e resposta a incidente estão aprovados;
- pelo menos um pastor conclui o fluxo real sem intervenção durante a tarefa e explica corretamente destino, resultado e próximo passo;
- não ocorre perda silenciosa, Publicação duplicada ou Publicação sem intenção;
- não existe risco crítico de segurança sem tratamento aprovado.

Esses critérios especializam, sem substituir ou reduzir, ENG-15 de `APPROVED-DECISIONS.md`. O fluxo não está utilizável para piloto enquanto a prova do Facebook Pages, a governança e os controles exigidos por ENG-15 permanecerem incompletos.

Os limiares quantitativos devem ser aprovados antes do piloto. Caso os sinais não sejam atingidos, a decisão correta pode ser revisar segmento, linguagem, jornada, Canal ou proposta de valor em vez de ampliar funcionalidades.

## 16. Experiência mínima entregável

O MVP deve parecer uma única jornada, mesmo que diferentes partes do sistema realizem as tarefas. O Usuário não precisa conhecer as entidades internas nem decidir configurações que a plataforma pode recomendar com segurança.

### 16.1 Navegação mínima

- **Início:** situação atual e próximo passo.
- **Criar:** edição do aviso textual.
- **Biblioteca:** lista, retomada e arquivo dos avisos.
- **Resultados:** confirmação e detalhe da Publicação escolhida.
- **Ajuda:** acesso permanente ao Tutor.
- **Configurações:** perfil e Página conectada, acessível sem competir com a tarefa principal.

### 16.2 Regra do Tutor

Toda vez que o Tutor for aberto, inclusive no primeiro acesso e na tela principal, sua primeira pergunta será exatamente:

> O que você quer fazer hoje?

Depois da pergunta, o Tutor apresenta no máximo quatro opções coerentes com o contexto e aceita uma resposta escrita. Ele faz uma pergunta por vez, não repete dados já conhecidos sem necessidade e nunca executa Publicação, conexão ou consentimento pelo Usuário.

### 16.3 Padrão de etapa

Cada etapa do MVP inclui título, motivo curto, progresso quando aplicável, uma ação principal, alternativa segura, ajuda e retorno visível de salvamento ou resultado. Opções avançadas não aparecem no caminho principal.

### 16.4 Telas do fluxo vertical

| Tela | Responsabilidade mínima |
|---|---|
| Acesso | Receber convite, e-mail e código de uso único |
| Configuração mínima | Confirmar nome, igreja, consentimentos e Página |
| Início / Tutor | Mostrar “O que você quer fazer hoje?” e **Publicar um aviso** |
| Criar aviso | Editar título interno e texto, salvar e retomar |
| Biblioteca | Listar, abrir e arquivar avisos |
| Revisão e confirmação | Mostrar texto, Página e ação **Sim, publicar agora** |
| Andamento da Publicação | Mostrar espera, sucesso, falha ou resultado incerto |
| Resultado da Publicação | Mostrar Página, horário, link, atualização e próximo passo |

## 17. Escopo detalhado dos fluxos

### 17.1 Onboarding

**Telas mínimas:**

1. Acesso por convite, com promessa curta e garantia de confirmação antes de Publicar.
2. Entrada por código de e-mail e consentimentos essenciais em linguagem simples.

**Critérios de aceite:**

- A primeira tela identifica o convite e permite entrar ou sair.
- O Usuário entende que nada será publicado sozinho.
- Código inválido ou vencido pode ser solicitado novamente sem revelar indevidamente a existência de conta.
- Campos mostram requisitos antes do envio e erros junto ao campo.
- Consentimento opcional não começa selecionado.
- Conclusão leva diretamente à configuração assistida.

### 17.2 Configuração assistida

**Passos mínimos:** nome, nome da igreja, consentimentos pendentes, explicação do Canal, conexão e confirmação da Página.

**Critérios de aceite:**

- O progresso informa o passo atual e o total.
- O objetivo inicial é **Publicar avisos da igreja** e o público inicial é **comunidade da igreja**; ambos podem ser corrigidos nas Configurações sem criar nova etapa no fluxo.
- Um Projeto com o nome da igreja é criado sem exigir que o Usuário conheça esse conceito.
- Antes da autorização externa, a interface explica o que acontecerá e informa que não verá a senha do Canal.
- Ao retornar, nome e imagem pública da conta são mostrados para confirmação quando disponíveis.
- Cancelar a autorização não registra sucesso nem perde respostas anteriores.
- A conexão pode ser pulada para Criar, mas Publicar explica a pendência.
- O resumo final permite corrigir nome da igreja ou Página.

### 17.3 Tela principal

**Componentes mínimos:** pergunta do Tutor, próximo passo recomendado, Conteúdos recentes, estado de Publicações e acesso à navegação.

**Critérios de aceite:**

- O Tutor começa com “O que você quer fazer hoje?”.
- A tela oferece apenas ações possíveis para o estado atual.
- Novo Usuário recebe “Criar meu primeiro conteúdo”.
- Rascunho recente recebe “Continuar de onde parei”.
- Conteúdo Pronto recebe “Revisar e publicar”.
- Falha de Publicação aparece como prioridade e informa que o Conteúdo está salvo.
- Resultado disponível leva ao Conteúdo e ao período corretos.
- Voltar de outra área atualiza a recomendação sem exigir recarregamento manual.

### 17.4 Criação de Conteúdo

**Passos mínimos:** título interno, texto do aviso, salvamento, revisão e decisão de Publicar ou guardar.

**Critérios de aceite:**

- O fluxo informa que haverá revisão antes da Publicação.
- O primeiro texto não vazio cria um Rascunho recuperável.
- Editor mostra confirmação real de salvamento.
- Usuário consegue voltar à versão anterior suportada pelo MVP.
- Não existe entrada de mídia, sugestão gerada ou formatação rica.
- Texto vazio ou acima do limite comprovado bloqueia a revisão com correção simples.
- Marcar como Pronto não Publica.
- No fim, as ações são “Publicar agora” e “Guardar na Biblioteca”.

### 17.5 Organização da Biblioteca

**Recursos mínimos:** lista ordenada pela atualização mais recente, abertura, retomada e arquivamento.

**Critérios de aceite:**

- Título da área é “Seus conteúdos”.
- Cartão mostra título, pequena prévia, atualização, estado simples e uma ação principal.
- Estado vazio oferece “Criar meu primeiro conteúdo”.
- Arquivar explica que o Conteúdo não será apagado.
- Arquivar preserva versões, Publicações e Resultados.
- Busca, filtros, renomeação, duplicação, restauração e ações em massa não fazem parte do primeiro fluxo.
- Ações em massa não fazem parte do MVP.

### 17.6 Publicação

**Passos mínimos:** validar, escolher ou confirmar destino, visualizar prévia, confirmar, acompanhar envio e receber resultado.

**Critérios de aceite:**

- Pendências aparecem uma por vez com “Corrigir agora”.
- Conta Conectada é mostrada por nome e Canal antes da confirmação.
- Prévia representa a versão que será congelada.
- Alteração feita para adequação ao Canal é informada.
- A pergunta final é “Você quer publicar este conteúdo agora?”.
- O botão de confirmação é “Sim, publicar agora”.
- Voltar, fechar ou tocar fora não confirma.
- Toques repetidos não criam Publicações duplicadas.
- Sair durante o envio não perde o andamento.
- Sucesso, espera, falha conhecida e resultado incerto possuem mensagens distintas.
- Falha preserva Conteúdo e indica “Corrigir” ou “Tentar novamente” apenas quando seguro.

### 17.7 Acompanhamento dos resultados

**Tela mínima:** detalhe da Publicação acessível pelo sucesso, Início ou Conteúdo.

**Critérios de aceite:**

- Cada resultado identifica Conteúdo, Canal, período e última atualização.
- Página, horário, estado confirmado e link externo aparecem primeiro.
- No máximo uma métrica externa comprovada aparece com nome simples e explicação.
- Ausência de dados não é mostrada como zero.
- Dado antigo permanece com data quando a atualização falhar.
- Fato, limitação e próximo passo aparecem separados.
- Não há comparação, consolidação ou recomendação baseada em desempenho.

### 17.8 Ajuda do Tutor

**Critérios de aceite:**

- Abre em qualquer área sem descartar dados da tela.
- Inicia sempre com “O que você quer fazer hoje?”.
- Opções sugeridas refletem a tela e o estado atuais.
- Aceita somente intenções previstas e texto livre curto; baixa confiança volta às opções guiadas.
- Faz somente uma pergunta por mensagem quando precisa esclarecer.
- Encaminha ao campo ou etapa correta com explicação prévia.
- Não mostra termos internos nem dados de outra conta ou Projeto.
- Não gera texto, não altera Conteúdo e não realiza ação externa.
- Quando não entende, reformula com opções e oferece suporte após nova dificuldade.
- Ao fechar, o Usuário retorna ao mesmo ponto com dados preservados.

## 18. Conteúdo textual mínimo da interface

| Situação | Texto principal esperado | Ação principal |
|---|---|---|
| Abertura do Tutor | “O que você quer fazer hoje?” | Opção escolhida pelo Usuário |
| Primeiro Conteúdo | “Sobre o que você quer falar?” | Continuar |
| Rascunho salvo | “Salvo” | Continuar |
| Conta não conectada | “Conecte sua conta para publicar” | Conectar minha conta |
| Conta expirada | “Sua conta precisa ser conectada novamente” | Conectar novamente |
| Revisão concluída | “Seu conteúdo está pronto para publicar” | Publicar agora |
| Confirmação | “Você quer publicar este conteúdo agora?” | Sim, publicar agora |
| Em andamento | “Estamos publicando seu conteúdo” | Ver andamento |
| Sucesso | “Seu conteúdo foi publicado” | Ver publicação |
| Falha | “Não conseguimos publicar. Seu conteúdo está salvo” | Corrigir ou tentar novamente |
| Sem resultado | “Ainda não há informações suficientes” | Voltar depois |
| Biblioteca vazia | “Seus conteúdos aparecerão aqui” | Criar meu primeiro conteúdo |

Os textos podem receber ajustes após testes, exceto a pergunta obrigatória de abertura do Tutor, que deve permanecer exata enquanto esta decisão estiver vigente.

## 19. Cenários de retomada e recuperação

| Interrupção | Retomada esperada |
|---|---|
| App fechado no onboarding | Primeiro passo obrigatório incompleto |
| Saída durante configuração | Resumo do progresso e botão Continuar |
| Saída durante edição | Última versão confirmada e etapa correspondente |
| Perda de conexão ao salvar | Conteúdo mantido na tela e nova tentativa informada |
| Saída durante Publicação | Estado reconciliado antes de oferecer nova ação |
| Conta perdeu permissão | Conteúdo preservado e fluxo de nova conexão |
| Atualização de Resultados falhou | Últimos dados com data e aviso de atualização |
| Tutor fechado | Retorno à mesma tela e aos mesmos dados |

## 20. Plano de teste da experiência

### 20.1 Participantes

Recrutar representantes do segmento prioritário com baixa familiaridade tecnológica declarada e uso real do Canal escolhido. Incluir variação de idade, dispositivo, necessidade de acessibilidade e experiência anterior quando possível.

### 20.2 Tarefas moderadas

1. Entrar pela primeira vez e explicar o que a plataforma faz.
2. Configurar objetivo e público e conectar uma conta de teste.
3. Criar um Conteúdo a partir de uma ideia curta.
4. Guardar, localizar na Biblioteca e retomar.
5. Preparar e confirmar uma Publicação controlada.
6. Recuperar uma falha simulada de conexão ou envio.
7. Encontrar e explicar um resultado.
8. Pedir ao Tutor ajuda para decidir o próximo passo.

O moderador não deve ensinar o caminho. Pode perguntar o que a pessoa espera que aconteça antes de uma ação importante.

### 20.3 Evidências observadas

- conclusão sem ajuda;
- tempo e hesitação por etapa;
- retornos desnecessários;
- termos não compreendidos;
- percepção de salvamento;
- compreensão de conta, destino e consequência antes de Publicar;
- capacidade de recuperar erro;
- interpretação correta de resultados;
- confiança no Tutor e compreensão de seus limites.

### 20.4 Problemas críticos

Impedem o piloto real:

- Usuário publica sem perceber;
- não distingue salvar de Publicar;
- não reconhece a conta de destino;
- perde Conteúdo ou acredita ter perdido;
- envia duas vezes após espera ou erro;
- interpreta ausência de dados como resultado zero;
- acredita que o Tutor executará uma ação sem confirmação;
- não consegue sair, voltar ou pedir ajuda.

## 21. Critério de pronto da UX do MVP

A UX está pronta para piloto quando todos os oito fluxos possuem caminho principal, estados vazios, espera, erro e retomada definidos; os textos essenciais foram testados; os problemas críticos foram resolvidos; e representantes do público completam o ciclo entendendo o próximo passo, o que foi salvo e quando uma ação externa realmente aconteceu.
