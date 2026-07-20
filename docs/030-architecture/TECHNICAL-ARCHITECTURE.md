# Arquitetura Técnica do MVP

**Versão:** 0.2

**Fase:** Sprint 5 — Decisões de Engenharia do MVP

**Atualizado em:** 20 de julho de 2026

**Estado:** baseline tecnológica aprovada; implementação ainda não iniciada

## 1. Objetivo

Definir a primeira arquitetura técnica completa para entregar e operar o ciclo do MVP: entrar, configurar, conectar uma conta, criar e organizar Conteúdo, confirmar uma Publicação, acompanhar resultados e receber orientação do Tutor.

A arquitetura traduz o domínio e a experiência aprovados nas Sprints anteriores sem ampliar o produto. A Sprint 5 fecha a plataforma necessária para iniciar o núcleo; Canal, formato, armazenamento conectado, governança e metas operacionais permanecem subordinados às evidências registradas em `OPEN-DECISIONS.md`.

## 2. Princípios obrigatórios

1. **Conteúdo é a entidade central.** Identidade, Biblioteca, versões, mídias, Publicações, métricas e recomendações se relacionam a ele.
2. **Tutor é a única porta de entrada da experiência.** Web e Mobile iniciam pelo Tutor com “O que você quer fazer hoje?” e convertem a intenção em passos guiados.
3. **Usuário não conversa com integrações técnicas.** O backend traduz capacidades, estados e erros de serviços externos. Telas oficiais de consentimento do provedor são a única passagem externa inevitável.
4. **Ações sensíveis exigem confirmação.** Tutor e modelos de inteligência artificial não publicam, conectam, removem ou consentem em nome do Usuário.
5. **Arquivos grandes ficam preferencialmente onde o Usuário já os guarda.** Google Drive, OneDrive, Dropbox e serviços equivalentes permanecem como origem; a plataforma guarda referência e conhecimento, não uma cópia permanente por padrão.
6. **A plataforma guarda principalmente conhecimento.** Metadados, relacionamentos, versões textuais, estados, histórico, configurações, permissões, auditoria e métricas compõem seu núcleo persistente.
7. **Modular antes de distribuído.** O MVP usa um monólito modular e separa processos apenas onde execução assíncrona ou isolamento operacional exigem.
8. **Falha é estado, não exceção invisível.** Operações externas são rastreáveis, idempotentes e reconciliáveis.
9. **Segurança e privacidade por padrão.** Menor privilégio, minimização, criptografia, retenção explícita e auditoria orientam todos os componentes.

## 3. Decisões arquiteturais da primeira versão

| Tema | Decisão para o MVP | Motivo |
|---|---|---|
| Cliente primário | Web responsivo em Next.js; Mobile posterior | Entrega o ciclo em desktop e celular sem manter dois clientes |
| Linguagem e runtime | TypeScript estrito sobre Node.js 24 LTS | Compartilha contratos e reduz divergência entre Web, API e worker |
| Estilo de backend | Monólito modular | Menor custo operacional e transações locais, com limites evolutivos claros |
| Framework backend | NestJS para API e worker | Organiza módulos e permite dois processos sobre o mesmo núcleo |
| Framework frontend | Next.js com React e App Router | Cliente Web responsivo, acessível e implantável em contêiner |
| Processos | Next.js Web, NestJS API e NestJS worker | Separa experiência interativa de Integrações lentas sem criar microserviços prematuros |
| Contrato dos clientes | API HTTP versionada; consulta ou fluxo de estado para tarefas longas | Um contrato para Web e Mobile e retomada simples |
| Dados transacionais | Amazon RDS for PostgreSQL; PostgreSQL local | Integridade, relacionamentos, auditoria e consultas da Biblioteca |
| Acesso a dados | Prisma ORM, Prisma Migrate e SQL explícito revisado quando necessário | Produtividade tipada sem perder recursos essenciais do PostgreSQL |
| Busca inicial | Índices e busca textual do próprio banco | Evita motor de busca adicional no MVP |
| Mensageria inicial | Outbox e fila de trabalhos no PostgreSQL | Evita perda entre mudança de estado e trabalho assíncrono sem outro serviço |
| Identidade | Amazon Cognito por OIDC, código de uso único por e-mail e sessão Web de servidor | Evita senha própria e mantém autorização no domínio |
| Tutor | Orientação determinística; sem IA generativa na primeira versão utilizável | Mantém previsibilidade e valida valor antes de enviar dados a outro provedor |
| Integrações | Adaptadores por capacidade, atrás de contratos internos | Impede dependência do domínio em detalhes de um provedor |
| Arquivos grandes | Referência a armazenamento conectado pelo Usuário | Reduz custo, duplicação e responsabilidade sobre vídeos |
| Arquivos pequenos | Amazon S3 privado quando necessário | Suporta avatar, miniatura e mídia pequena do piloto com controle de acesso |
| Publicação | Uma Publicação independente por destino | Permite estado, tentativa e resultado próprios e prepara multicanal |
| Nuvem e região | AWS, região `sa-east-1` | Serviços gerenciados em uma região próxima ao público inicial |
| Execução | Imagens OCI no ECR e tarefas ECS Fargate | Separa Web, API e worker sem administrar servidores ou Kubernetes |
| Ambientes | Desenvolvimento local, homologação e produção isolados | Evita usar contas e dados reais em testes |

Motivo, benefícios, riscos e reversibilidade de cada escolha estão registrados em `APPROVED-DECISIONS.md`.

## 4. Visão geral do sistema

```text
┌──────────────────── Clientes ────────────────────┐
│ Frontend Web responsivo      Aplicativo Mobile  │
│          Tutor como porta de entrada            │
└──────────────────────┬──────────────────────────┘
                       │ HTTPS
                 ┌─────▼─────┐
                 │ Borda/API │
                 │ proteção, │
                 │ sessão e  │
                 │ limites   │
                 └─────┬─────┘
                       │
┌──────────────────────▼───────────────────────────┐
│ Backend — monólito modular                       │
│                                                 │
│ Experiência/Tutor ── Orquestrador               │
│       │                │                        │
│ Identidade/Projeto  Conteúdo/Biblioteca          │
│       │                │                        │
│ Contas/Canais ── Publicação ── Resultados        │
│       │                │              │         │
│ Auditoria        Outbox/Trabalhos     │         │
└──────────────┬─────────┬──────────────┬─────────┘
               │         │              │
        ┌──────▼───┐ ┌───▼─────┐  ┌────▼────────┐
        │PostgreSQL│ │ Worker  │  │Armazenamento│
        │          │ │durável  │  │privado      │
        └──────────┘ └───┬─────┘  └─────────────┘
                         │
             ┌───────────▼───────────────────────┐
             │ Adaptadores de Integração         │
             │ Canal, armazenamento, IA, avisos │
             └─────┬─────────┬─────────┬────────┘
                   │         │         │
              Canais    Drive/Cloud  Provedor
              externos  do Usuário   de IA?
```

### 4.1 Fluxo técnico principal

1. Cliente autenticado abre o Tutor e informa uma intenção.
2. Tutor identifica a intenção e pede ao Orquestrador a próxima etapa permitida.
3. Orquestrador consulta domínio, autorização e estado atual.
4. Cliente apresenta formulário ou confirmação guiada.
5. Backend executa a mudança transacional no módulo dono da entidade.
6. Trabalho externo é registrado no outbox e processado pelo worker.
7. Adaptador chama o serviço externo e normaliza o resultado.
8. Backend reconcilia estado, registra auditoria e disponibiliza a atualização.
9. Tutor explica o resultado e recomenda o próximo passo.

O cliente nunca recebe segredos de Integração nem escolhe chamadas técnicas. Mesmo quando uma autorização externa abre a página do provedor, o backend prepara e valida todo o fluxo.

## 5. Unidades de implantação

### 5.1 Frontend Web

Aplicação Next.js responsiva, executada sobre Node.js e distribuída atrás da borda da AWS. É o único cliente completo da primeira versão utilizável. Recursos de aplicação web progressiva somente serão habilitados se o teste do piloto demonstrar valor; operação offline completa permanece fora.

### 5.2 Aplicativo Mobile

Cliente instalável permanece como possibilidade arquitetural posterior. Não será implementado na primeira versão utilizável. Se aprovado depois, usará a mesma API e terá recursos próprios apenas onde agregarem valor: armazenamento seguro de sessão, retorno de autorização externa, seleção de arquivo e compartilhamento para o App.

### 5.3 API

Processo sem estado responsável por autenticação da requisição, autorização, comandos e consultas rápidas. Escala horizontalmente e não executa chamada externa longa dentro da resposta quando ela puder bloquear ou duplicar a ação.

### 5.4 Worker

Processo sem interface pública que consome trabalhos duráveis: publicar, reconciliar estado, renovar conexão quando permitido, coletar métricas, gerar miniatura e limpar arquivo temporário. Escala separadamente por tipo e volume de trabalho.

### 5.5 Persistência e serviços gerenciados

Produção e homologação usam Amazon RDS for PostgreSQL, Amazon S3, Amazon Cognito, AWS Secrets Manager/KMS, Amazon CloudWatch, Amazon ECR e Amazon ECS com Fargate. A fila inicial permanece no PostgreSQL. Não há Redis, broker dedicado ou Kubernetes no MVP.

## 6. Backend

### 6.0 Tecnologia

API e worker são escritos em TypeScript estrito sobre Node.js 24 LTS e NestJS. A linha do runtime deve ser atualizada antes do fim de seu suporte, mediante teste automatizado e homologação. O repositório usa `npm workspaces` para manter aplicações e pacotes internos sem publicação independente obrigatória.

### 6.1 Estilo

O backend é um monólito modular com dependências internas direcionadas. Cada módulo possui modelo, regras, casos de uso e interfaces de persistência próprios. Comunicação direta entre tabelas de outro módulo é evitada; integrações internas ocorrem por serviços públicos ou eventos de domínio.

Não há necessidade de microserviços no MVP. API e worker podem usar o mesmo artefato, configuração e módulos, mas executam papéis diferentes.

### 6.2 Módulos

| Módulo | Responsabilidade | Dados dominantes |
|---|---|---|
| Identidade e Acesso | Vincular identidade externa, sessão, consentimentos e participação | Usuário e participação |
| Projeto | Contexto, objetivo, público e isolamento | Projeto |
| Conteúdo | Fonte editorial, versões, mídias e estados | Conteúdo, Versão e Mídia |
| Biblioteca | Consultas, busca, filtros, retomada e arquivo | Projeção sobre Conteúdo |
| Tutor | Intenção, contexto permitido, explicação e recomendação | Interação e recomendação |
| Orquestração | Jornada, próximos passos e coordenação de casos de uso | Marcos e trabalhos, sem possuir Conteúdo |
| Canais e Contas | Catálogo, autorização e saúde de conexões | Canal e Conta Conectada |
| Publicação | Validação, congelamento, idempotência, tentativa e reconciliação | Publicação e Tentativa |
| Resultados | Coleta, catálogo de métricas e relatórios | Métrica e Relatório |
| Integrações | Adaptadores externos e tradução de erros | Configuração operacional, sem regra editorial |
| Auditoria | Fatos sensíveis imutáveis e consultáveis | Evento de auditoria |
| Notificações | Avisos mínimos e preferências futuras | Entrega e estado do aviso |

### 6.3 Regras de dependência

- Tutor chama Orquestrador, nunca um adaptador externo.
- Orquestrador chama casos de uso dos módulos e não altera suas tabelas diretamente.
- Conteúdo não conhece SDKs de Canal, armazenamento ou IA.
- Publicação depende de uma interface de Canal; o adaptador implementa a interface.
- Resultados preserva a definição original da métrica e não inventa equivalência.
- Integrações não decidem se algo pode ser publicado; recebem uma intenção já autorizada e validada.
- Auditoria recebe fatos dos casos de uso; não é reconstruída a partir de logs.

### 6.4 API

- Contrato versionado e documentado para Web e Mobile.
- Comandos de escrita aceitam identificador de idempotência quando repetição for perigosa.
- Respostas de erro usam código interno estável e mensagem segura traduzível para a experiência.
- Paginação por cursor ou chave estável nas listas que podem crescer.
- Limites de tamanho, frequência e tempo definidos por operação.
- Atualizações longas retornam um identificador de operação; o cliente consulta ou recebe mudança de estado autenticada.
- Webhooks ficam em rotas separadas, autenticadas pelo método do provedor e processadas de forma assíncrona.

## 7. Frontend Web

O framework aprovado é Next.js com React, TypeScript e App Router. Renderização no servidor e cache são usados somente por rota e com política explícita; estado autenticado ou variável não pode ser servido por cache compartilhado. A aplicação é empacotada como imagem OCI e não depende de uma plataforma proprietária do framework.

### 7.1 Responsabilidades

- renderizar a conversa e as ações guiadas do Tutor;
- implementar onboarding, criação, Biblioteca, Publicação e Resultados;
- manter estado transitório de formulário sem assumir que foi persistido;
- mostrar “Salvo” somente após confirmação do backend;
- preservar acessibilidade, navegação por teclado e leitura assistiva;
- traduzir códigos de estado conhecidos para linguagem aprovada;
- nunca chamar Canal, armazenamento ou provedor de IA diretamente.

### 7.2 Estrutura

- Casca da experiência com Tutor, navegação e contexto do Projeto.
- Módulos de tela alinhados aos fluxos funcionais, sem duplicar regras do backend.
- Cliente de API único com tratamento de sessão, correlação e erros.
- Estado remoto tratado como fonte do servidor; cache local curto e invalidável.
- Formulários com validação de usabilidade no cliente e validação autoritativa no backend.
- Arquivos enviados por URL temporária ou fluxo controlado, nunca por credencial permanente.

### 7.3 Sessão e segurança Web

- Sessão preferencial em cookie `Secure`, `HttpOnly` e com política `SameSite` adequada.
- Proteção contra falsificação de requisição nas operações de escrita.
- Política de conteúdo restritiva, proteção contra inclusão em quadros e cabeçalhos seguros.
- Nenhum token de Canal, provedor de IA ou armazenamento em JavaScript, URL ou armazenamento local.
- Conteúdo produzido pelo Usuário é tratado como dado, não HTML executável.

### 7.4 Continuidade

O navegador pode manter somente estado transitório necessário para evitar perda visual. Rascunhos confirmados, etapa da jornada e operações em andamento permanecem no backend. Suporte offline completo não faz parte do MVP.

## 8. Aplicativo Mobile

### 8.1 Responsabilidades

O Mobile entrega a mesma experiência do Web usando a mesma API. Não implementa regras paralelas de Conteúdo, autorização ou Publicação.

### 8.2 Estrutura

- Casca nativa ou multiplataforma com componentes acessíveis.
- Módulos de telas equivalentes aos fluxos do MVP.
- Adaptadores locais para seleção de mídia, compartilhamento para o App, links de retorno e armazenamento seguro.
- Cache local mínimo, criptografado quando contiver dado sensível e descartável após sincronização.
- Operações externas continuam no backend mesmo se o App for fechado.

### 8.3 Sessão e segurança Mobile

- Fluxo de autorização com navegador do sistema e prova de posse apropriada ao cliente público.
- Credencial renovável armazenada apenas no cofre seguro do sistema operacional.
- Credencial de curta duração mantida em memória quando possível.
- Links de retorno com validação de estado e associação à tentativa iniciada.
- Nenhum segredo permanente embutido no aplicativo.
- Dispositivo comprometido não é considerado ambiente confiável; toda autorização é repetida no backend.

### 8.4 Estratégia do MVP

Web responsivo é o cliente primário e único cliente completo da primeira versão utilizável. Mobile está adiado até que evidência de instalação, recursos do aparelho ou compartilhamento justifique a segunda base. A arquitetura preserva uma API independente para permitir essa evolução sem exigir outro backend.

## 9. Banco de dados

### 9.1 Tecnologia e topologia

PostgreSQL é o motor aprovado. Desenvolvimento usa instância em contêiner; homologação e produção usam Amazon RDS for PostgreSQL na região `sa-east-1`. Cópias automáticas e recuperação pontual são obrigatórias em produção; topologia de alta disponibilidade e prazos de recuperação serão dimensionados pela análise de impacto e orçamento do piloto.

### 9.2 Acesso e migrações

Prisma ORM fornece o cliente tipado e Prisma Migrate mantém o histórico de alterações. Migrações SQL são versionadas, revisadas e promovidas junto do artefato. Recursos como locks de fila, busca, índices especializados e restrições não expressas adequadamente pelo ORM usam SQL explícito encapsulado e coberto por testes de integração.

### 9.3 Organização lógica

- Um banco transacional por ambiente.
- Tabelas agrupadas por módulos ou esquemas lógicos, sem um banco por módulo.
- Chaves estrangeiras para relações críticas.
- `projeto_id` presente nas entidades isoladas por contexto.
- Restrições únicas para versão, participação, idempotência e referências externas.
- Estados controlados e transições validadas pelo domínio.
- Dados específicos de Canal podem usar estrutura flexível controlada, sem transformar todo o modelo em documentos sem regra.

### 9.4 Biblioteca e busca

A Biblioteca é uma consulta paginada sobre Conteúdo, versões e estados. O MVP usa índices compostos por Projeto, arquivo, estado e atualização, além de busca textual normalizada em título e campos aprovados. Motor de busca dedicado só é introduzido se medições reais mostrarem necessidade.

### 9.5 Concorrência e consistência

- Edição usa controle otimista por versão para evitar sobrescrita silenciosa.
- Publicação congela uma Versão antes de registrar o trabalho externo.
- Estado de Publicação muda por transições permitidas e comparação de versão.
- Outbox é escrita na mesma transação do fato que precisa gerar trabalho.
- Coleta e Publicação são eventualmente consistentes com serviços externos.

### 9.6 Proteção e ciclo de vida

- Criptografia em trânsito e repouso.
- Acesso privado por identidade de serviço, sem exposição pública direta.
- Migrações versionadas e reversão ou correção planejada.
- Cópias com retenção definida e restauração testada.
- Dados de produção não são copiados para ambientes inferiores sem anonimização aprovada.
- Exclusão e retenção por categoria são implementadas após política definida.

## 10. Biblioteca de Conteúdos

### 10.1 Arquitetura

A Biblioteca não é um repositório separado. É uma projeção de leitura sobre o agregado Conteúdo, enriquecida com versão atual, última Publicação, pendências e referências de mídia.

### 10.2 Consultas do MVP

- Conteúdos do Projeto atual;
- ordenação por atualização mais recente;
- filtro por estado traduzido;
- busca simples por título e assunto aprovado;
- itens arquivados em consulta separada;
- ação principal derivada do estado atual.

### 10.3 Evolução

Se volume ou combinação de filtros exigir, uma projeção materializada pode ser atualizada por eventos. Um índice externo de busca não deve ser introduzido antes de evidência de limite no banco.

## 11. Tutor

### 11.1 Papel técnico

Tutor é a fachada de experiência que recebe intenção em linguagem comum, consulta contexto permitido e apresenta a próxima ação. Ele não é o Orquestrador, não é um Canal e não possui Conteúdo.

### 11.2 Componentes

| Componente | Responsabilidade |
|---|---|
| Entrada de intenção | Receber escolha guiada ou texto do Usuário |
| Classificador de intenção | Mapear o pedido para uma intenção permitida e nível de confiança |
| Montador de contexto | Selecionar dados mínimos do Usuário, Projeto e etapa |
| Motor de orientação | Aplicar regras de jornada e gerar opções simples |
| Catálogo de modelos determinísticos | Sugerir estrutura ou explicação a partir de regras aprovadas |
| Adaptador de assistência desabilitado | Preservar contrato para experimento futuro sem chamar provedor no MVP |
| Validador de saída | Conferir formato, escopo, segurança e ação proposta |
| Registro de interação | Guardar resumo minimizado e resposta à recomendação |

### 11.3 Base determinística

A primeira versão utilizável executa toda a jornada com intenções, respostas, estruturas e recomendações predefinidas. Texto livre é mapeado por regras conservadoras; baixa confiança retorna opções guiadas. Nenhum Conteúdo ou interação é enviado a provedor de IA.

### 11.4 Evolução segura para IA

IA generativa está adiada. Um experimento futuro somente pode ser habilitado depois de aprovação explícita de valor, qualidade em português, categorias de dados, região, retenção, treinamento, subprocessadores, custo e tratamento de abuso. Mesmo após aprovação, deverá respeitar:

- Conteúdo e instruções do Usuário são dados não confiáveis.
- Contexto enviado é mínimo, autorizado e separado das instruções do sistema.
- Ferramentas disponíveis ao modelo formam uma lista explícita e de baixo risco.
- Saída é estruturada, validada e convertida em proposta, nunca executada diretamente.
- Publicar, conectar, consentir, desconectar, excluir e alterar permissão não são ferramentas autônomas.
- Sugestão editorial precisa de aceitação antes de virar Versão de Conteúdo.
- Provedor, retenção, uso para treinamento e localização de dados devem ser avaliados antes de qualquer experimento com Usuários.

### 11.5 Continuidade e degradação

Se a regra não compreender o pedido, Tutor volta a opções guiadas. Estruturas simples e edição manual permanecem disponíveis. Depois de nova dificuldade, encaminha para etapa ou suporte sem perder o estado.

## 12. Orquestrador

### 12.1 Papel

Orquestrador transforma uma intenção validada em uma sequência segura de casos de uso. Ele conhece a jornada e os estados públicos dos módulos, mas não substitui suas regras.

### 12.2 Responsabilidades

- verificar identidade, Projeto e participação;
- recuperar a etapa atual e as pendências;
- selecionar o próximo caso de uso permitido;
- pedir confirmação quando a ação exigir;
- criar operação durável para trabalho externo;
- acompanhar conclusão, espera, falha e retomada;
- reunir resultado para o Tutor explicar;
- evitar que um comando seja executado duas vezes.

### 12.3 Estado e execução

Fluxos curtos usam transação síncrona. Fluxos externos usam uma máquina de estados persistida, outbox e worker. O Orquestrador não mantém estado crítico apenas em memória e não considera um timeout como falha definitiva.

### 12.4 Limites

- Não escreve diretamente em agregados de outros módulos.
- Não guarda tokens de Conta Conectada.
- Não chama SDK externo sem passar pelo adaptador.
- Não interpreta texto livre; recebe intenção normalizada do Tutor.
- Não contorna confirmação ou autorização por conveniência.

## 13. Integrações externas

### 13.1 Tipos de adaptador

- **Canal:** autorização, validação, Publicação, consulta de estado e métricas.
- **Armazenamento do Usuário:** autorização, seleção, leitura temporária, metadados e revogação.
- **Identidade:** entrada, recuperação, verificação e sessão.
- **IA futura, desabilitada no MVP:** classificação, sugestão e explicação atrás do contrato de assistência.
- **Notificação:** entrega de aviso permitido pelo Usuário.

### 13.2 Contrato interno

Cada adaptador declara capacidades, formatos, limites, permissões, erros normalizados, política de tentativa, idempotência, consulta de estado, webhooks e saúde. O domínio consulta capacidades em vez de presumir comportamento uniforme.

### 13.3 Autorização externa

- Fluxo iniciado pelo backend com estado aleatório de uso único e retorno autorizado.
- Permissões mínimas e incrementais.
- Troca e renovação de credenciais realizadas no backend.
- Tokens guardados em cofre ou forma criptografada com chave separada.
- Revogação local e, quando suportada, no provedor.
- Conta externa, permissões e vencimento registrados como metadados.

### 13.4 Webhooks

- Endpoint dedicado por provedor.
- Validação de assinatura, origem lógica, carimbo de tempo e prevenção de repetição.
- Corpo limitado e persistência mínima antes do processamento assíncrono.
- Associação obrigatória a uma Conta ou Publicação conhecida.
- Resposta rápida ao provedor; regra de domínio executada pelo worker.

### 13.5 Resiliência

- Timeout por operação.
- Nova tentativa somente para falha classificada como temporária.
- Espera crescente com aleatoriedade e limite máximo.
- Respeito aos limites informados pelo provedor.
- Circuito de proteção por Integração para evitar cascata.
- Reconciliação antes de repetir operação com resultado incerto.
- Painel operacional de saúde sem expor dados do Usuário.

## 14. Publicação multicanal

### 14.1 Modelo evolutivo

Uma solicitação editorial pode produzir uma Versão preparada e uma Publicação por destino. Cada Publicação possui chave de idempotência, estado, tentativas, referência externa e métricas independentes.

```text
Conteúdo
  ├── Versão para Canal A → Publicação A → Resultado A
  ├── Versão para Canal B → Publicação B → Resultado B
  └── Versão para Canal C → Publicação C → Resultado C
```

### 14.2 Coordenação

1. Validar a fonte e as capacidades de cada Canal.
2. Preparar versões por destino sem alterar a fonte.
3. Exibir todas as adaptações e contas ao Usuário.
4. Obter confirmação explícita para o conjunto e seus destinos.
5. Criar uma Publicação independente por destino.
6. Executar trabalhos de forma isolada e limitada.
7. Apresentar sucesso parcial sem reverter o que já foi publicado.
8. Coletar resultados sem somar métricas incompatíveis.

### 14.3 Limite do MVP

O desenho suporta múltiplos destinos, mas o MVP implementa um Canal e uma Publicação por confirmação. Não há lote multicanal, adaptação automática para vários Canais ou relatório consolidado no primeiro ciclo.

## 15. Armazenamento

### 15.1 Classificação

| Categoria | Local preferencial | O que a plataforma guarda |
|---|---|---|
| Texto, estrutura e versões | Banco transacional | Fonte editorial, versões e histórico |
| Metadados de arquivo | Banco transacional | Nome, tipo, tamanho, origem, identificador, versão e estado |
| Vídeo e arquivo grande | Serviço conectado pelo Usuário, a escolher após pesquisa | Referência protegida e permissão necessária |
| Imagem ou arquivo pequeno do MVP | Amazon S3 privado, se necessário | Objeto privado e metadados |
| Miniatura derivada | Amazon S3 privado com ciclo de vida | Referência e origem |
| Arquivo temporário para Publicação | Amazon S3 privado com TTL e área lógica por trabalho | Referência de trabalho e prazo de exclusão |
| Segredo | AWS Secrets Manager, protegido por KMS | Apenas referência nos dados de negócio |

### 15.2 Arquivos grandes conectados

- Usuário conecta Google Drive, OneDrive, Dropbox ou serviço equivalente pelo fluxo guiado.
- A plataforma solicita acesso mínimo ao arquivo selecionado ou ao escopo tecnicamente indispensável.
- Persiste identificador estável, provedor, conta, versão, tipo, tamanho, integridade quando disponível e estado de acesso.
- Não persiste endereço público permanente.
- Para Publicar, o worker lê por fluxo temporário ou transferência suportada pelo destino.
- Se precisar de cópia temporária, ela é privada, criptografada, limitada em tamanho e removida automaticamente após sucesso, falha final ou prazo máximo.
- Revogação ou remoção no provedor marca a referência como indisponível sem apagar o Conteúdo.

### 15.3 Validação

Tipo declarado não é confiável. O serviço verifica tamanho, tipo real, nome, integridade e limites do Canal. Arquivos hospedados pela plataforma passam por verificação de conteúdo malicioso compatível com o risco. Pré-visualizações usam URLs curtas e vinculadas ao Usuário autorizado.

### 15.4 Limites

O MVP não é serviço de backup, edição ou transcodificação de vídeos. Não garante disponibilidade de arquivo removido pelo Usuário de um provedor externo.

## 16. Autenticação e autorização

### 16.1 Identidade

Amazon Cognito User Pools é o provedor de identidade gerenciado e atua como emissor OIDC. O identificador local é vinculado ao `sub` estável do provedor; perfil, consentimentos, participação e autorização permanecem no banco da plataforma.

### 16.2 Fluxos

- Web: Authorization Code com PKCE; backend troca e valida o código e mantém sessão em cookie `Secure`, `HttpOnly` e `SameSite`.
- Entrada primária: código de uso único enviado por e-mail, sem senha mantida pela plataforma.
- Mobile futuro: autorização pelo navegador do sistema com proteção para cliente público.
- Recuperação, verificação, limitação e bloqueio são responsabilidade do Cognito, com experiência guiada pelo Tutor.
- Autenticação reforçada é obrigatória para operação administrativa e pode ser exigida para ação de alto risco após análise.

### 16.3 Autorização

- Negação por padrão.
- Toda consulta e comando verifica identidade e participação ativa no Projeto.
- No MVP, papel único de proprietário.
- `projeto_id` da URL ou corpo nunca é aceito como prova de acesso.
- Objetos relacionados são validados como pertencentes ao mesmo Projeto.
- Worker usa identidade própria e escopo por função, não credencial de Usuário.
- Operação administrativa é separada, restrita e auditada.

### 16.4 Contas externas

A autorização de uma Conta Conectada não substitui a sessão da plataforma. Cada token tem escopo, dono, finalidade, vencimento e revogação. O Usuário vê nome da conta e permissões em linguagem simples, nunca o segredo.

## 17. Auditoria

Auditoria é uma trilha de segurança e responsabilidade separada de logs técnicos e métricas de produto.

### 17.1 Eventos mínimos

- criação, recuperação relevante, bloqueio e encerramento de acesso;
- alteração de consentimento e preferência de privacidade;
- criação, entrada e mudança futura de participação em Projeto;
- início, sucesso, falha e revogação de Conta Conectada;
- mudança relevante de estado e congelamento de Versão;
- confirmação, tentativa, resultado e reconciliação de Publicação;
- leitura administrativa excepcional de dados;
- mudança de configuração de Integração, segurança ou retenção;
- exportação e exclusão de dados.

### 17.2 Campos

Identificador do evento, instante confiável, ator, tipo de ator, Projeto, ação, tipo e identificador do recurso, resultado, motivo ou código, origem da requisição de forma minimizada e identificador de correlação.

### 17.3 Proteção

- Escrita por caminho controlado e acesso restrito.
- Não contém token, senha, corpo integral do Conteúdo ou texto livre do Tutor.
- Retenção definida por finalidade e obrigação.
- Alteração e exclusão protegidas; exportação administrativa auditada.
- Horários normalizados e sincronizados.

## 18. Logs e observabilidade

### 18.1 Logs

Logs estruturados incluem ambiente, serviço, versão, severidade, instante, correlação, operação e código de resultado. Identificadores de Usuário e Projeto são pseudonimizados ou reduzidos quando não indispensáveis.

Nunca registrar:

- senha, código de acesso ou segredo;
- token de sessão, Canal, armazenamento ou IA;
- cabeçalho de autorização ou cookie;
- URL assinada completa;
- corpo integral do Conteúdo ou arquivo;
- texto livre do Tutor por padrão;
- dado pessoal sem finalidade operacional aprovada.

### 18.2 Métricas técnicas

- latência, volume e erros por operação;
- sucesso e falha de conexão por Integração;
- profundidade e idade do trabalho mais antigo;
- tempo e resultado de Publicação;
- divergências encontradas pela reconciliação;
- atualização e atraso de métricas externas;
- disponibilidade do banco, armazenamento, identidade e Tutor;
- uso de recurso e saturação.

### 18.3 Rastreamento

Identificador de correlação atravessa cliente, API, outbox, worker e adaptador. Rastreamento não inclui conteúdo ou segredo em atributos. Amostragem e retenção variam por ambiente e risco.

### 18.4 Alertas mínimos

- aumento sustentado de falhas de entrada;
- falha de autorização ou webhook fora do padrão;
- fila parada ou trabalho envelhecido;
- Publicações com estado incerto acima do limite;
- degradação de uma Integração;
- falha de cópia ou restauração;
- aumento de erro de autorização entre Projetos;
- acesso anômalo a segredos ou administração.

## 19. Segurança

Os controles detalhados estão em `docs/050-security/SECURITY.md`. A arquitetura aplica em todos os componentes:

- identidade forte e autorização por Projeto;
- menor privilégio para pessoas, serviços e Integrações;
- criptografia em trânsito e repouso;
- segredos fora de código, banco de negócio e logs;
- validação de entrada e saída;
- proteção contra duplicidade e falsificação de webhooks;
- isolamento de conteúdo não confiável e de sugestões de IA;
- dependências verificadas e ambientes separados;
- cópias, restauração e resposta a incidentes testadas;
- privacidade, retenção e exclusão definidas antes do piloto.

Segurança do Tutor não substitui autorização do backend. Uma resposta correta da IA nunca concede permissão.

## 20. Processamento assíncrono e consistência

### 20.0 Tecnologia inicial

A fila é uma tabela de trabalhos no PostgreSQL, alimentada pelo outbox transacional. Workers concorrentes adquirem lotes pequenos com lock de linha e `SKIP LOCKED`, registram prazo de posse e renovação, e devolvem trabalho abandonado após expiração. Não há broker dedicado no MVP. A idade do trabalho mais antigo, profundidade, tentativas e contenção do banco determinam se a fila deverá migrar para SQS ou equivalente.

### 20.1 Outbox e trabalhos

1. Caso de uso grava mudança de domínio e evento de outbox na mesma transação.
2. Despachante publica ou disponibiliza o trabalho ao worker.
3. Worker adquire o trabalho com prazo de posse e registra tentativa.
4. Adaptador executa a chamada com timeout e idempotência disponível.
5. Resultado atualiza a entidade por transição válida.
6. Trabalho é concluído, reagendado ou enviado para tratamento de falha final.

### 20.2 Garantia

Entrega pode ocorrer mais de uma vez; consumidores devem ser idempotentes. A arquitetura não promete exatamente uma execução através de sistemas externos. Promete uma intenção rastreável, tentativa segura e reconciliação.

### 20.3 Trabalhos do MVP

- publicar e consultar resultado incerto;
- receber e processar webhook;
- coletar métricas essenciais;
- verificar saúde ou vencimento de Conta Conectada;
- remover arquivos temporários;
- gerar derivado pequeno quando indispensável.

Agendamento editorial e lote multicanal não fazem parte do MVP.

## 21. Escalabilidade

### 21.1 Estratégia inicial

- API sem estado com réplicas horizontais.
- Worker escalado por fila e limite do provedor.
- Banco verticalmente dimensionado com índices e consultas medidas.
- Pool de conexões com limites por processo.
- Cache longo no navegador para ativos Web imutáveis e não sensíveis; CDN somente se medições justificarem.
- Cache somente para catálogo e leitura segura; não é fonte de verdade.
- Paginação na Biblioteca, Publicações, Relatórios e auditoria.

### 21.2 Proteção contra sobrecarga

- Limites por identidade, Projeto, IP quando apropriado e Integração.
- Tamanho máximo por entrada e arquivo.
- Concorrência máxima por Conta e Canal.
- Controle de pressão entre fila e adaptadores.
- Prioridade para ação interativa sobre coleta não urgente.
- Degradação: se IA ou Resultados falharem, edição e Biblioteca continuam disponíveis.

### 21.3 Caminho de evolução

Um módulo só se torna serviço separado quando houver necessidade observada de escala, isolamento, equipe ou ciclo de implantação. Candidatos naturais são Publicação/Integrações, processamento de mídia e Tutor. A separação preserva contratos e eventos já definidos.

## 22. Disponibilidade, cópias e recuperação

- Banco gerenciado com cópias automáticas e recuperação pontual.
- Armazenamento com versionamento ou proteção equivalente para objetos próprios quando justificado.
- Infraestrutura e configuração reproduzíveis, sem depender de alteração manual não registrada.
- Restauração de banco e objetos testada antes do piloto e periodicamente.
- Objetivos de recuperação e disponibilidade definidos após conhecer impacto e custo do piloto.
- Falha regional não exige arquitetura multirregional no MVP, mas procedimento de recuperação e comunicação é obrigatório.
- Dependência de arquivo externo é exibida; plataforma não restaura arquivo apagado no armazenamento do Usuário.

## 23. Ambientes e entrega

### 23.1 Ambientes

- **Desenvolvimento:** Node.js 24 LTS; Web, API e worker locais; PostgreSQL e emuladores necessários em contêineres; dados fictícios e adaptadores simulados. Nenhum segredo ou dado produtivo é permitido.
- **Homologação:** conta AWS exclusiva em `sa-east-1`, mesma topologia lógica de produção em capacidade mínima, Integrações e contas oficiais de teste, dados sintéticos e telemetria própria.
- **Produção:** conta AWS exclusiva em `sa-east-1`, dados e credenciais reais, entrada pela borda aprovada e acesso operacional restrito.

Cada ambiente possui banco, armazenamento, chaves, clientes externos, callbacks e telemetria separados.

Produção executa Next.js, API e worker como tarefas separadas no ECS Fargate, a partir de imagens OCI no ECR. RDS, S3, Cognito, Secrets Manager/KMS e CloudWatch são privados ou restritos conforme sua função. Infraestrutura é declarada em AWS CDK com TypeScript.

### 23.2 Caminho de entrega

- mudança revisada e verificada automaticamente pelo GitHub Actions;
- autenticação da entrega na AWS por OIDC, sem chave de acesso duradoura no repositório;
- migração de dados testada antes da promoção;
- artefatos imutáveis promovidos entre ambientes;
- segredos injetados pelo ambiente;
- ativação gradual de Integrações e funções de risco;
- reversão da aplicação não reverte cegamente dados ou Publicações externas;
- checklist manual para primeira Publicação real do Canal.

### 23.3 Testes arquiteturalmente necessários

- unidade para regras e transições;
- integração com banco e outbox;
- contrato para cada adaptador externo;
- ponta a ponta do Tutor à Publicação em ambiente de teste;
- idempotência, timeout, webhook repetido e reconciliação;
- autorização entre Projetos;
- acessibilidade Web e Mobile;
- restauração, expiração de segredo e resposta a incidente em exercício controlado.

## 24. Limites técnicos do MVP

### Incluído

- um ambiente produtivo em uma região;
- um Usuário proprietário e um Projeto principal na experiência;
- um tipo principal de Conteúdo;
- Biblioteca simples com busca e filtro no banco;
- um Canal de Publicação e um adaptador;
- uma Conta Conectada ativa por fluxo principal;
- Publicação imediata e processamento assíncrono confiável;
- coleta de métricas essenciais e um Relatório simples;
- Tutor guiado por regras, sem IA generativa habilitada;
- Web responsivo como cliente primário sobre API independente;
- arquivos grandes referenciados em armazenamento conectado;
- auditoria e observabilidade dos eventos críticos.

### Fora do MVP

- microserviços e malha de serviços;
- múltiplas regiões ativas;
- múltiplos Canais em produção e publicação simultânea;
- Agendamento, Campanha e colaboração avançada;
- busca externa dedicada e armazém analítico;
- transcodificação ou edição profissional de vídeo;
- cópia permanente de grandes arquivos por padrão;
- uso autônomo de ferramentas pelo Tutor;
- treinamento de modelo com dados do Usuário;
- relatório comparativo multicanal;
- operação offline completa;
- desenvolvimento de um provedor próprio de identidade, IA ou armazenamento;
- aplicativo Mobile;
- Redis, broker dedicado e Kubernetes.

## 25. Critérios para avançar

### 25.1 Início da implementação do núcleo

- baseline de `APPROVED-DECISIONS.md` aceita pela equipe;
- repositório, módulos e contratos organizados de acordo com o monólito modular;
- ambientes locais reproduzíveis sem dados ou segredos reais;
- estados, regras de autorização, idempotência, auditoria e campos proibidos de log tratados como critérios de cada incremento;
- adaptadores externos definidos por contrato e substituíveis por simuladores.

Segmento, Canal e armazenamento conectado não impedem o início de Identidade, Projeto, Tutor determinístico, Conteúdo, Biblioteca, outbox, Publicação e observabilidade. Impedem ligar o caminho a uma conta real e liberar o piloto.

### 25.2 Primeira versão utilizável para piloto

A versão somente é liberável quando satisfizer cumulativamente ENG-15 em `APPROVED-DECISIONS.md`, incluindo recorte de produto aprovado, provas reais de Canal e armazenamento, ciclo completo, acessibilidade, segurança, privacidade, restauração, alertas e teste com representantes do público.

## 26. Decisões abertas

As decisões técnicas que impediam iniciar o núcleo foram fechadas na Sprint 5. Permanecem abertas, com evidência e trabalho permitido definidos em `OPEN-DECISIONS.md`: segmento e tipo de Conteúdo; Canal, formato e métricas; armazenamento conectado e limites de arquivo; retenção e fundamento; metas operacionais e orçamento; responsáveis; e limiares do piloto.

Depois da primeira versão utilizável serão reavaliados IA generativa, Mobile, broker dedicado, multicanal, Agendamento, Campanha, colaboração, busca externa, armazém analítico, microserviços e múltiplas regiões.

## 27. Riscos técnicos

| ID | Risco | Impacto | Mitigação inicial |
|---|---|---|---|
| RT-01 | API do Canal não permitir publicação ou métricas esperadas | Inviabiliza o ciclo prometido | Escolher Canal somente após prova técnica completa |
| RT-02 | Mudança de política, revisão ou limite externo | Interrompe conexão ou Publicação | Adaptador isolado, monitoramento, capacidade declarada e plano de degradação |
| RT-03 | Token externo vazado | Acesso indevido a conta do Usuário | Cofre, criptografia, escopo mínimo, rotação, redação e resposta a incidente |
| RT-04 | Publicação duplicada após timeout | Dano público e perda de confiança | Idempotência, estado persistido e reconciliação antes de repetir |
| RT-05 | Arquivo grande indisponível ou removido | Publicação falha sem perda do texto | Verificação antes de confirmar, referência de versão e erro recuperável |
| RT-06 | Cópia temporária permanecer além do necessário | Aumenta exposição e custo | TTL obrigatório, limpeza automática, monitoramento e auditoria |
| RT-07 | Tutor ou futura IA induzir ação incorreta | Conteúdo ou destino errado | Regras controladas, confirmação humana e autorização no backend |
| RT-08 | Instrução maliciosa no Conteúdo influenciar o Tutor ou futura IA | Exfiltração ou comando indevido | Tratar Conteúdo como dado; antes de IA, separar instruções, restringir ferramentas e validar saída |
| RT-09 | Falha de isolamento entre Projetos | Exposição de dados | Autorização em cada caso de uso, testes negativos e defesa no banco quando viável |
| RT-10 | Logs capturarem Conteúdo ou segredos | Incidente de privacidade | Campos permitidos, redação central e testes de telemetria |
| RT-11 | Mobile ser introduzido antes de evidência e duplicar regras | Comportamentos divergentes e defeitos | Manter Web como único cliente do MVP e backend autoritativo |
| RT-12 | Monólito acumular acoplamento | Evolução e escala difíceis | Limites de módulo, testes de arquitetura e eventos internos |
| RT-13 | Banco ou fila perder trabalho | Publicação ausente ou estado incoerente | Outbox transacional, cópias e reconciliação periódica |
| RT-14 | Métricas externas atrasadas ou incompatíveis | Recomendação enganosa | Fonte e período explícitos, estado parcial e sem agregação indevida |
| RT-15 | Custos de mídia, chamadas externas ou infraestrutura crescerem | Piloto financeiramente inviável | Orçamento, limites por Projeto e medição; IA permanece desabilitada |

## 28. Critérios de revisão

Esta arquitetura deve ser revisada quando Canal, formato, armazenamento conectado ou metas operacionais forem escolhidos; quando uma prova contrariar o cliente Web, Cognito ou outra decisão aprovada; quando um risco de segurança alterar o fluxo; antes de habilitar IA; ou quando métricas reais justificarem separar componente ou adicionar infraestrutura.
