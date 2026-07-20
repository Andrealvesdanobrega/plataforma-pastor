# Estado Atual

**Fase:** Sprint 4 — Arquitetura Técnica do MVP

**Atualizado em:** 19 de julho de 2026

**Natureza deste registro:** baseline arquitetural anterior à implementação

## Resumo executivo

O produto possui uma primeira definição integrada de descoberta, domínio, experiência, MVP, arquitetura técnica e segurança. A direção permanece: oferecer a pessoas com pouca familiaridade tecnológica uma única porta de entrada guiada pelo Tutor para configurar contas, criar e organizar Conteúdos, Publicar e compreender resultados.

A Sprint 4 definiu uma arquitetura de referência modular e evolutiva. O backend é proposto como monólito modular, com API e worker separados operacionalmente, banco relacional, outbox para trabalhos duráveis, adaptadores externos isolados e armazenamento de arquivos grandes preferencialmente nos serviços conectados pelo Usuário.

Ainda não há software funcional, prova técnica documentada, protótipo validado, Canal escolhido ou controles de segurança implementados. Esta arquitetura é uma decisão de desenho sujeita às evidências listadas neste documento; não é evidência de viabilidade ou segurança operacional.

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
| Clientes | Web responsivo e Mobile sobre a mesma API; cliente primário a escolher | Definido conceitualmente |
| Backend | Monólito modular com processos de API e worker | Definido conceitualmente |
| Banco | Relacional PostgreSQL compatível, gerenciado e isolado por ambiente | Definido conceitualmente |
| Biblioteca | Projeção paginada e pesquisável do Conteúdo no banco | Definido conceitualmente |
| Tutor | Fachada guiada e determinística, com IA opcional isolada | Definido conceitualmente |
| Orquestrador | Coordena jornada e operações duráveis sem contornar domínio | Definido conceitualmente |
| Integrações | Adaptadores por capacidade, com credenciais protegidas | Contrato definido; provedores não escolhidos |
| Publicação | Versão congelada, idempotência, worker e reconciliação | Fluxo definido; não provado |
| Armazenamento | Referência externa para arquivos grandes; objeto privado para pequenos | Política definida; limite e provedor abertos |
| Identidade | Provedor gerenciado e protocolos abertos | Direção definida; provedor e método abertos |
| Auditoria | Trilha separada de logs e analytics | Eventos mínimos definidos |
| Logs | Estruturados, correlacionados e sem Conteúdo ou segredos | Política definida; solução aberta |
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
- Tutor baseado em regras, com IA opcional;
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
- escolha do cliente primário e forma de distribuição;
- Canal, formato e métricas do piloto;
- prova de autorização, Publicação, consulta de estado, webhook e revogação;
- prova do armazenamento conectado e do tratamento de arquivo removido;
- definição de provedor de identidade e recuperação;
- decisão sobre uso de IA e avaliação do provedor;
- política aprovada de privacidade, retenção, exclusão e incidentes;
- objetivos de disponibilidade, recuperação, custo e capacidade;
- implementação, testes, restauração ou exercício de incidente.

## Decisões abertas prioritárias

1. Segmento, caso de uso, tipo de Conteúdo e Canal inicial.
2. Cliente primário: Web, Mobile ou entrega combinada limitada.
3. Linguagem, framework, organização do código e capacidade da equipe.
4. Nuvem, região e serviços gerenciados.
5. Identidade, método de entrada, recuperação e autenticação reforçada.
6. Primeiro armazenamento conectado e limite entre arquivo pequeno e grande.
7. Uso de IA no Tutor, provedor e política de dados.
8. Transporte de fila inicial e metas de desempenho.
9. Retenção por categoria, fundamento aplicável e subprocessadores.
10. Objetivos de disponibilidade, recuperação e orçamento.
11. Solução de auditoria, logs, métricas e alertas.
12. Responsáveis operacionais por segurança, privacidade e incidentes.

## Riscos atuais

| Risco | Impacto | Tratamento imediato |
|---|---|---|
| Arquitetura avançar antes de validar problema e Canal | Construção correta do produto errado | Fechar descoberta e prova técnica antes de compromisso de implementação |
| API do Canal não suportar a promessa | Ciclo do MVP inviável | Provar autorização, envio, estado, métricas e revogação |
| Dois clientes completos ampliarem escopo | Atraso e superfície de defeitos | Escolher cliente primário pelo contexto do público |
| Token externo ser exposto | Ação indevida na conta do Usuário | Cofre, menor escopo, rotação, redação e exercício de incidente |
| Timeout causar Publicação duplicada | Dano público e perda de confiança | Idempotência e reconciliação antes de repetir |
| Arquivo grande externo ficar indisponível | Publicação bloqueada | Verificação prévia, referência de versão e erro recuperável |
| Cópia temporária persistir | Exposição e custo | TTL, limpeza automática e monitoramento |
| Tutor ou IA contornar confirmação | Ação não autorizada | Ferramentas restritas, saída validada e autorização no backend |
| Falha de isolamento entre Projetos | Vazamento de dados | Escopo obrigatório, testes negativos e acesso mínimo |
| Logs capturarem Conteúdo ou segredo | Incidente de privacidade | Campos permitidos, redação e teste de telemetria |
| Retenção e fundamento indefinidos | Bloqueio legal e risco ao Usuário | Revisão de privacidade antes do piloto |
| Dependência externa degradar | Jornada parcial ou parada | Adaptadores, worker, circuitos, estado e degradação guiada |

## Maturidade por área

| Área | Estado | Condição para avançar |
|---|---|---|
| Problema e público | Hipóteses documentadas | Evidência de pesquisa e segmento escolhido |
| Domínio | Modelo completo inicial | Validar conceitos e ajustar ao Canal real |
| UX | Fluxos completos documentados | Protótipo testado com Usuários iniciantes |
| MVP | Recorte técnico e funcional definido | Fechar decisões prioritárias e métricas |
| Arquitetura técnica | Baseline completa | Provas, escolhas de provedores e contratos detalhados |
| Segurança | Baseline e ameaças iniciais | Implementar, testar e revisar controles |
| Operação | Requisitos iniciais | Definir ambientes, metas, alertas e responsáveis |
| Implementação | Não iniciada | Cumprir critérios de entrada |

## Critério para iniciar implementação do MVP

A implementação pode começar quando segmento, Canal, formato e cliente primário estiverem decididos; protótipo e Integrações críticas tiverem evidência; identidade, dados e segurança estiverem aprovados; contratos dos módulos e adaptadores estiverem fechados; e backlog de implementação possuir critérios de aceite, riscos e observabilidade.
