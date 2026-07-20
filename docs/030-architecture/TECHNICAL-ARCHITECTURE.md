# Arquitetura Técnica do MVP

**Versão:** 0.1

**Fase:** Sprint 4 — Arquitetura Técnica do MVP

**Atualizado em:** 19 de julho de 2026

**Estado:** arquitetura de referência; implementação ainda não iniciada

## 1. Objetivo

Definir a primeira arquitetura técnica completa para entregar e operar o ciclo do MVP: entrar, configurar, conectar uma conta, criar e organizar Conteúdo, confirmar uma Publicação, acompanhar resultados e receber orientação do Tutor.

A arquitetura traduz o domínio e a experiência aprovados nas Sprints anteriores sem ampliar o produto. Decisões de provedor, linguagem e framework permanecem subordinadas a provas de viabilidade e às decisões abertas registradas ao final.

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
| Estilo de backend | Monólito modular | Menor custo operacional e transações locais, com limites evolutivos claros |
| Processos | API síncrona e worker assíncrono, usando os mesmos módulos | Separa experiência interativa de Integrações lentas sem criar microserviços prematuros |
| Contrato dos clientes | API HTTP versionada; consulta ou fluxo de estado para tarefas longas | Um contrato para Web e Mobile e retomada simples |
| Dados transacionais | Banco relacional PostgreSQL compatível | Integridade, relacionamentos, auditoria e consultas da Biblioteca |
| Busca inicial | Índices e busca textual do próprio banco | Evita motor de busca adicional no MVP |
| Mensageria inicial | Outbox transacional e fila durável simples | Evita perda entre mudança de estado e trabalho assíncrono |
| Tutor | Orientação determinística como base; geração assistida opcional e isolada | Mantém previsibilidade e permite validar valor antes de depender de IA |
| Integrações | Adaptadores por capacidade, atrás de contratos internos | Impede dependência do domínio em detalhes de um provedor |
| Arquivos grandes | Referência a armazenamento conectado pelo Usuário | Reduz custo, duplicação e responsabilidade sobre vídeos |
| Arquivos pequenos | Armazenamento privado da plataforma quando necessário | Suporta avatar, miniatura e mídia pequena do piloto com controle de acesso |
| Publicação | Uma Publicação independente por destino | Permite estado, tentativa e resultado próprios e prepara multicanal |
| Ambientes | Desenvolvimento, homologação e produção isolados | Evita usar contas e dados reais em testes |

Tecnologias e provedores concretos deverão atender a essas decisões; não são considerados aprovados apenas por aparecerem como exemplos neste documento.

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

Aplicação responsiva distribuída por CDN e executada em navegador moderno. Pode ser uma aplicação web progressiva se instalação pelo navegador for útil ao piloto.

### 5.2 Aplicativo Mobile

Cliente instalável para os sistemas móveis escolhidos. Usa o mesmo contrato de experiência e domínio do Web, com recursos próprios apenas onde agregarem valor: armazenamento seguro de sessão, retorno de autorização externa, seleção de arquivo e compartilhamento para o App.

### 5.3 API

Processo sem estado responsável por autenticação da requisição, autorização, comandos e consultas rápidas. Escala horizontalmente e não executa chamada externa longa dentro da resposta quando ela puder bloquear ou duplicar a ação.

### 5.4 Worker

Processo sem interface pública que consome trabalhos duráveis: publicar, reconciliar estado, renovar conexão quando permitido, coletar métricas, gerar miniatura e limpar arquivo temporário. Escala separadamente por tipo e volume de trabalho.

### 5.5 Persistência e serviços gerenciados

Banco relacional, armazenamento privado, cofre de segredos, fila durável quando adotada, monitoramento e entrega dos clientes devem preferir serviços gerenciados para reduzir operação no MVP.

## 6. Backend

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

Web e Mobile compartilham design, contratos e comportamento, mas não devem duplicar todo o esforço antes do piloto. A decisão de qual será o cliente primário e se o segundo será completo, limitado ou posterior permanece aberta. A arquitetura suporta ambos sem exigir dois backends.

## 9. Banco de dados

### 9.1 Tecnologia e topologia

Banco relacional PostgreSQL compatível, preferencialmente gerenciado, em uma região primária no MVP. Alta disponibilidade, cópias automáticas e recuperação pontual são habilitadas conforme o nível de serviço do piloto.

### 9.2 Organização lógica

- Um banco transacional por ambiente.
- Tabelas agrupadas por módulos ou esquemas lógicos, sem um banco por módulo.
- Chaves estrangeiras para relações críticas.
- `projeto_id` presente nas entidades isoladas por contexto.
- Restrições únicas para versão, participação, idempotência e referências externas.
- Estados controlados e transições validadas pelo domínio.
- Dados específicos de Canal podem usar estrutura flexível controlada, sem transformar todo o modelo em documentos sem regra.

### 9.3 Biblioteca e busca

A Biblioteca é uma consulta paginada sobre Conteúdo, versões e estados. O MVP usa índices compostos por Projeto, arquivo, estado e atualização, além de busca textual normalizada em título e campos aprovados. Motor de busca dedicado só é introduzido se medições reais mostrarem necessidade.

### 9.4 Concorrência e consistência

- Edição usa controle otimista por versão para evitar sobrescrita silenciosa.
- Publicação congela uma Versão antes de registrar o trabalho externo.
- Estado de Publicação muda por transições permitidas e comparação de versão.
- Outbox é escrita na mesma transação do fato que precisa gerar trabalho.
- Coleta e Publicação são eventualmente consistentes com serviços externos.

### 9.5 Proteção e ciclo de vida

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
| Gerador assistido opcional | Sugerir texto ou explicação dentro de limites |
| Validador de saída | Conferir formato, escopo, segurança e ação proposta |
| Registro de interação | Guardar resumo minimizado e resposta à recomendação |

### 11.3 Base determinística

O MVP consegue executar a jornada principal com intenções e respostas predefinidas. Um serviço de IA pode ajudar na compreensão de texto livre e na sugestão editorial, mas a indisponibilidade dele não deve impedir acesso à Biblioteca, edição, confirmação de Publicação ou consulta de estados.

### 11.4 Uso seguro de IA

- Conteúdo e instruções do Usuário são dados não confiáveis.
- Contexto enviado é mínimo, autorizado e separado das instruções do sistema.
- Ferramentas disponíveis ao modelo formam uma lista explícita e de baixo risco.
- Saída é estruturada, validada e convertida em proposta, nunca executada diretamente.
- Publicar, conectar, consentir, desconectar, excluir e alterar permissão não são ferramentas autônomas.
- Sugestão editorial precisa de aceitação antes de virar Versão de Conteúdo.
- Provedor, retenção, uso para treinamento e localização de dados devem ser avaliados antes do piloto.

### 11.5 Continuidade e degradação

Se classificação livre falhar, Tutor volta a opções guiadas. Se o gerador estiver indisponível, oferece estrutura simples e edição manual. Se não compreender após nova tentativa, encaminha para etapa ou suporte sem perder o estado.

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
- **IA opcional:** classificação, sugestão e explicação.
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
| Vídeo e arquivo grande | Serviço conectado pelo Usuário | Referência protegida e permissão necessária |
| Imagem ou arquivo pequeno do MVP | Armazenamento privado da plataforma, se necessário | Objeto privado e metadados |
| Miniatura derivada | Armazenamento privado com expiração ou ciclo definido | Referência e origem |
| Arquivo temporário para Publicação | Área efêmera criptografada | Referência de trabalho e prazo de exclusão |
| Segredo | Cofre de segredos | Apenas referência nos dados de negócio |

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

Preferir provedor de identidade gerenciado com protocolos abertos. A plataforma recebe um identificador estável e mantém perfil de produto separado dos dados de autenticação.

### 16.2 Fluxos

- Web: sessão de servidor protegida por cookie seguro.
- Mobile: autorização pelo navegador do sistema com proteção para cliente público.
- Recuperação, verificação e bloqueio são responsabilidade do serviço de identidade, com experiência guiada pelo Tutor.
- Autenticação reforçada para operação administrativa e, quando justificável, para ação de alto risco.

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
- CDN para aplicação Web e ativos públicos não sensíveis.
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

- **Desenvolvimento:** dados fictícios e adaptadores simulados.
- **Homologação:** integrações em contas de teste e validação de fluxo completo.
- **Produção:** dados e credenciais reais, acesso operacional restrito.

Cada ambiente possui banco, armazenamento, chaves, clientes externos, callbacks e telemetria separados.

### 23.2 Caminho de entrega

- mudança revisada e verificada automaticamente;
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
- Tutor guiado por regras, com IA opcional e limitada;
- Web e Mobile suportados por uma API comum, com cliente primário ainda a decidir;
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
- desenvolvimento de um provedor próprio de identidade, IA ou armazenamento.

## 25. Critérios para avançar à implementação

- segmento, Canal, formato e cliente primário do piloto escolhidos;
- prova técnica do Canal cobre autorização, Publicação, estado, métricas e revogação;
- prova de armazenamento cobre seleção, leitura temporária, expiração e arquivo removido;
- arquitetura de identidade e recuperação aprovada;
- ameaça, retenção e base legal revisadas;
- protótipo da UX validado com Usuários;
- contratos dos módulos, adaptadores, estados e erros definidos;
- metas iniciais de disponibilidade, recuperação e custo aprovadas;
- plano de ambientes, auditoria, logs e resposta a incidente pronto.

## 26. Decisões abertas

| ID | Decisão | Por que está aberta | Evidência necessária |
|---|---|---|---|
| TA-01 | Cliente primário: Web, aplicativo Mobile ou entrega combinada | Dois clientes completos ampliam custo antes de validar uso | Pesquisa de dispositivos, instalação e tarefas do segmento |
| TA-02 | Linguagem, framework e organização do repositório | Devem refletir capacidade da equipe e operação escolhida | Equipe, prova curta e critérios de manutenção |
| TA-03 | Provedor de nuvem e região | Impacta custo, residência de dados e serviços gerenciados | Requisitos legais, orçamento e localização do piloto |
| TA-04 | Provedor de identidade e método de entrada | Público pode preferir código, senha ou identidade social | Teste de UX, segurança e recuperação |
| TA-05 | Primeiro Canal e formato | Define contrato, mídia, permissões e métricas | Descoberta e prova oficial da Integração |
| TA-06 | Primeiro armazenamento conectado | Nem todo público usa o mesmo serviço | Pesquisa e prova de seleção/leitura |
| TA-07 | Limite entre arquivo pequeno e grande | Afeta custo, UX e Publicação | Formato do piloto, medição de tamanho e orçamento |
| TA-08 | Uso de IA no Tutor do MVP | Jornada pode ser validada com regras; IA adiciona custo e risco | Teste comparativo de valor, qualidade e privacidade |
| TA-09 | Provedor de IA e política de dados | Termos, retenção e região variam | Avaliação de segurança, privacidade, qualidade e custo |
| TA-10 | Transporte da fila | Banco pode bastar no piloto; serviço dedicado melhora isolamento | Volume, latência, operação e limites medidos |
| TA-11 | Metas de disponibilidade, recuperação e retenção | Dependem de impacto, obrigação e custo | Análise do piloto e política aprovada |
| TA-12 | Canal de notificações do MVP | Pode não ser necessário para publicação imediata | Testes de retomada e expectativa do Usuário |

## 27. Riscos técnicos

| ID | Risco | Impacto | Mitigação inicial |
|---|---|---|---|
| RT-01 | API do Canal não permitir publicação ou métricas esperadas | Inviabiliza o ciclo prometido | Escolher Canal somente após prova técnica completa |
| RT-02 | Mudança de política, revisão ou limite externo | Interrompe conexão ou Publicação | Adaptador isolado, monitoramento, capacidade declarada e plano de degradação |
| RT-03 | Token externo vazado | Acesso indevido a conta do Usuário | Cofre, criptografia, escopo mínimo, rotação, redação e resposta a incidente |
| RT-04 | Publicação duplicada após timeout | Dano público e perda de confiança | Idempotência, estado persistido e reconciliação antes de repetir |
| RT-05 | Arquivo grande indisponível ou removido | Publicação falha sem perda do texto | Verificação antes de confirmar, referência de versão e erro recuperável |
| RT-06 | Cópia temporária permanecer além do necessário | Aumenta exposição e custo | TTL obrigatório, limpeza automática, monitoramento e auditoria |
| RT-07 | Tutor ou IA induzir ação incorreta | Conteúdo ou destino errado | Saída validada, lista de ações, confirmação humana e autorização no backend |
| RT-08 | Instrução maliciosa no Conteúdo influenciar o Tutor | Exfiltração ou comando indevido | Separar instruções e dados, minimizar ferramentas e validar saída |
| RT-09 | Falha de isolamento entre Projetos | Exposição de dados | Autorização em cada caso de uso, testes negativos e defesa no banco quando viável |
| RT-10 | Logs capturarem Conteúdo ou segredos | Incidente de privacidade | Campos permitidos, redação central e testes de telemetria |
| RT-11 | Web e Mobile duplicarem regras | Comportamentos divergentes e defeitos | Backend autoritativo, contrato comum e cliente primário definido |
| RT-12 | Monólito acumular acoplamento | Evolução e escala difíceis | Limites de módulo, testes de arquitetura e eventos internos |
| RT-13 | Banco ou fila perder trabalho | Publicação ausente ou estado incoerente | Outbox transacional, cópias e reconciliação periódica |
| RT-14 | Métricas externas atrasadas ou incompatíveis | Recomendação enganosa | Fonte e período explícitos, estado parcial e sem agregação indevida |
| RT-15 | Custos de IA, mídia ou chamadas crescerem | Piloto financeiramente inviável | Orçamento, limites por Projeto, medição e degradação para regras |

## 28. Critérios de revisão

Esta arquitetura deve ser revisada quando cliente primário, Canal, formato, provedor de identidade, armazenamento ou IA forem escolhidos; quando uma prova contrariar um contrato; quando risco de segurança alterar fluxo; ou quando métricas reais justificarem separar componente ou adicionar infraestrutura.
