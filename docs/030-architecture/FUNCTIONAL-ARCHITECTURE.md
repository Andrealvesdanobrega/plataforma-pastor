# Arquitetura Funcional

**Versão:** 0.2

**Fase:** Sprint 2 — Domain Model

**Atualizado em:** 19 de julho de 2026

## 1. Objetivo

Definir o modelo funcional do produto: entidades, responsabilidades, relacionamentos, estados e fluxos que sustentam a jornada do usuário. Este documento descreve o comportamento do domínio sem determinar banco de dados, framework ou provedor de infraestrutura.

## 2. Princípio central do domínio

O **Conteúdo é a entidade central do sistema**.

Todo o ciclo do produto existe para permitir que um Conteúdo seja criado, organizado, preparado para um Canal, publicado por uma Conta Conectada, acompanhado por Relatórios e usado pelo Tutor para orientar o próximo passo.

- Usuários são autores ou responsáveis por Conteúdos.
- Projetos definem o contexto em que Conteúdos são produzidos.
- A Biblioteca de Conteúdos organiza e apresenta Conteúdos.
- Campanhas podem agrupar Conteúdos em torno de uma iniciativa.
- Canais determinam regras e formatos possíveis.
- Contas Conectadas autorizam destinos reais nesses Canais.
- Integrações executam comunicação com serviços externos.
- Agendamentos definem quando Conteúdos devem ser enviados.
- Publicações registram cada distribuição de uma versão de Conteúdo.
- Relatórios transformam resultados das Publicações em informação.
- O Tutor usa contexto, progresso e resultados para orientar o Usuário.

Criar ou salvar um Conteúdo nunca provoca publicação automaticamente. Toda publicação deve apontar para uma versão identificável do Conteúdo, uma Conta Conectada e uma decisão explícita do Usuário.

## 3. Visão funcional

```text
Usuário
  ├── configura seu perfil
  ├── participa de Projeto
  └── interage com Tutor
          │
          ▼
Projeto ── Biblioteca de Conteúdos
  │                 │
  ├── Campanha? ────┤
  │                 ▼
  │              CONTEÚDO
  │             ┌────┼───────────┐
  │             ▼    ▼           ▼
  │       Agendamento        Publicação
  │             │                │
  └── Conta Conectada ─ Canal ─ Integração
                                   │
                                   ▼
                                Relatório
                                   │
                                   ▼
                                  Tutor
```

## 4. Atores

| Ator | Responsabilidade |
|---|---|
| Usuário | Configura o ambiente, conecta contas, cria, organiza, revisa, publica e acompanha Conteúdos |
| Tutor | Orienta a jornada, explica pendências, sugere ações e apoia o aprendizado sem decidir pelo Usuário |
| Canal externo | Autoriza contas, recebe publicações e disponibiliza estados e métricas |
| Operação da plataforma | Mantém integrações, políticas, observabilidade, segurança e suporte |

No MVP há um único papel funcional: proprietário. Colaboração e papéis adicionais dependem de validação posterior.

## 5. Entidades principais

### 5.1 Usuário

**Finalidade:** representar a pessoa que acessa a plataforma, controla seus dados e realiza ações no produto.

**Informações principais:** identificador, nome, dados de acesso, idioma, fuso horário, nível de familiaridade digital, objetivo, público, preferências, consentimentos, estado do onboarding e datas de criação e atualização.

**Relacionamentos:**

- participa de um ou mais Projetos;
- cria e atualiza Conteúdos;
- autoriza Contas Conectadas;
- solicita Publicações e Agendamentos;
- interage com o Tutor;
- consulta Relatórios.

### 5.2 Projeto

**Finalidade:** delimitar um contexto de trabalho, como uma igreja, negócio, marca pessoal ou iniciativa criativa.

**Informações principais:** identificador, nome, descrição, objetivo, público, identidade editorial, fuso horário, proprietário, estado e datas.

**Relacionamentos:**

- possui Usuários participantes;
- possui uma Biblioteca de Conteúdos;
- reúne Conteúdos, Contas Conectadas, Campanhas e Relatórios;
- fornece contexto ao Tutor.

No MVP, um Usuário possui um Projeto principal. O limite existe desde o domínio para permitir evolução sem misturar dados de contextos diferentes.

### 5.3 Conteúdo

**Finalidade:** representar a unidade editorial central, desde a ideia até o aprendizado após a distribuição.

**Informações principais:** identificador, projeto, autor, título interno, objetivo, público, tipo, mensagem principal, corpo, mídias, versão atual, estado editorial, etiquetas, datas e histórico de versões.

**Relacionamentos:**

- pertence a um Projeto e aparece em sua Biblioteca de Conteúdos;
- pode pertencer a uma Campanha;
- é criado ou editado por Usuários;
- pode possuir nenhum ou vários Agendamentos;
- origina nenhuma ou várias Publicações;
- é adaptado segundo as regras de Canais;
- tem resultados consolidados em Relatórios;
- fornece contexto de orientação e aprendizado ao Tutor.

**Regras essenciais:**

- pode existir sem Canal ou Conta Conectada;
- mantém uma fonte editorial e versões rastreáveis;
- alterações posteriores não modificam uma versão já publicada;
- seu estado editorial resume a jornada, mas não substitui o estado individual de cada Publicação;
- arquivar não apaga histórico, Publicações ou Relatórios.

### 5.4 Biblioteca de Conteúdos

**Finalidade:** organizar e oferecer acesso aos Conteúdos de um Projeto sem duplicar a fonte editorial.

**Informações principais:** projeto proprietário, critérios de ordenação, filtros, visualizações, etiquetas e preferências de exibição.

**Relacionamentos:**

- pertence a um Projeto;
- referencia todos os Conteúdos visíveis desse Projeto;
- usa estados, etiquetas, Campanhas e datas para organizar;
- apresenta pendências indicadas pelo Tutor.

A Biblioteca é uma projeção organizacional do conjunto de Conteúdos. Excluir uma visualização ou filtro não exclui os Conteúdos.

### 5.5 Canal

**Finalidade:** representar um tipo de destino digital suportado pela plataforma, como uma rede social, plataforma de vídeo ou serviço de mensagens.

**Informações principais:** identificador, nome, categoria, capacidades, formatos, limites, métricas disponíveis, requisitos de autorização, estado de disponibilidade e política aplicável.

**Relacionamentos:**

- é suportado por uma Integração;
- possui Contas Conectadas autorizadas por Usuários;
- define requisitos para versões de Conteúdo, Agendamentos e Publicações;
- fornece dados usados por Relatórios.

Canal é o catálogo de um serviço; Conta Conectada é a conta real do Usuário nesse serviço.

### 5.6 Conta Conectada

**Finalidade:** representar o vínculo autorizado entre um Projeto e uma conta externa existente em um Canal.

**Informações principais:** identificador, projeto, canal, identificador externo, nome público da conta, referência protegida das credenciais, permissões, estado da conexão, expiração, última verificação e datas.

**Relacionamentos:**

- pertence a um Projeto e aponta para um Canal;
- é autorizada e revogável por um Usuário;
- usa uma Integração para autenticação e operações externas;
- é destino de Agendamentos e Publicações;
- fornece métricas externas para Relatórios.

Desconectar uma conta impede novas operações, mas preserva Conteúdos e histórico local.

### 5.7 Publicação

**Finalidade:** registrar a intenção aprovada e o resultado da distribuição de uma versão de Conteúdo para uma Conta Conectada.

**Informações principais:** identificador, conteúdo, versão congelada, conta conectada, canal, solicitante, origem imediata ou agendada, estado, instante solicitado, instante publicado, identificador e endereço externos, tentativas e erro sanitizado.

**Relacionamentos:**

- pertence a exatamente um Conteúdo;
- usa exatamente uma Conta Conectada e seu Canal;
- pode ser criada por um Agendamento;
- é executada por uma Integração;
- produz métricas usadas em Relatórios;
- é interpretada pelo Tutor.

Cada destino gera uma Publicação independente. Uma publicação em um Canal não altera o resultado de outra.

### 5.8 Agendamento

**Finalidade:** registrar a decisão de publicar uma versão de Conteúdo em uma Conta Conectada em data e hora futuras.

**Informações principais:** identificador, conteúdo, versão preparada, conta conectada, data e hora, fuso horário, criador, estado, recorrência futura opcional, data de execução, cancelamento e falha.

**Relacionamentos:**

- pertence a um Conteúdo;
- aponta para uma Conta Conectada;
- é criado ou cancelado por um Usuário;
- quando executado, origina uma Publicação;
- pode ser acompanhado pelo Tutor e exibido na Biblioteca.

Reagendar ou cancelar deve ser possível enquanto o envio não tiver começado. O MVP pode limitar-se a publicação imediata, mantendo Agendamento modelado para evolução.

### 5.9 Tutor

**Finalidade:** conduzir o Usuário pelo fluxo, reduzir complexidade, explicar estados e transformar resultados em próximos passos compreensíveis.

**Informações principais:** contexto da jornada, etapa atual, pendências, recomendações, explicações, interações, feedback do Usuário e limites de atuação.

**Relacionamentos:**

- orienta um Usuário dentro de um Projeto;
- consulta estado de onboarding, Contas Conectadas, Conteúdos, Agendamentos, Publicações e Relatórios;
- registra recomendações e interações;
- aponta ações na Biblioteca de Conteúdos;
- nunca publica, conecta contas ou altera Conteúdos sem ação autorizada do Usuário.

Funcionalmente, o Tutor combina um serviço de orientação com registros persistentes de interação e recomendação. Ele não é proprietário dos objetos orientados.

### 5.10 Campanha — opcional

**Finalidade:** agrupar Conteúdos de um Projeto em torno de um objetivo, tema, período ou iniciativa comum.

**Informações principais:** identificador, projeto, nome, objetivo, público, período, estado, etiquetas e metas descritivas.

**Relacionamentos:**

- pertence a um Projeto;
- agrupa nenhum ou vários Conteúdos;
- pode orientar filtros da Biblioteca e consolidação em Relatórios;
- fornece contexto ao Tutor.

Campanha não é necessária para criar ou publicar Conteúdo e fica fora do MVP inicial.

### 5.11 Integração

**Finalidade:** encapsular a comunicação entre a plataforma e um serviço externo.

**Informações principais:** identificador, provedor, versão, canais suportados, capacidades, credenciais da plataforma, estado operacional, limites, política de novas tentativas, última verificação e eventos de falha.

**Relacionamentos:**

- implementa operações de um ou mais Canais;
- cria, verifica, renova e revoga Contas Conectadas;
- envia Publicações e consulta seus estados;
- consulta métricas para Relatórios;
- informa indisponibilidades à operação e ao Tutor.

Integração é componente da plataforma; Conta Conectada é autorização do Usuário. Credenciais de ambas têm ciclo e escopo separados.

### 5.12 Relatório

**Finalidade:** apresentar resultados de Conteúdos e Publicações em um período, com contexto e limitações compreensíveis.

**Informações principais:** identificador, projeto, escopo, período, métricas, fonte, data da última atualização, comparações permitidas, resumo e recomendações relacionadas.

**Relacionamentos:**

- pertence a um Projeto;
- agrega dados de Conteúdos e suas Publicações;
- pode ser filtrado por Canal, Conta Conectada ou Campanha;
- recebe dados por Integrações;
- alimenta explicações e recomendações do Tutor;
- é consultado por Usuários.

O Relatório não deve tratar métricas diferentes como equivalentes sem regra explícita nem afirmar causalidade não demonstrada.

## 6. Mapa de relacionamentos

| Origem | Cardinalidade | Destino | Regra |
|---|---:|---|---|
| Usuário | N:N | Projeto | No MVP, simplificado para um proprietário e um Projeto principal |
| Projeto | 1:1 | Biblioteca de Conteúdos | Cada Projeto possui uma Biblioteca lógica |
| Projeto | 1:N | Conteúdo | Todo Conteúdo pertence a um Projeto |
| Projeto | 1:N | Conta Conectada | Conexões não são compartilhadas implicitamente entre Projetos |
| Projeto | 1:N | Campanha | Campanha é opcional |
| Conteúdo | N:1 opcional | Campanha | Um Conteúdo pode existir sem Campanha |
| Conteúdo | 1:N | Agendamento | Pode não haver Agendamento |
| Conteúdo | 1:N | Publicação | Pode haver várias por Canal, conta, versão e tentativa aprovada |
| Canal | 1:N | Conta Conectada | Cada conta aponta para um Canal |
| Integração | 1:N | Canal | Uma implementação pode suportar mais de um Canal relacionado |
| Conta Conectada | 1:N | Publicação | Cada Publicação tem um único destino |
| Agendamento | 0:1 | Publicação | Só existe Publicação quando a execução começa |
| Relatório | N:N | Conteúdo | Agrega um conjunto definido de Conteúdos |
| Tutor | N:1 | Usuário e Projeto | Orienta a jornada dentro do contexto selecionado |

## 7. Estados principais

### 7.1 Conteúdo

```text
Ideia → Rascunho → Precisa de revisão → Pronto
            ↑              │              │
            └──────────────┘              ├→ Agendado
                                         └→ Em publicação
                                                │
                                      ┌─────────┴─────────┐
                                      ▼                   ▼
                                  Publicado            Com falha

Estado ativo → Arquivado → estado ativo anterior
```

“Agendado”, “Publicado” e “Com falha” são resumos editoriais. O estado preciso permanece no Agendamento ou na Publicação correspondente.

### 7.2 Conta Conectada

```text
Não conectada → Autorizando → Ativa → Requer reconexão → Ativa
                     │          │
                     ▼          └→ Desconectada
                   Falhou
```

### 7.3 Agendamento

```text
Rascunho → Confirmado → Em execução → Executado
              │              │
              ├→ Cancelado   └→ Falhou
              └→ Reagendado → Confirmado
```

### 7.4 Publicação

```text
Aguardando confirmação → Na fila → Processando → Publicada
           │                │           │
           └→ Cancelada     └───────────┴→ Falhou → Nova tentativa
```

## 8. Fluxo principal do usuário

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

### 8.1 Baixar App

1. Usuário encontra a versão oficial compatível com seu dispositivo.
2. Instala e abre o App.
3. O App explica a proposta, requisitos básicos e tratamento de dados antes do cadastro.

**Entidades envolvidas:** Usuário e Tutor.

### 8.2 Configurar

1. Usuário cria seu acesso ou entra.
2. Informa nome, objetivo, público, preferências e fuso horário.
3. Cria ou confirma seu Projeto principal.
4. Tutor apresenta as etapas e registra o progresso do onboarding.

**Entidades envolvidas:** Usuário, Projeto e Tutor.

### 8.3 Conectar Contas

1. Usuário escolhe um Canal disponível.
2. A plataforma explica permissões e benefício da conexão.
3. Integração conduz a autorização externa.
4. Conta Conectada registra a identidade externa, permissões e saúde, sem expor credenciais.
5. Tutor confirma o sucesso ou mostra como corrigir a falha.

**Entidades envolvidas:** Usuário, Projeto, Canal, Conta Conectada, Integração e Tutor.

### 8.4 Criar Conteúdo

1. Usuário registra uma ideia, objetivo, público e mensagem.
2. Tutor sugere estrutura ou requisitos, sempre como opção.
3. Usuário edita, adiciona mídia e aprova as alterações.
4. O Conteúdo é salvo como fonte editorial versionada.

**Entidades envolvidas:** Usuário, Projeto, Conteúdo e Tutor.

### 8.5 Organizar Biblioteca

1. Biblioteca apresenta Conteúdos por estado, atualização, etiqueta ou Campanha quando disponível.
2. Usuário localiza, filtra, arquiva, retoma ou prepara um Conteúdo.
3. Tutor destaca pendências e o próximo passo.

**Entidades envolvidas:** Biblioteca de Conteúdos, Conteúdo, Projeto, Campanha opcional e Tutor.

### 8.6 Publicar

1. Usuário seleciona Conteúdo, versão e Conta Conectada.
2. A plataforma valida regras do Canal e apresenta a prévia e o destino.
3. Usuário publica imediatamente ou, quando disponível, cria um Agendamento.
4. Publicação congela a versão aprovada e a Integração realiza o envio.
5. O resultado é reconciliado sem duplicar envios incertos.

**Entidades envolvidas:** Conteúdo, Conta Conectada, Canal, Agendamento, Publicação, Integração, Usuário e Tutor.

### 8.7 Acompanhar Resultados

1. Integração coleta estados e métricas permitidas.
2. Relatório associa os dados às Publicações e aos Conteúdos corretos.
3. Usuário visualiza fonte, período, atualização e limitações.

**Entidades envolvidas:** Integração, Publicação, Conteúdo, Relatório, Canal e Conta Conectada.

### 8.8 Aprender com o Tutor

1. Tutor interpreta o progresso e os Relatórios sem prometer resultados.
2. Explica em linguagem simples o que aconteceu.
3. Sugere um próximo passo justificável.
4. Usuário aceita, rejeita ou adapta a recomendação, iniciando um novo ciclo de Conteúdo.

**Entidades envolvidas:** Tutor, Usuário, Projeto, Conteúdo, Publicação e Relatório.

## 9. Contextos funcionais

| Contexto | Entidades dominantes | Responsabilidade |
|---|---|---|
| Identidade e configuração | Usuário, Projeto | Acesso, preferências, consentimentos e contexto de trabalho |
| Tutoria | Tutor, Usuário, Projeto | Progresso, explicações, pendências e recomendações |
| Conteúdo e organização | Conteúdo, Biblioteca, Campanha | Autoria, versões, classificação, localização e planejamento editorial |
| Canais | Canal, Conta Conectada, Integração | Capacidades, autorização e saúde dos vínculos externos |
| Distribuição | Agendamento, Publicação | Aprovação, tempo, envio, estado e recuperação |
| Resultados | Relatório, Publicação, Conteúdo | Coleta, consolidação, explicação e aprendizado |

## 10. Contrato funcional das Integrações

Cada Integração deve declarar:

- Canais e operações suportados;
- autorização, permissões, renovação e revogação;
- formatos, campos, mídias e limites;
- suporte a publicação imediata, agendamento externo, edição ou remoção;
- identificadores de idempotência e consulta de estado;
- métricas, definições e janelas de atualização;
- limites de uso e política de novas tentativas;
- estados e erros traduzíveis para o Usuário;
- requisitos de revisão e conformidade do provedor.

O domínio não presume que todos os Canais oferecem as mesmas operações ou métricas.

## 11. Regras transversais

- O Usuário acessa somente Projetos dos quais participa.
- Conteúdos, conexões e resultados são isolados por Projeto.
- Credenciais externas nunca aparecem na interface, em Relatórios ou em logs.
- Publicação exige confirmação associada ao Usuário, à versão e à Conta Conectada.
- Falhas preservam o Conteúdo e indicam uma ação de recuperação.
- Se o resultado de um envio for desconhecido, o estado externo deve ser consultado antes de reenviar.
- Métricas sempre exibem fonte, período e última atualização.
- Recomendações do Tutor são explicáveis, opcionais e não executam ações externas sozinhas.
- Operações sensíveis geram auditoria com minimização de dados.

## 12. Eventos funcionais observáveis

- App aberto pela primeira vez;
- onboarding iniciado e concluído;
- Projeto configurado;
- conexão iniciada, ativada, falhou, expirou ou foi revogada;
- Conteúdo criado, versionado, considerado pronto, arquivado ou retomado;
- Biblioteca consultada ou filtrada;
- Agendamento confirmado, reagendado, cancelado ou executado;
- Publicação confirmada, iniciada, concluída ou falhou;
- Relatório atualizado ou consultado;
- recomendação apresentada, aceita, adaptada ou rejeitada.

Cada evento precisa de finalidade, propriedades mínimas e retenção definidas antes da implementação.

## 13. Tratamento de falhas críticas

| Situação | Comportamento esperado |
|---|---|
| Credencial expirada | Marcar Conta Conectada como requer reconexão, preservar Conteúdo e orientar o Usuário |
| Canal indisponível | Manter operação rastreável e aplicar nova tentativa segura conforme política |
| Envio com resultado desconhecido | Consultar a Integração antes de permitir novo envio |
| Conteúdo incompatível | Bloquear confirmação, explicar o requisito e apontar a correção |
| Agendamento perdeu validade | Não publicar fora da regra definida; informar e pedir nova decisão |
| Falha na coleta de métricas | Manter último dado com data e sinalizar que não foi atualizado |
| Interrupção de rede | Preservar rascunho e nunca tratar interrupção como confirmação |

## 14. Recorte funcional do MVP

O MVP percorre o fluxo completo com profundidade controlada: um Usuário proprietário, um Projeto principal, uma Biblioteca simples, um tipo de Conteúdo, um Canal, uma Conta Conectada, publicação imediata, um Relatório essencial e tutoria baseada em etapas e regras.

Agendamento, Campanha, colaboração, múltiplos Canais e recomendações avançadas permanecem modelados como evolução, sem ampliar a entrega inicial.

## 15. Decisões pendentes

- plataforma e distribuição inicial do App;
- segmento, Canal e formato do piloto;
- autenticação e recuperação de acesso;
- estrutura final de versões e mídias do Conteúdo;
- uso ou não de Agendamento no MVP;
- estratégia de idempotência da primeira Integração;
- métricas e frequência dos Relatórios;
- origem, explicabilidade e limites das sugestões do Tutor;
- regras de colaboração futuras em Projetos;
- consentimento, retenção, exclusão e suporte aplicáveis.

## 16. Critérios de validação

A arquitetura funcional estará pronta para orientar a implementação quando:

- usuários compreenderem Projeto, Conteúdo, Biblioteca, Canal e Conta Conectada;
- o fluxo completo for validado em protótipo;
- Canal, formato e Integração iniciais forem tecnicamente viáveis;
- estados e falhas críticas forem aceitos;
- autorização, consentimento e publicação estiverem definidos;
- entidades e eventos necessários às métricas do piloto estiverem confirmados.

Mudanças neste documento exigem revisão do modelo de dados e do MVP.
