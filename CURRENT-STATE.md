# Estado Atual

**Fase:** Sprint 6 — Primeiro Fluxo Vertical do MVP

**Atualizado em:** 20 de julho de 2026

**Natureza deste registro:** primeiro fluxo vertical definido; implementação ainda não iniciada

## Resumo executivo

O produto possui uma definição integrada de descoberta, domínio, experiência, MVP, arquitetura técnica e segurança. A direção permanece: oferecer a pessoas com pouca familiaridade tecnológica uma única porta de entrada guiada pelo Tutor para configurar contas, criar e organizar Conteúdos, Publicar e compreender resultados.

A Sprint 5 fechou a baseline necessária para iniciar a implementação do núcleo. A Sprint 6 definiu o primeiro fluxo vertical: um pastor convidado, responsável pela comunicação de uma pequena igreja, publica um aviso textual curto em uma única Página do Facebook que já administra, recebe confirmação e consegue abrir o link da Publicação.

O recorte elimina mídia, armazenamento conectado, IA generativa, Agendamento, colaboração, notificações e múltiplos Canais do primeiro fluxo. Facebook Pages e o segmento pastoral são hipóteses operacionais: ainda não há pesquisa sintetizada, prova técnica, software funcional, protótipo validado ou controles de segurança implementados. A definição libera implementação com simulador, não Publicação real nem piloto.

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

### Sprint 6 — Primeiro Fluxo Vertical do MVP

- primeiro Usuário definido como pastor responsável direto pela comunicação de pequena igreja;
- dor delimitada à insegurança e dependência para publicar um aviso na conta correta;
- objetivo delimitado a uma Publicação textual imediata e confirmada;
- hipótese de Facebook Pages como único Canal do fluxo;
- telas, entidades, dados, Integrações e métricas mínimas definidos;
- mídia, armazenamento conectado e demais capacidades não essenciais removidos do primeiro fluxo;
- primeira versão utilizável especializada sem alterar ENG-01 a ENG-15.

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
- IA generativa não entra na primeira versão e nunca recebe autorização autônoma para ação externa.

## Primeiro fluxo vertical

```text
Convite e código por e-mail
→ configuração mínima da igreja
→ “Publicar um aviso” no Tutor
→ conectar e confirmar uma Página do Facebook
→ escrever e salvar texto
→ retomar pela Biblioteca
→ revisar texto e Página
→ confirmar explicitamente
→ acompanhar Publicação
→ abrir link e entender o próximo passo
```

**Valor entregue:** o aviso aparece na Página correta sem o Usuário precisar dominar a configuração técnica e sem risco de envio implícito ou duplicado.

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
| Integrações | Cognito aprovado; Facebook Pages como hipótese do primeiro Canal | Identidade aprovada; Canal não provado |
| Publicação | Versão congelada, idempotência, worker e reconciliação | Fluxo definido; não provado |
| Fila | Outbox e trabalhos no PostgreSQL, consumidos pelo worker | Aprovado; não implementado |
| Armazenamento | Estratégia aprovada; nenhum arquivo participa do primeiro fluxo | Implementação adiada para mídia futura |
| Identidade | Amazon Cognito OIDC, código por e-mail e sessão Web de servidor | Aprovado; não testado |
| Nuvem e entrega | AWS `sa-east-1`, ECR, ECS Fargate e GitHub Actions por OIDC | Aprovado; não provisionado |
| Auditoria | Trilha separada de logs e analytics | Eventos mínimos definidos |
| Logs | CloudWatch, estruturados, correlacionados e sem Conteúdo ou segredos | Solução aprovada; campos e alertas a detalhar |
| Segurança | Menor privilégio, isolamento por Projeto e confirmação humana | Baseline definida; não implementada |
| Escala | API e worker horizontais; banco medido antes de separar serviços | Caminho definido |

## Limite atual do MVP

### Incluído

- um pastor convidado, proprietário de um Projeto de igreja;
- um tipo de Conteúdo: aviso textual;
- Biblioteca simples para listar, abrir e arquivar;
- hipótese de Facebook Pages e uma Página conectada;
- Publicação imediata;
- confirmação, link e no máximo uma métrica comprovada;
- Tutor baseado em regras, sem IA generativa habilitada;
- auditoria e observabilidade dos eventos críticos.

### Excluído

- múltiplos Canais ativos e lote multicanal;
- mídia, upload e armazenamento conectado;
- Agendamento, Campanha e colaboração;
- microserviços e operação multirregional ativa;
- transcodificação e edição profissional de vídeo;
- cópia permanente de arquivo grande por padrão;
- Tutor com ferramentas autônomas;
- relatório comparativo avançado;
- operação offline completa.
- cadastro público irrestrito, notificações externas e Biblioteca avançada.

## Evidências que ainda não existem

- entrevistas e síntese que validem o pastor responsável pela comunicação;
- teste de usabilidade do fluxo completo;
- validação do Web responsivo e de sua forma de distribuição nos dispositivos reais;
- prova de Facebook Pages, permissões, texto, estado, link e possível métrica;
- prova de autorização, Publicação, consulta de estado, webhook e revogação;
- necessidade real de mídia ou armazenamento conectado, adiados neste fluxo;
- prova de usabilidade, recuperação, sessão e bloqueio no Amazon Cognito;
- evidência de que o Tutor determinístico atende ao piloto; provedor de IA não é necessário para a primeira versão;
- política aprovada de privacidade, retenção, exclusão e incidentes;
- objetivos de disponibilidade, recuperação, custo e capacidade;
- implementação, testes, restauração ou exercício de incidente.

## Decisões abertas prioritárias

1. Validar o segmento pastoral, a frequência da dor e a disposição para Publicação real.
2. Provar Facebook Pages, permissões mínimas, publicação textual, estado, revogação, link e resultado.
3. Confirmar se uma métrica externa simples é necessária e disponível.
4. Retenção, exclusão, fundamento e subprocessadores.
5. Objetivos de disponibilidade, recuperação, capacidade e orçamento.
6. Responsáveis por segurança, privacidade, suporte e incidentes.
7. Limiares quantitativos do piloto depois do primeiro teste real.

Mídia, armazenamento conectado, IA generativa, Mobile, broker dedicado, multicanal, Agendamento, Campanha, colaboração, notificações, Biblioteca avançada, busca externa, microserviços e múltiplas regiões estão adiados.

## Riscos atuais

| Risco | Impacto | Tratamento imediato |
|---|---|---|
| Pastor e aviso textual não representarem dor frequente | Primeiro fluxo não gerar uso recorrente | Entrevistar e observar comportamento antes do piloto |
| Facebook Pages não permitir o contrato mínimo | Ciclo vertical inviável | Provar autorização, texto, estado, link, revogação e revisão externa |
| Pastor não administrar Página elegível | Usuário escolhido não consegue iniciar | Recrutar participante com acesso legítimo e validar elegibilidade antes do convite |
| Conteúdo religioso revelar dado sensível | Exposição de crença, pessoa ou comunidade | Minimização, texto controlado no piloto e proibição em logs/analytics |
| Web responsivo não atender ao contexto do público | Bloqueio de entrada ou retorno de autorização | Testar em dispositivos reais; Mobile só será reconsiderado com evidência |
| Token externo ser exposto | Ação indevida na conta do Usuário | Cofre, menor escopo, rotação, redação e exercício de incidente |
| Timeout causar Publicação duplicada | Dano público e perda de confiança | Idempotência e reconciliação antes de repetir |
| Texto sem mídia ter pouco valor no Canal | Fluxo funciona tecnicamente, mas não resolve a tarefa real | Validar avisos textuais passados; adicionar mídia somente em novo recorte aprovado |
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
| Problema e público | Hipótese vertical definida | Validar pastor, aviso e Página com comportamento real |
| Domínio | Modelo completo inicial | Validar conceitos e ajustar ao Canal real |
| UX | Fluxos completos documentados | Protótipo testado com Usuários iniciantes |
| MVP | Primeiro fluxo vertical e critérios definidos | Provar Canal, protótipo e valor com primeiro Usuário |
| Arquitetura técnica | Baseline tecnológica aprovada | Implementar núcleo e validar contratos com provas |
| Segurança | Baseline e ameaças iniciais | Implementar, testar e revisar controles |
| Operação | Ambientes definidos conceitualmente | Provisionar, definir metas, alertas e responsáveis |
| Implementação | Não iniciada | Cumprir critérios de entrada |

## Critério para iniciar implementação do MVP

A implementação do fluxo vertical pode começar a partir desta baseline, usando pastor, igreja, Página e avisos fictícios e um adaptador simulado. Cada incremento deve incluir autorização por Projeto, testes proporcionais ao risco, auditoria, logs seguros, acessibilidade e tratamento de falha.

A versão não pode ser liberada para piloto até que o segmento pastoral e o aviso textual estejam validados; Facebook Pages tenha prova oficial; privacidade e metas operacionais estejam definidas; e todos os critérios especializados do MVP e ENG-15 em `APPROVED-DECISIONS.md` sejam atendidos.
