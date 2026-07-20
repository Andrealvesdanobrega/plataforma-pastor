# Decisões Aprovadas

**Fase:** Sprint 5 — Decisões de Engenharia do MVP

**Atualizado em:** 20 de julho de 2026

**Escopo:** decisões técnicas suficientes para iniciar a implementação, sem substituir as validações de produto, Integração, privacidade e operação exigidas antes do piloto.

## Resumo da baseline

A primeira implementação será uma aplicação Web responsiva em monorepositório, escrita em TypeScript e executada sobre Node.js 24 LTS. O Frontend usará Next.js; API e worker usarão NestJS; os dados transacionais e a fila inicial usarão PostgreSQL; o acesso a dados usará Prisma ORM. A identidade será gerenciada pelo Amazon Cognito. A operação ficará na AWS, região de São Paulo, com contêineres no ECS Fargate, RDS for PostgreSQL, S3 privado, Secrets Manager/KMS e CloudWatch.

O Tutor da primeira versão utilizável será determinístico. IA generativa e aplicativo Mobile não fazem parte dessa versão. O desenho mantém contratos que permitem avaliá-los depois sem alterar as regras centrais do domínio.

## Decisões

### ENG-01 — Stack tecnológica inicial

**Decisão:** monorepositório com `npm workspaces`, Node.js 24 LTS e TypeScript; aplicações separadas para Next.js Web, NestJS API e NestJS worker; PostgreSQL, Prisma ORM, Amazon Cognito e serviços gerenciados da AWS. O cliente primário é Web responsivo. Mobile fica posterior ao MVP.

**Motivo:** entregar o ciclo completo com uma linguagem, um contrato e poucas unidades operacionais, preservando a separação entre experiência interativa e trabalho assíncrono.

**Benefícios:** menor troca de contexto; compartilhamento de tipos e validações de contrato; operação compatível com o monólito modular; implantação independente de Web, API e worker; menor superfície que dois clientes completos.

**Riscos:** acoplamento indevido entre pacotes do monorepositório; dependência forte do ecossistema Node.js; Next.js, NestJS e Prisma exigem políticas coordenadas de atualização.

**Reversibilidade:** alta para organização do repositório e cliente; média para trocar a plataforma de execução depois que módulos e bibliotecas crescerem.

### ENG-02 — Linguagem

**Decisão:** TypeScript em modo estrito para Frontend, backend, worker e contratos; JavaScript sem tipagem fica limitado a configuração inevitável. A versão de Node.js deve permanecer em linha LTS suportada, começando pela 24.

**Motivo:** a jornada possui muitos estados, comandos e integrações; tipagem compartilhada reduz divergência entre cliente, API e worker.

**Benefícios:** contratos verificáveis; refatoração mais segura; uma linguagem para a equipe; ecossistema maduro para Web e Integrações.

**Riscos:** tipos podem dar falsa sensação de validação em tempo de execução; compilação e configuração adicionam disciplina; bibliotecas externas podem ter tipos incompletos.

**Reversibilidade:** média. Módulos ou workers futuros podem usar outra linguagem atrás dos contratos HTTP/eventos, mas reescrever o núcleo terá custo alto.

### ENG-03 — Framework backend

**Decisão:** NestJS sobre Node.js para API e worker, organizados pelos módulos de domínio já definidos. API e worker usam o mesmo núcleo de casos de uso, mas inicializam processos distintos.

**Motivo:** NestJS fornece módulos, injeção de dependência, validação, testes e execução sem servidor HTTP, adequados ao monólito modular com worker separado.

**Benefícios:** limites de módulo explícitos; infraestrutura transversal consistente; reaproveitamento do núcleo; caminho claro para adaptadores e testes de contrato.

**Riscos:** excesso de abstrações e decorators; módulos podem se tornar acoplados se regras de dependência não forem testadas; sobrecarga desnecessária em funções muito simples.

**Reversibilidade:** média. Casos de uso e contratos podem ser preservados, mas controladores, injeção e bootstrap dependerão do framework.

### ENG-04 — Framework frontend

**Decisão:** Next.js com React e TypeScript, App Router e renderização no servidor somente quando trouxer benefício concreto. O MVP entrega Web responsivo e acessível; não entrega aplicativo Mobile.

**Motivo:** atender primeiro navegador e celular pelo mesmo cliente, com roteamento, acessibilidade, formulários e possibilidade de sessão protegida no servidor.

**Benefícios:** uma entrega para desktop e mobile Web; boa divisão por rotas; suporte a contêiner; ativos cacheáveis; menor custo que dois clientes completos.

**Riscos:** cache e renderização híbrida aumentam complexidade; recursos específicos de dispositivo podem ser limitados; atualização do framework pode exigir migração.

**Reversibilidade:** média/alta. A API permanece independente e outro cliente pode substituí-lo ou coexistir depois.

### ENG-05 — Banco de dados

**Decisão:** Amazon RDS for PostgreSQL em produção e homologação; PostgreSQL em contêiner no desenvolvimento. Um banco transacional isolado por ambiente, com `projeto_id`, chaves estrangeiras, restrições e índices definidos no modelo físico.

**Motivo:** o domínio é relacional, exige integridade, transações, auditoria, idempotência e consultas da Biblioteca.

**Benefícios:** transação conjunta entre domínio e outbox; recursos maduros de concorrência e busca textual; backup e recuperação gerenciados; portabilidade entre ofertas PostgreSQL.

**Riscos:** banco concentra dados e fila; consultas ou locks mal desenhados afetam API e worker; RDS cria dependência operacional da AWS.

**Reversibilidade:** média. PostgreSQL é portátil entre provedores; trocar o modelo relacional ou o motor exige migração relevante.

### ENG-06 — ORM

**Decisão:** Prisma ORM e Prisma Migrate. Migrações SQL são versionadas e revisadas; SQL explícito é permitido, encapsulado e testado para busca, locks, outbox e restrições que o ORM não represente adequadamente.

**Motivo:** obter cliente tipado e migrações rastreáveis sem impedir o uso dos recursos essenciais do PostgreSQL.

**Benefícios:** produtividade no CRUD; tipos coerentes com TypeScript; histórico de migração; acesso controlado a SQL quando necessário.

**Riscos:** abstração incompleta para recursos avançados; consulta gerada pode ser ineficiente; SQL explícito reduz portabilidade e exige revisão especializada.

**Reversibilidade:** média. O esquema e as migrações SQL permanecem aproveitáveis, mas repositórios e consultas terão de ser reescritos.

### ENG-07 — Autenticação

**Decisão:** Amazon Cognito User Pools como provedor OIDC. Entrada primária por código de uso único enviado por e-mail; fluxo Authorization Code com PKCE; sessão Web mantida pelo servidor em cookie `Secure`, `HttpOnly` e `SameSite`; autenticação reforçada obrigatória para administração. Perfil e autorização por Projeto permanecem no backend.

**Motivo:** evitar senha própria, manter identidade gerenciada na mesma região da plataforma e separar autenticação de autorização de domínio.

**Benefícios:** recuperação e proteção contra abuso gerenciadas; protocolo aberto; menor responsabilidade sobre credenciais; caminho futuro para passkeys ou federação.

**Riscos:** dependência da entrega de e-mail e do Cognito; código por e-mail herda o risco da conta de e-mail; experiência e acessibilidade precisam de teste; configuração incorreta de callback ou sessão é crítica.

**Reversibilidade:** média/alta. O vínculo local usa o `sub` estável de um IdP OIDC; outro provedor pode ser adotado com migração de identidades e sessões.

### ENG-08 — Sistema de filas

**Decisão:** outbox e fila de trabalhos no PostgreSQL, consumidas pelo processo worker com locks de linha, prazo de posse, tentativas limitadas, `SKIP LOCKED`, idempotência e reconciliação. Redis, SQS, RabbitMQ e brokers equivalentes não entram no MVP.

**Motivo:** garantir que estado de domínio e trabalho durável sejam gravados na mesma transação, sem operar outro serviço antes de haver volume que o justifique.

**Benefícios:** nenhuma janela entre commit e enfileiramento; menos infraestrutura; diagnóstico no mesmo banco; suficiente para um Canal e piloto controlado.

**Riscos:** polling e trabalhos competem com carga transacional; locks, limpeza e reprocessamento exigem cuidado; não é adequado a volume alto ou retenção longa.

**Reversibilidade:** alta. O outbox permanece como origem e pode despachar para SQS ou outro broker quando métricas demonstrarem necessidade.

### ENG-09 — Estratégia de armazenamento

**Decisão:** PostgreSQL guarda textos, versões, metadados e referências. S3 privado guarda apenas objetos pequenos próprios, derivados e cópias temporárias com TTL. Arquivos grandes permanecem no armazenamento conectado do Usuário e são lidos pelo worker somente durante a operação autorizada. Secrets Manager/KMS guarda segredos e chaves, nunca o banco de negócio.

**Motivo:** preservar a plataforma como repositório de conhecimento e evitar cópia permanente de mídia grande.

**Benefícios:** menor custo e responsabilidade sobre mídia; objetos privados com controle e expiração; separação entre dado, arquivo e segredo.

**Riscos:** arquivo externo pode ser removido ou alterado; transferências podem falhar; temporários podem não ser limpos; o primeiro provedor conectado e os limites de tamanho ainda dependem do Canal e do piloto.

**Reversibilidade:** alta para provedor de objeto por meio do adaptador; média para mudar a política depois que objetos próprios forem persistidos.

### ENG-10 — Estratégia de Publicação

**Decisão:** Publicação imediata e assíncrona. A confirmação congela a Versão e cria, na mesma transação, a Publicação e o evento de outbox. O worker usa adaptador de Canal, chave de idempotência, timeout e tentativas classificadas; resultado incerto entra em reconciliação antes de novo envio. Uma Publicação representa um único destino.

**Motivo:** chamadas externas não cabem na resposta interativa e podem terminar sem resultado conhecido.

**Benefícios:** resposta rápida; retomada após fechar a tela; proteção contra duplicidade; trilha completa; troca de Canal isolada pelo adaptador.

**Riscos:** idempotência do provedor pode ser limitada; reconciliação pode não encontrar o envio; bugs de estado podem bloquear ou repetir Publicações.

**Reversibilidade:** alta na escolha do Canal e do transporte; baixa nas invariantes de confirmação, versão congelada e uma Publicação por destino, que são intencionais.

### ENG-11 — Estratégia de IA

**Decisão:** a primeira versão utilizável não usa IA generativa. Tutor, explicações, sugestões de estrutura e próximo passo são produzidos por regras, modelos de texto e opções controladas. Um contrato interno de assistência será mantido, sem chamada a provedor. Experimentos com IA ocorrerão somente depois de aprovação de valor, qualidade, privacidade, custo e segurança.

**Motivo:** validar primeiro a jornada e a tutoria, evitando que custo, retenção de dados e comportamento não determinístico bloqueiem o ciclo.

**Benefícios:** previsibilidade; operação mesmo sem terceiro; testes objetivos; nenhum Conteúdo enviado a provedor de IA; menor custo e superfície de ataque.

**Riscos:** compreensão de texto livre e variedade editorial serão limitadas; Tutor pode parecer rígido; regras podem crescer se tentarem cobrir casos demais.

**Reversibilidade:** alta. O contrato de assistência permite adicionar um adaptador de IA sem conceder ações sensíveis nem mudar a autorização do backend.

### ENG-12 — Ambiente de desenvolvimento

**Decisão:** execução local em Linux, macOS ou Windows com WSL2; Node.js 24 LTS e dependências fixadas; PostgreSQL e emuladores necessários em contêineres; adaptadores simulados para Canal, identidade, armazenamento e IA; dados exclusivamente fictícios. Web, API e worker executam separadamente com configuração local.

**Motivo:** permitir desenvolvimento reprodutível sem credenciais ou dependências externas reais no caminho cotidiano.

**Benefícios:** início rápido; testes determinísticos; ausência de dados produtivos; paridade de banco; trabalho possível sem disponibilidade dos provedores.

**Riscos:** simuladores podem divergir dos serviços reais; contêineres exigem recursos locais; diferenças de arquitetura do host podem aparecer.

**Reversibilidade:** alta. Ferramentas locais podem mudar sem alterar contratos ou produção.

### ENG-13 — Ambiente de homologação

**Decisão:** conta AWS própria, isolada de produção, na região `sa-east-1`; mesma topologia lógica de produção em capacidade mínima; banco, buckets, chaves, Cognito, callbacks, contas externas e telemetria exclusivos; somente dados sintéticos e contas oficiais de teste. Recebe o mesmo artefato imutável candidato a produção.

**Motivo:** validar fluxos e Integrações reais sem misturar credenciais, dados ou destinos produtivos.

**Benefícios:** paridade operacional; teste de migração, idempotência, callback, restauração e segurança; promoção do mesmo artefato.

**Riscos:** custo fixo; serviços de sandbox podem divergir de produção; configuração manual pode causar desvio se não for automatizada.

**Reversibilidade:** alta na capacidade e média no provedor de nuvem.

### ENG-14 — Ambiente de produção e entrega

**Decisão:** conta AWS exclusiva na região `sa-east-1`; Next.js, API e worker em imagens OCI no ECR e tarefas separadas no ECS Fargate; ALB na borda, RDS for PostgreSQL privado, S3 privado, Cognito, Secrets Manager/KMS e CloudWatch. Infraestrutura declarativa em AWS CDK com TypeScript. Entrega contínua pelo GitHub Actions usando OIDC, artefato imutável, promoção homologação–produção, migração controlada e reversão de aplicação sem desfazer cegamente dados ou ações externas.

**Motivo:** operar uma região próxima ao público inicial com serviços gerenciados e sem administrar servidores ou Kubernetes.

**Benefícios:** isolamento por processo; escala independente de API e worker; credenciais curtas na entrega; infraestrutura reproduzível; caminho de desligamento por componente.

**Riscos:** concentração na AWS; custo de Fargate, RDS e região de São Paulo; erro de infraestrutura pode afetar todos os componentes; primeira migração incompatível pode dificultar reversão.

**Reversibilidade:** média. Contêineres e PostgreSQL são portáveis, mas CDK e serviços gerenciados exigem reconstrução ao trocar de nuvem.

### ENG-15 — Primeira versão utilizável

**Decisão:** “primeira versão utilizável” significa uma versão liberável para piloto controlado que atende todos os critérios abaixo, e não apenas uma demonstração técnica.

**Motivo:** impedir que uma interface navegável seja confundida com um ciclo seguro e operacional.

**Benefícios:** critério comum de prontidão; priorização do ciclo completo; segurança, operação e experiência tratadas como parte da entrega.

**Riscos:** dependências externas podem atrasar o marco; critérios qualitativos exigem evidência e responsável; pressão por prazo pode tentar reduzir a verificação.

**Reversibilidade:** baixa como padrão de qualidade; os limiares quantitativos podem ser ajustados antes do piloto com justificativa registrada.

**Critérios cumulativos:**

- segmento, tipo de Conteúdo, Canal, formato e métricas essenciais aprovados;
- Web responsivo permite entrar por código de e-mail, retomar sessão e encerrar acesso;
- um Usuário configura seu Projeto principal e entende o próximo passo do Tutor determinístico;
- Conta de teste e Conta real controlada podem ser conectadas, verificadas, revogadas e reconectadas;
- Usuário cria, salva, reabre, revisa, encontra e arquiva um Conteúdo sem perda silenciosa;
- prévia identifica Versão, Canal e conta; somente “Sim, publicar agora” cria a intenção;
- Publicação real é processada de forma assíncrona, idempotente, auditável e reconciliável, incluindo timeout e falha recuperável;
- resultado disponível mostra fonte, período, última atualização e próximo passo sem promessa de desempenho;
- arquivos, temporários, tokens, segredos, logs e isolamento por Projeto atendem à baseline de segurança;
- caminho principal atende aos testes de acessibilidade definidos para o MVP;
- homologação comprova migração, restauração, alertas críticos e exercício de revogação de token;
- política mínima de privacidade, retenção, exclusão, suporte e incidente está aprovada;
- teste com representantes do público não apresenta problema crítico de UX ou risco crítico de segurança sem tratamento aprovado.

## Regra de mudança

Uma decisão aprovada pode ser revista quando prova técnica, teste com Usuários, custo medido, requisito legal ou incidente contradisser sua premissa. A revisão deve registrar a evidência, o impacto no MVP e o plano de migração; preferência tecnológica isolada não é motivo suficiente.
