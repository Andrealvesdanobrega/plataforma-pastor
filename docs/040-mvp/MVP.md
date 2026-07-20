# Produto Mínimo Viável

**Versão:** 0.3

**Fase:** Sprint 5 — Decisões de Engenharia do MVP

**Atualizado em:** 20 de julho de 2026

## 1. Objetivo do MVP

Validar se uma pessoa com pouca familiaridade tecnológica consegue completar, em uma única experiência guiada, o ciclo entre configurar a plataforma, conectar uma conta, criar e organizar um Conteúdo, publicá-lo e compreender os primeiros resultados.

O MVP não busca validar todas as automações ou todos os Canais. Ele valida a proposta central do produto com um ciclo pequeno, real e mensurável.

## 2. Hipótese principal

Se a plataforma apresentar uma única porta de entrada e um Tutor que explique cada etapa, então usuários iniciantes conseguirão publicar Conteúdo com maior autonomia e confiança e saberão qual próximo passo realizar depois de consultar os resultados.

## 3. Princípio do escopo

O **Conteúdo é a entidade central do MVP**. A configuração prepara o contexto do Conteúdo; a Biblioteca o organiza; o Canal e a Conta Conectada permitem sua distribuição; a Publicação registra o envio; o Relatório apresenta seu resultado; e o Tutor transforma esse resultado em orientação.

### 3.1 Baseline de engenharia

A primeira versão utilizável será Web responsiva. Usará TypeScript e Node.js 24 LTS, Next.js no Frontend, NestJS na API e no worker, PostgreSQL com Prisma, Amazon Cognito para identidade, outbox e fila no PostgreSQL, e serviços gerenciados da AWS na região de São Paulo. IA generativa e aplicativo Mobile ficam fora dessa versão. As justificativas, riscos e reversibilidade estão em `APPROVED-DECISIONS.md`.

## 4. Público do piloto

O piloto deverá escolher um único segmento entre pastores, pequenos empreendedores, influenciadores iniciantes e criadores de conteúdo. A escolha depende da pesquisa de descoberta e deve registrar:

- problema frequente e relevante;
- acesso viável a participantes;
- tipo de Conteúdo prioritário;
- Canal utilizado no comportamento atual;
- disposição para testar uma publicação real.

Enquanto essa escolha não for validada, o documento define estrutura e critérios, não um compromisso com segmento ou Canal específico.

## 5. Fluxo mínimo de ponta a ponta

```text
Baixar App
→ Configurar
→ Conectar Contas
→ Criar Conteúdo
→ Organizar Biblioteca
→ Publicar
→ Acompanhar Resultados
→ Aprender com o Tutor
```

### 5.1 Baixar App

**Entrega mínima:** disponibilizar uma forma oficial de instalar ou acessar a experiência escolhida para o piloto, informar compatibilidade e apresentar a proposta no primeiro acesso.

**Resultado esperado:** Usuário abre o App e entende o benefício antes de criar o acesso.

### 5.2 Configurar

**Entrega mínima:** criação e recuperação básica de acesso, perfil simples, objetivo, público, fuso horário, consentimentos e um Projeto principal criado automaticamente ou confirmado.

**Resultado esperado:** Usuário conclui o onboarding, entende as etapas e pode retomar se interromper.

### 5.3 Conectar Contas

**Entrega mínima:** catálogo com um Canal, explicação de permissões, autorização externa por uma Integração, identificação da conta e estados de sucesso, falha e reconexão.

**Resultado esperado:** existe uma Conta Conectada ativa e o Usuário sabe qual conta será usada.

### 5.4 Criar Conteúdo

**Entrega mínima:** criação de um tipo de Conteúdo a partir de ideia, objetivo, público e mensagem; edição, salvamento automático ou explícito confiável e assistência simples do Tutor.

**Resultado esperado:** existe um Rascunho persistido e editável, sem publicação implícita.

### 5.5 Organizar Biblioteca

**Entrega mínima:** lista dos Conteúdos do Projeto com título, data e estado; abertura, filtro por estado, retomada e arquivamento.

**Resultado esperado:** Usuário localiza o Conteúdo e entende sua situação e próximo passo.

### 5.6 Publicar

**Entrega mínima:** validação das regras do Canal, prévia, identificação da Conta Conectada, confirmação explícita, publicação imediata, estado do envio e recuperação de falha.

**Resultado esperado:** Publicação real confirmada ou falha compreensível e recuperável, sem duplicidade nem perda do Conteúdo.

### 5.7 Acompanhar Resultados

**Entrega mínima:** consulta de um conjunto essencial de métricas oferecidas pelo Canal, relacionadas à Publicação, com fonte, período e última atualização.

**Resultado esperado:** Usuário confirma que o Conteúdo foi publicado e entende pelo menos um resultado disponível.

### 5.8 Aprender com o Tutor

**Entrega mínima:** explicação baseada em regras sobre o estado do ciclo e os resultados, seguida de uma recomendação simples e opcional.

**Resultado esperado:** Usuário explica o próximo passo e pode iniciar um novo ciclo sem que o Tutor aja em seu nome.

## 6. Entidades no MVP

| Entidade | Uso no MVP | Profundidade inicial |
|---|---|---|
| Usuário | Identidade, preferências e onboarding | Um papel: proprietário |
| Projeto | Contexto editorial e isolamento | Um Projeto principal por Usuário |
| Conteúdo | Fonte editorial central | Um tipo prioritário, com versões essenciais |
| Biblioteca de Conteúdos | Localização e organização | Lista, filtro por estado, retomada e arquivo |
| Canal | Destino suportado | Um Canal validado |
| Conta Conectada | Conta real autorizada | Uma Conta ativa no caminho principal |
| Integração | Autorização, publicação e métricas | Um conector operacional |
| Publicação | Envio e resultado por destino | Publicação imediata e tentativas rastreáveis |
| Relatório | Resultados essenciais | Uma visão por Conteúdo ou Publicação |
| Tutor | Orientação do ciclo | Regras, modelos de texto e recomendações simples, sem IA generativa |
| Agendamento | Evolução condicionada | Fora do caminho crítico; incluir somente se pesquisa exigir |
| Campanha | Agrupamento opcional | Fora do MVP |

## 7. Capacidades obrigatórias

### Experiência e tutoria

- indicador de progresso e próximo passo;
- linguagem simples e ajuda contextual;
- salvamento e retomada;
- sugestões identificadas como opcionais;
- estados vazios, carregamento, sucesso e erro compreensíveis;
- acessibilidade do caminho principal.

### Conteúdo e Biblioteca

- criar, editar, salvar, revisar e arquivar Conteúdo;
- preservar versão usada em Publicação;
- validar requisitos do tipo e do Canal;
- listar e filtrar Conteúdos por estado;
- impedir que salvar ou receber sugestão publique algo.

### Canal, Conta e Integração

- explicar permissões antes da autorização;
- conectar, verificar, reconectar e desconectar;
- proteger tokens e demais segredos;
- conhecer formatos, limites e métricas suportadas;
- traduzir falhas técnicas em ações possíveis.

### Publicação e resultado

- mostrar Conteúdo, versão, Canal e conta na prévia;
- exigir confirmação explícita;
- usar chave de idempotência e consultar envios incertos;
- guardar tentativas e referência externa;
- coletar métricas sem misturar definições;
- informar período, fonte e atualização;
- recomendar sem prometer causalidade ou desempenho.

## 8. Cenário de sucesso do piloto

1. Uma pessoa elegível instala ou acessa o App sem assistência da equipe.
2. Conclui sua configuração e identifica o próximo passo.
3. Conecta conscientemente uma conta externa.
4. Cria um Conteúdo alinhado ao formato escolhido.
5. Encontra e retoma esse Conteúdo na Biblioteca.
6. Revisa o destino e confirma uma Publicação real.
7. Consulta os resultados disponíveis.
8. Compreende a orientação do Tutor e decide o que fazer depois.

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

- percentual que conclui a configuração;
- percentual que conecta a primeira conta;
- percentual que cria o primeiro Rascunho;
- percentual que chega à primeira Publicação confirmada;
- tempo mediano entre primeiro acesso e cada marco.

### Autonomia e confiança

- conclusão de cada etapa sem ajuda externa;
- pedidos de ajuda e erros por etapa;
- compreensão de permissões e destino antes da confirmação;
- percepção de confiança para publicar;
- compreensão correta do estado e do próximo passo.

### Valor contínuo

- retorno para consultar Relatório;
- retorno para criar um segundo Conteúdo;
- recomendação do Tutor aceita, adaptada ou rejeitada;
- redução percebida de esforço ou número de ferramentas.

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

- Campanhas;
- colaboração e múltiplos papéis;
- múltiplos Projetos por Usuário na interface;
- múltiplos Canais e distribuição simultânea;
- calendário editorial completo e recorrência;
- Agendamento, salvo se a descoberta o tornar indispensável;
- edição profissional de imagem, áudio ou vídeo;
- geração ou publicação totalmente autônoma;
- relatórios comparativos avançados;
- mídia paga, CRM, comércio eletrônico ou garantia de desempenho.

## 13. Dependências e riscos

| Dependência ou risco | Tratamento antes do piloto |
|---|---|
| Web responsivo não atender ao contexto do público | Testar primeiro acesso, retorno de autorização e seleção de mídia nos dispositivos reais |
| Canal e formato não definidos | Selecionar a partir de pesquisa comportamental |
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

Segmento, Canal, formato, métricas, armazenamento conectado, retenção e metas operacionais não bloqueiam o núcleo. Eles bloqueiam a conexão a serviços reais e a liberação para piloto.

## 15. Critérios para a primeira versão utilizável

“Primeira versão utilizável” significa uma versão liberável para piloto controlado, e não apenas uma demonstração técnica. Todos os critérios são cumulativos:

- segmento, tipo de Conteúdo, Canal, formato e métricas essenciais aprovados;
- acesso Web por código de e-mail, sessão protegida e encerramento testados;
- configuração de Usuário e Projeto, Tutor determinístico, criação, retomada e Biblioteca funcionando no ciclo completo;
- conexão, verificação, revogação e reconexão de uma Conta real controlada comprovadas;
- prévia mostra Versão, Canal e conta, e somente a confirmação explícita cria a Publicação;
- Publicação real é assíncrona, idempotente, auditável e reconciliável em sucesso, timeout e falha;
- Resultado mostra fonte, período, atualização e próximo passo sem promessa de desempenho;
- arquivos, temporários, tokens, segredos, logs e isolamento por Projeto atendem à baseline de segurança;
- acessibilidade do caminho principal, migração, restauração, alertas e revogação de token foram verificados em homologação;
- privacidade, retenção, exclusão, suporte e resposta a incidente estão aprovados;
- representantes do público concluem o ciclo sem problema crítico de UX;
- não existe risco crítico de segurança sem tratamento aprovado.

Os detalhes normativos estão em ENG-15 de `APPROVED-DECISIONS.md`. O MVP será considerado validado quando o grupo piloto completar Publicações reais com segurança, a maioria compreender o fluxo e o próximo passo sem suporte especializado, as falhas críticas forem recuperáveis e houver evidência de retorno para acompanhar resultados ou criar novo Conteúdo.

Os limiares quantitativos devem ser aprovados antes do piloto. Caso os sinais não sejam atingidos, a decisão correta pode ser revisar segmento, linguagem, jornada, Canal ou proposta de valor em vez de ampliar funcionalidades.

## 16. Experiência mínima entregável

O MVP deve parecer uma única jornada, mesmo que diferentes partes do sistema realizem as tarefas. O Usuário não precisa conhecer as entidades internas nem decidir configurações que a plataforma pode recomendar com segurança.

### 16.1 Navegação mínima

- **Início:** situação atual e próximo passo.
- **Criar:** início de um Conteúdo.
- **Biblioteca:** lista e organização dos Conteúdos.
- **Resultados:** acompanhamento das Publicações.
- **Ajuda:** acesso permanente ao Tutor.
- **Configurações:** perfil e Conta Conectada, acessível sem competir com as tarefas principais.

### 16.2 Regra do Tutor

Toda vez que o Tutor for aberto, inclusive no primeiro acesso e na tela principal, sua primeira pergunta será exatamente:

> O que você quer fazer hoje?

Depois da pergunta, o Tutor apresenta no máximo quatro opções coerentes com o contexto e aceita uma resposta escrita. Ele faz uma pergunta por vez, não repete dados já conhecidos sem necessidade e nunca executa Publicação, conexão ou consentimento pelo Usuário.

### 16.3 Padrão de etapa

Cada etapa do MVP inclui título, motivo curto, progresso quando aplicável, uma ação principal, alternativa segura, ajuda e retorno visível de salvamento ou resultado. Opções avançadas não aparecem no caminho principal.

## 17. Escopo detalhado dos fluxos

### 17.1 Onboarding

**Telas mínimas:**

1. Boas-vindas e promessa.
2. Explicação de como funciona e garantia de confirmação antes de Publicar.
3. Criar acesso ou entrar.
4. Consentimentos essenciais em linguagem simples.
5. Confirmação e entrada na configuração.

**Critérios de aceite:**

- A primeira tela possui “Começar” e “Já tenho uma conta”.
- O Usuário entende que nada será publicado sozinho.
- Campos mostram requisitos antes do envio e erros junto ao campo.
- É possível recuperar o acesso por um caminho visível.
- Fechar e reabrir retoma a primeira etapa incompleta.
- Consentimento opcional não começa selecionado.
- Conclusão leva diretamente à configuração assistida.

### 17.2 Configuração assistida

**Passos mínimos:** sobre o Usuário, objetivo, público, Canal, conexão de conta e resumo.

**Critérios de aceite:**

- O progresso informa o passo atual e o total.
- Objetivo e público usam exemplos, mas aceitam resposta própria ou incerteza.
- Um Projeto principal é criado sem exigir que o Usuário conheça esse conceito.
- Antes da autorização externa, a interface explica o que acontecerá e informa que não verá a senha do Canal.
- Ao retornar, nome e imagem pública da conta são mostrados para confirmação quando disponíveis.
- Cancelar a autorização não registra sucesso nem perde respostas anteriores.
- A conexão pode ser pulada para Criar, mas Publicar explica a pendência.
- O resumo final permite corrigir objetivo, público ou conta.

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

**Passos mínimos:** ideia, objetivo, público, sugestão opcional, edição, mídia quando necessária, revisão e decisão de Publicar ou guardar.

**Critérios de aceite:**

- O fluxo informa que haverá revisão antes da Publicação.
- A primeira ideia confirmada cria um Rascunho recuperável.
- Público configurado é sugerido, mas pode ser alterado para o Conteúdo.
- Sugestão do Tutor só entra no Conteúdo depois de escolha explícita.
- Editor mostra confirmação real de salvamento.
- Usuário consegue voltar à versão anterior suportada pelo MVP.
- Mídia obrigatória e opcional são diferenciadas.
- Pendência obrigatória e melhoria opcional são apresentadas separadamente.
- Marcar como Pronto não Publica.
- No fim, as ações são “Publicar agora” e “Guardar na Biblioteca”.

### 17.5 Organização da Biblioteca

**Recursos mínimos:** lista, busca simples, filtro por estado, abertura, retomada, renomeação e arquivamento.

**Critérios de aceite:**

- Título da área é “Seus conteúdos”.
- Cartão mostra título, pequena prévia, atualização, estado simples e uma ação principal.
- Filtros são “Todos”, “Para continuar”, “Prontos”, “Publicados” e “Precisam de atenção”.
- Estado vazio oferece “Criar meu primeiro conteúdo”.
- Filtro sem resultado oferece “Limpar filtro”.
- Arquivar explica que o Conteúdo não será apagado.
- Restaurar mantém versões, Publicações e Resultados.
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

**Telas mínimas:** lista de Conteúdos Publicados com dados e detalhe de uma Publicação.

**Critérios de aceite:**

- A área começa com uma explicação, não com uma grade de números.
- Cada resultado identifica Conteúdo, Canal, período e última atualização.
- No máximo as métricas essenciais validadas aparecem no primeiro nível.
- Cada métrica possui nome simples e uma frase de explicação.
- Ausência de dados não é mostrada como zero.
- Dado antigo permanece com data quando a atualização falhar.
- Fato, limitação e recomendação aparecem em blocos separados.
- Tutor oferece uma ação que pode ser aceita, adaptada ou recusada.

### 17.8 Ajuda do Tutor

**Critérios de aceite:**

- Abre em qualquer área sem descartar dados da tela.
- Inicia sempre com “O que você quer fazer hoje?”.
- Opções sugeridas refletem a tela e o estado atuais.
- Aceita pedido com palavras do Usuário.
- Faz somente uma pergunta por mensagem quando precisa esclarecer.
- Encaminha ao campo ou etapa correta com explicação prévia.
- Não mostra termos internos nem dados de outra conta ou Projeto.
- Não altera Conteúdo sem aceitação nem realiza ação externa.
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
