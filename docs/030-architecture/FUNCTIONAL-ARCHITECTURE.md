# Arquitetura Funcional

**Versão:** 0.5

**Fase:** Sprint 7 — Modelo de Comunicação Assistida

**Atualizado em:** 20 de julho de 2026

## 1. Objetivo

Definir o comportamento do Sistema Operacional de Comunicação Assistida: conceitos, responsabilidades, relações, estados e invariantes que transformam Intenção humana em comunicação organizada. Este documento não escolhe tecnologia nem um primeiro canal.

## 2. Princípio central do domínio

O ciclo funcional começa na **Intenção**, materializa-se em **Conteúdo** e organiza sua distribuição pela **Presença Digital**.

- Intenção registra o propósito humano original.
- Tutor compreende, conduz, explica e aprende contexto autorizado.
- Conteúdo preserva a fonte editorial organizada.
- Versões para Destino adaptam a fonte sem substituí-la.
- Presença Digital reúne os ambientes conectados do Projeto.
- Destino de Publicação representa um ponto externo concreto.
- Publicação registra uma ação confirmada e seu resultado por Destino.

O Conteúdo continua sendo a entidade editorial central aprovada na arquitetura técnica; a Intenção passa a ser sua origem funcional. Nenhuma dessas entidades pertence a uma rede social específica.

## 3. Visão funcional

```text
Usuário
  │ expressa
  ▼
Intenção ───── contexto autorizado ───── Conhecimento do Usuário
  │                                         ▲
  └──────────────► Tutor ◄──────────────────┘
                     │ conduz e explica
                     ▼
                  Conteúdo ── Biblioteca
                     │
                     ├── Versão fonte
                     └── Versão para Destino
                                  │
Projeto ── Presença Digital ── Destino de Publicação
                                  │
                                  ▼
                              Publicação
                                  │
                            Resultado / Tutor
```

## 4. Atores

| Ator | Responsabilidade |
|---|---|
| Usuário | Expressar intenção, corrigir contexto, escolher, revisar e dar a confirmação final |
| Tutor | Compreender, perguntar o mínimo, orientar, explicar, ensinar e propor próximos passos |
| Destino externo | Autorizar conexão, receber versão confirmada e informar estados ou resultados disponíveis |
| Operação da plataforma | Manter regras, integrações, segurança, privacidade, observabilidade e suporte |

No MVP existe um único papel: proprietário do Projeto.

## 5. Entidades e conceitos do domínio

### 5.1 Usuário

Representa a pessoa que controla acesso, dados, preferências, decisões e ações. Pode corrigir o conhecimento usado pelo Tutor, retirar consentimentos e recusar qualquer recomendação.

### 5.2 Projeto

Delimita o contexto de comunicação de uma igreja, negócio, marca ou iniciativa. Possui objetivo, público, identidade editorial, Biblioteca, Presença Digital e isolamento de dados.

### 5.3 Intenção

Representa o propósito humano que origina um Conteúdo.

**Informações funcionais mínimas:** enunciado original, objetivo, público, mensagem principal, contexto temporal, origem, autor, grau de completude e datas.

**Regras:**

- pode nascer incompleta e em linguagem livre;
- é confirmada pelo Usuário antes de orientar uma proposta material;
- não é sobrescrita por adaptação de formato;
- todo Conteúdo aponta para a Intenção que o originou;
- uma nova finalidade material exige nova Intenção ou revisão explicitamente registrada.

### 5.4 Conhecimento do Usuário

Conjunto de fatos, preferências e decisões autorizados para reutilização: público, objetivo, identidade editorial, termos preferidos, restrições e feedback. Cada item precisa de origem, finalidade, escopo, atualidade e possibilidade de correção ou exclusão.

Não inclui segredos, texto livre integral ou inferência sensível por padrão. É contexto do Tutor, não permissão para ação.

### 5.5 Tutor

É o núcleo funcional e a fachada da jornada. Abre com “O que você quer fazer hoje?”, identifica a Intenção, consulta o contexto permitido, pergunta somente quando necessário, sugere boas práticas, explica o motivo e conduz até uma decisão compreensível.

O Tutor ensina no momento da tarefa, sem criar um curso separado. Nunca publica, conecta, consente, desconecta, exclui ou altera permissões em nome do Usuário.

### 5.6 Conteúdo

Representa a fonte editorial organizada: mensagem, objetivo, público, estrutura, histórico e estado. Pertence a um Projeto, nasce de uma Intenção e pode existir sem Presença Digital ou Destino conectado.

Alterações preservam versões rastreáveis. Uma versão usada em Publicação é imutável.

### 5.7 Biblioteca de Conteúdos

É a projeção organizacional dos Conteúdos do Projeto. Permite localizar, retomar, revisar e arquivar sem duplicar a fonte editorial.

### 5.8 Presença Digital

Representa todos os ambientes digitais conectados a um Projeto. É a raiz funcional para organização de Destinos, identidade pública, saúde de conexões e disponibilidade de capacidades.

Um Projeto possui uma Presença Digital lógica, ainda que ela esteja vazia. Desconectar um ambiente preserva Conteúdos e histórico de Publicações.

### 5.9 Destino de Publicação

Representa o ponto concreto que pode receber comunicação: Página, perfil profissional, Canal, número empresarial, canal de vídeo ou equivalente futuro.

**Informações funcionais:** provedor, tipo, identidade pública, capacidades, formatos, limites, autorização, saúde e disponibilidade.

Instagram, Facebook, YouTube, WhatsApp, Telegram e futuros provedores são implementações possíveis. O domínio consulta capacidades e não presume equivalência entre eles.

### 5.10 Conexão Externa

Representa o vínculo autorizado entre a Presença Digital e um ambiente externo. Guarda identidade, permissões, estado e referência protegida de credencial. É suporte técnico do Destino e não o modelo mental central do Usuário.

### 5.11 Versão para Destino

É uma versão derivada de um Conteúdo fonte e associada a um Destino. Registra regras aplicadas, diferenças materiais, motivo, estado de revisão e autoria/aceitação.

**Invariantes:**

- preserva vínculo com a fonte e com a Intenção;
- não altera silenciosamente a mensagem principal;
- toda mudança material é visível e explicável;
- pode ser rejeitada ou editada;
- fica congelada antes da Publicação.

### 5.12 Publicação

Registra a distribuição confirmada de uma Versão para um Destino. Possui solicitante, confirmação, estado, tentativas, referência externa e resultado. Cada Destino gera uma Publicação independente.

### 5.13 Integração

Encapsula autorização, capacidades, envio, consulta e erros de um provedor externo. Traduz detalhes técnicos para o domínio e nunca decide o que deve ser comunicado.

### 5.14 Resultado

Apresenta fatos de uma Publicação com fonte, período, atualização e limitações. Resultados alimentam explicações e aprendizado, mas não provam causalidade automaticamente.

### 5.15 Agendamento e Campanha

Continuam conceitos evolutivos e fora do primeiro fluxo. Agendamento define execução futura; Campanha agrupa Conteúdos. Nenhum é necessário para criar Intenção, Conteúdo ou Presença Digital.

## 6. Relacionamentos

| Origem | Relação | Destino | Regra |
|---|---|---|---|
| Usuário | expressa | Intenção | Autoria e confirmação são rastreáveis |
| Projeto | possui | Presença Digital | Uma presença lógica por Projeto |
| Projeto | reúne | Conhecimento do Usuário | Isolado e controlado no contexto |
| Intenção | origina | Conteúdo | Todo Conteúdo possui origem humana identificável |
| Conteúdo | possui | Versão fonte | Histórico rastreável |
| Conteúdo | deriva | Versão para Destino | Zero ou várias, sem alterar a fonte |
| Presença Digital | reúne | Destino de Publicação | Zero ou vários Destinos conectados |
| Destino | usa | Conexão Externa e Integração | Detalhes permanecem atrás do contrato |
| Versão para Destino | origina | Publicação | Somente após confirmação |
| Publicação | produz | Resultado | Um ciclo independente por Destino |
| Tutor | consulta | contexto autorizado | Não adquire permissão de escrita sensível |

## 7. Fluxo funcional principal

1. Usuário abre o Tutor e expressa uma Intenção.
2. Tutor recupera contexto autorizado e avalia o que falta.
3. Tutor faz somente uma pergunta necessária por vez.
4. Usuário confirma o entendimento resumido.
5. Sistema cria a Intenção e o Conteúdo fonte recuperável.
6. Tutor sugere estrutura ou boa prática e explica o motivo.
7. Usuário aceita, adapta ou rejeita a sugestão.
8. Quando houver Destino selecionado, sistema consulta suas capacidades.
9. Sistema cria uma Versão para Destino por regras determinísticas no MVP.
10. Usuário vê fonte, adaptação, motivo, Destino e consequência.
11. Confirmação explícita congela a versão e cria a Publicação.
12. Integração executa e reconcilia o resultado.
13. Tutor explica o fato, a limitação e o próximo passo.

Sem Destino aprovado, os passos 8 a 13 operam somente com simulador e não produzem ação externa.

## 8. Contrato do Tutor

### Deve

- compreender linguagem comum e contexto da etapa;
- reutilizar conhecimento válido antes de perguntar;
- indicar quando uma suposição influencia a proposta;
- explicar recomendações em linguagem proporcional à experiência do Usuário;
- oferecer controle progressivo a Usuários avançados;
- registrar aceitação, adaptação ou rejeição para continuidade;
- encaminhar a um formulário ou caso de uso autorizado.

### Não deve

- expor API, algoritmo, formato técnico, jargão de marketing ou mecanismo de IA como requisito de uso;
- repetir pergunta cuja resposta válida já existe;
- inventar contexto ou resultado;
- converter recomendação em ação externa;
- ocultar mudança material, limitação ou risco;
- tratar conhecimento absorvido como consentimento irrestrito.

## 9. Contrato de adaptação e distribuição

Cada tipo de Destino declara capacidades, formatos, limites, permissões, estados e resultados. O mecanismo de adaptação recebe Conteúdo fonte e capacidades e produz proposta rastreável.

No MVP, regras determinísticas podem ajustar estrutura, comprimento e campos, mas não criar uma nova mensagem principal. Se uma limitação exigir mudança de sentido, o Tutor explica e pede decisão. IA generativa permanece desabilitada.

Publicar exige confirmação ligada ao Usuário, Intenção, Conteúdo, Versão, Destino e instante. Timeout não autoriza repetição; resultado incerto exige reconciliação.

## 10. Contextos funcionais

| Contexto | Responsabilidade |
|---|---|
| Identidade e Projeto | Acesso, consentimento, preferências e isolamento |
| Tutoria e Conhecimento | Intenção, contexto, perguntas, explicações e aprendizado |
| Conteúdo e Biblioteca | Fonte editorial, versões, retomada e organização |
| Presença Digital | Ambientes, Destinos, identidades e saúde das conexões |
| Adaptação | Capacidades, regras, versões e justificativas |
| Distribuição | Confirmação, envio, tentativa e reconciliação |
| Resultados | Fatos, limitações e próximo passo |

## 11. Estados principais

```text
Intenção: capturada → precisa de contexto → compreendida → convertida

Conteúdo: rascunho → em revisão → pronto → arquivado

Versão para Destino: proposta → ajustada → aprovada → congelada

Destino: indisponível → conectando → disponível → requer atenção → desconectado

Publicação: aguardando confirmação → na fila → processando
                                      ├→ publicada
                                      ├→ falhou
                                      └→ resultado incerto → reconciliada
```

Salvar Intenção ou Conteúdo nunca cria Publicação.

## 12. Telas e fontes de verdade

| Tela | Fonte de verdade | Ação principal |
|---|---|---|
| Acesso | Usuário | Entrar e gerir consentimentos |
| Início / Tutor | Intenção, contexto e jornada | Expressar objetivo e seguir próximo passo |
| Criar | Conteúdo e versões | Revisar, editar e salvar |
| Biblioteca | Conteúdo | Localizar, retomar e arquivar |
| Presença Digital | Presença e Destinos | Compreender e gerir ambientes conectados |
| Revisão | Fonte e Versão para Destino | Comparar, ajustar e confirmar |
| Andamento | Publicação | Acompanhar ou recuperar |
| Resultados | Resultado | Compreender fato e próximo passo |

## 13. Regras transversais

- A complexidade tecnológica pertence ao sistema, nunca ao Usuário.
- O sistema reduz escolhas técnicas, mas não remove escolhas editoriais ou de consequência.
- Recomendações são explicáveis, opcionais e registram resposta.
- A Intenção original permanece rastreável.
- Adaptação é automática quando segura e revisável quando material.
- O Usuário mantém controle final de qualquer ação externa.
- Conhecimento, Conteúdo, Presença e resultados são isolados por Projeto.
- Credenciais nunca são exibidas ao Tutor, interface ou telemetria.
- Falha preserva o trabalho e oferece recuperação compreensível.
- A mesma experiência oferece simplicidade inicial e controles progressivos.

## 14. Recorte funcional do MVP

O MVP valida um Usuário proprietário, um Projeto, uma Intenção curta, um Conteúdo textual, conhecimento contextual mínimo, Biblioteca simples, Presença Digital mínima, uma Versão para Destino produzida por regras e um Destino simulado.

Nenhum provedor específico é escolhido nesta Sprint. Integração real e Publicação externa permanecem bloqueadas até decisão e prova técnica posteriores. Mídia, armazenamento conectado, IA generativa, Agendamento, Campanha, colaboração, lote e métricas avançadas ficam fora.

## 15. Critérios de validação

- Usuário explica a proposta sem descrevê-la como mero publicador.
- Expressa a Intenção e reconhece o entendimento do Tutor.
- Tutor não pergunta novamente o que já sabe e não omite lacuna material.
- Usuário reconhece a Intenção na fonte e na versão adaptada.
- Compreende o motivo da recomendação e aprende algo aplicável.
- Identifica Presença Digital e Destino sem precisar entender a integração.
- Distingue salvar, adaptar, confirmar e publicar.
- Mantém controle final e recupera interrupções sem perda.

## 16. Impactos ainda não materializados

O modelo lógico de dados ainda usa Canal e Conta Conectada e não contém Intenção, Presença Digital nem conhecimento estruturado como conceitos próprios. Sua atualização física deve ocorrer em Sprint autorizada antes da implementação dessas estruturas. A arquitetura técnica continua válida por seus contratos de adaptadores e módulos, mas deverá refletir os novos nomes funcionais sem alterar ENG-01 a ENG-15.
