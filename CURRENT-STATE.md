# Estado Atual

**Fase:** Sprint 5 — Decisões de Engenharia do MVP

**Atualizado em:** 20 de julho de 2026

**Natureza deste registro:** baseline de engenharia aprovada; implementação ainda não iniciada

## Resumo executivo

O produto possui uma definição integrada de descoberta, domínio, experiência, MVP, arquitetura técnica e segurança. A direção permanece: oferecer a pessoas com pouca familiaridade tecnológica uma única porta de entrada guiada pelo Tutor para configurar contas, criar e organizar Conteúdos, Publicar e compreender resultados.

A Sprint 5 fechou a baseline necessária para iniciar a implementação do núcleo: Web responsivo; TypeScript sobre Node.js 24 LTS; Next.js; NestJS; PostgreSQL com Prisma; Amazon Cognito; outbox e fila no PostgreSQL; AWS em `sa-east-1`; imagens OCI no ECS Fargate; S3, Secrets Manager/KMS e CloudWatch; e Tutor determinístico sem IA generativa. Mobile e IA foram adiados.

Ainda não há software funcional, prova técnica documentada, protótipo validado, Canal escolhido ou controles de segurança implementados. As escolhas de engenharia liberam a implementação com adaptadores simulados; não provam viabilidade do Canal, adequação ao público, segurança operacional ou prontidão para piloto.

## Entregas acumuladas

### Sprint 1 — Product Discovery

- missão, visão e proposta de valor;
- público e hipóteses iniciais;
- backlog e roadmap orientados a resultados;
- Blueprint inicial.

### Sprint 2 — Domain Model

- Conteúdo definido como entidade central;
- entidades, relações, cardinalidades, estados e invariantes;
- arquitetura funcional e modelo de dados do domínio;
- recorte do primeiro ciclo completo.

### Sprint 3 — UX e Fluxo do Produto

- onboarding e configuração assistida;
- tela principal, criação e Biblioteca;
- Publicação e acompanhamento de resultados;
- Tutor como porta de entrada com a pergunta “O que você quer fazer hoje?”;
- estados vazios, falhas, retomada, acessibilidade e testes de UX.

### Sprint 4 — Arquitetura Técnica do MVP

- visão de componentes e unidades de implantação;
- backend, Frontend Web, Mobile, banco e Biblioteca;
- Tutor, Orquestrador e processamento assíncrono;
- Integrações, armazenamento conectado e Publicação evolutiva;
- autenticação, autorização, auditoria, logs e observabilidade;
- segurança, privacidade, escalabilidade, recuperação e limites do MVP;
- decisões abertas e riscos técnicos e de segurança.

### Sprint 5 — Decisões de Engenharia do MVP

- stack, linguagem, frameworks, banco e ORM aprovados;
- identidade, fila, armazenamento próprio e Publicação aprovados;
- estratégia determinística de IA definida para a primeira versão;
- desenvolvimento, homologação, produção e entrega definidos;
- critérios cumulativos da primeira versão utilizável definidos;
- decisões dependentes de descoberta, prova externa e governança mantidas abertas com gatilhos explícitos.

## Princípios estabelecidos

- Conteúdo é a entidade central e fonte editorial do ciclo.
- Tutor é a única porta de entrada para o Usuário.
- Tutor orienta; Orquestrador coordena; módulos de domínio autorizam e executam.
- Usuário nunca interage com chamadas, credenciais ou erros técnicos de Integrações.
- Publicação exige confirmação explícita de Conteúdo, Versão, conta e destino.
- Uma Publicação independente representa cada destino.
- Arquivos grandes permanecem preferencialmente em Google Drive, OneDrive, Dropbox ou serviço equivalente conectado.
- A plataforma guarda principalmente metadados, relacionamentos, histórico, configurações, permissões e conhecimento.
- MVP usa arquitetura modular simples e não adota microserviços sem necessidade observada.
- Falhas externas usam estado persistente, idempotência e reconciliação.
- IA é opcional no MVP e nunca recebe autorização autônoma para ação externa.

## Arquitetura de referência atual

| Área | Direção definida | Estado |
|---|---|---|
| Clientes | Next.js Web responsivo primário; Mobile posterior | Aprovado; não implementado |
| Linguagem | TypeScript estrito sobre Node.js 24 LTS | Aprovado; não implementado |
| Backend | NestJS em monólito modular, com API e worker separados | Aprovado; não implementado |
| Banco e ORM | RDS for PostgreSQL/PostgreSQL local com Prisma e SQL explícito controlado | Aprovado; não implementado |
| Biblioteca | Projeção paginada e pesquisável do Conteúdo no banco | Definido conceitualmente |
| Tutor | Fachada guiada e determinística; IA generativa desabilitada | Aprovado; não implementado |
| Orquestrador | Coordena jornada e operações duráveis sem contornar domínio | Definido conceitualmente |
| Integrações | Adaptadores por capacidade, com credenciais protegidas | Contrato definido; provedores não escolhidos |
| Publicação | Versão congelada, idempotência, worker e reconciliação | Fluxo definido; não provado |
| Fila | Outbox e trabalhos no PostgreSQL, consumidos pelo worker | Aprovado; não implementado |
| Armazenamento | S3 privado para pequenos/temporários; referência externa para grandes | Estratégia aprovada; provedor conectado e limites abertos |
| Identidade | Amazon Cognito OIDC, código por e-mail e sessão Web de servidor | Aprovado; não testado |
| Nuvem e entrega | AWS `sa-east-1`, ECR, ECS Fargate e GitHub Actions por OIDC | Aprovado; não provisionado |
| Auditoria | Trilha separada de logs e analytics | Eventos mínimos definidos |
| Logs | CloudWatch, estruturados, correlacionados e sem Conteúdo ou segredos | Solução aprovada; campos e alertas a detalhar |
| Segurança | Menor privilégio, isolamento por Projeto e confirmação humana | Baseline definida; não implementada |
| Escala | API e worker horizontais; banco medido antes de separar serviços | Caminho definido |

## Limite atual do MVP

### Incluído

- um Usuário proprietário e um Projeto principal;
- um tipo principal de Conteúdo;
- Biblioteca simples;
- um Canal e uma Conta Conectada no caminho principal;
- Publicação imediata;
- métricas essenciais e Relatório simples;
- Tutor baseado em regras, sem IA generativa habilitada;
- armazenamento conectado para arquivo grande;
- auditoria e observabilidade dos eventos críticos.

### Excluído

- múltiplos Canais ativos e lote multicanal;
- Agendamento, Campanha e colaboração;
- microserviços e operação multirregional ativa;
- transcodificação e edição profissional de vídeo;
- cópia permanente de arquivo grande por padrão;
- Tutor com ferramentas autônomas;
- relatório comparativo avançado;
- operação offline completa.

## Evidências que ainda não existem

- entrevistas e síntese que selecionem segmento prioritário;
- teste de usabilidade do fluxo completo;
- validação do Web responsivo e de sua forma de distribuição nos dispositivos reais;
- Canal, formato e métricas do piloto;
- prova de autorização, Publicação, consulta de estado, webhook e revogação;
- prova do armazenamento conectado e do tratamento de arquivo removido;
- prova de usabilidade, recuperação, sessão e bloqueio no Amazon Cognito;
- evidência de que o Tutor determinístico atende ao piloto; provedor de IA não é necessário para a primeira versão;
- política aprovada de privacidade, retenção, exclusão e incidentes;
- objetivos de disponibilidade, recuperação, custo e capacidade;
- implementação, testes, restauração ou exercício de incidente.

## Decisões abertas prioritárias

1. Segmento, caso de uso e tipo de Conteúdo inicial.
2. Canal, formato, permissões e métricas essenciais.
3. Primeiro armazenamento conectado.
4. Limites de arquivos próprios, grandes e temporários.
5. Retenção, exclusão, fundamento e subprocessadores.
6. Objetivos de disponibilidade, recuperação, capacidade e orçamento.
7. Responsáveis por segurança, privacidade, suporte e incidentes.
8. Limiares quantitativos de validação do piloto.

IA generativa, Mobile, broker dedicado, multicanal, Agendamento, Campanha, colaboração, busca externa, microserviços e múltiplas regiões estão adiados para depois da primeira versão utilizável.

## Riscos atuais

| Risco | Impacto | Tratamento imediato |
|---|---|---|
| Implementação avançar além do núcleo antes de validar problema e Canal | Construção correta do produto errado | Usar simuladores e impedir compromisso com formato ou Integração real antes da evidência |
| API do Canal não suportar a promessa | Ciclo do MVP inviável | Provar autorização, envio, estado, métricas e revogação |
| Web responsivo não atender ao contexto do público | Bloqueio de instalação, mídia ou retorno de autorização | Testar em dispositivos reais; Mobile só será reconsiderado com evidência |
| Token externo ser exposto | Ação indevida na conta do Usuário | Cofre, menor escopo, rotação, redação e exercício de incidente |
| Timeout causar Publicação duplicada | Dano público e perda de confiança | Idempotência e reconciliação antes de repetir |
| Arquivo grande externo ficar indisponível | Publicação bloqueada | Verificação prévia, referência de versão e erro recuperável |
| Cópia temporária persistir | Exposição e custo | TTL, limpeza automática e monitoramento |
| Tutor ou futura IA contornar confirmação | Ação não autorizada | Tutor inicial sem ferramentas autônomas e autorização sempre no backend |
| Falha de isolamento entre Projetos | Vazamento de dados | Escopo obrigatório, testes negativos e acesso mínimo |
| Logs capturarem Conteúdo ou segredo | Incidente de privacidade | Campos permitidos, redação e teste de telemetria |
| Retenção e fundamento indefinidos | Bloqueio legal e risco ao Usuário | Revisão de privacidade antes do piloto |
| Dependência externa degradar | Jornada parcial ou parada | Adaptadores, worker, circuitos, estado e degradação guiada |
| Fila no banco competir com transações | Latência e bloqueio de API ou worker | Lotes pequenos, índices, métricas de contenção e gatilho para broker dedicado |
| Concentração na AWS elevar custo ou lock-in | Piloto caro e migração futura trabalhosa | Orçamento, contêineres, PostgreSQL e contratos de adaptadores portáveis |
| Tutor determinístico ser rígido | Baixa compreensão e valor percebido | Opções guiadas, medição de falhas e experimento de IA somente após evidência |

## Maturidade por área

| Área | Estado | Condição para avançar |
|---|---|---|
| Problema e público | Hipóteses documentadas | Evidência de pesquisa e segmento escolhido |
| Domínio | Modelo completo inicial | Validar conceitos e ajustar ao Canal real |
| UX | Fluxos completos documentados | Protótipo testado com Usuários iniciantes |
| MVP | Recorte e critérios da primeira versão utilizável definidos | Fechar recorte de produto, métricas e provas externas |
| Arquitetura técnica | Baseline tecnológica aprovada | Implementar núcleo e validar contratos com provas |
| Segurança | Baseline e ameaças iniciais | Implementar, testar e revisar controles |
| Operação | Ambientes definidos conceitualmente | Provisionar, definir metas, alertas e responsáveis |
| Implementação | Não iniciada | Cumprir critérios de entrada |

## Critério para iniciar implementação do MVP

A implementação do núcleo pode começar a partir desta baseline, usando dados fictícios e adaptadores simulados. Cada incremento deve incluir autorização por Projeto, testes proporcionais ao risco, auditoria, logs seguros, acessibilidade e tratamento de falha.

A versão não pode ser liberada para piloto até que segmento, Canal, formato, métricas e armazenamento conectado estejam aprovados por evidência; privacidade e metas operacionais estejam definidas; Integrações críticas tenham prova; e todos os critérios de ENG-15 em `APPROVED-DECISIONS.md` sejam atendidos.
