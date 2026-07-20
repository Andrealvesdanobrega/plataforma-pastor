# Blueprint do Produto

**Versão:** 0.1

**Fase:** Product Discovery

**Atualizado em:** 19 de julho de 2026

## 1. Propósito deste documento

Traduzir a missão e a visão em um modelo de produto testável. Este blueprint orienta descoberta, experiência, arquitetura funcional, definição do MVP e backlog. Hipóteses são explicitadas para não serem confundidas com decisões validadas.

## 2. Problema

Pessoas que precisam comunicar ideias, mensagens, serviços ou conhecimento online encontram uma jornada fragmentada. Elas precisam criar contas, aprender regras de canais, produzir formatos diferentes, guardar arquivos, publicar corretamente e interpretar métricas. Para usuários iniciantes, isso aumenta esforço, insegurança, erros e dependência de terceiros.

## 3. Proposta de valor

Uma plataforma única que conduz o usuário como um tutor e transforma a publicação digital em um processo compreensível: configurar, criar, organizar, publicar e acompanhar.

### Promessa central

“Você traz a mensagem; a plataforma ajuda a preparar, publicar e aprender com ela.”

### Diferenciais pretendidos

- jornada completa no mesmo ambiente;
- orientação contextual, e não apenas funções isoladas;
- conteúdo como fonte central para organização e distribuição;
- linguagem pensada para iniciantes;
- resultados traduzidos em próximos passos;
- aprovação humana e transparência em ações importantes.

## 4. Público inicial

### Segmentos candidatos

| Segmento | Necessidade provável | Contexto a investigar |
|---|---|---|
| Pastores e líderes de comunidades | Compartilhar mensagens e manter comunicação recorrente | Rotina, voluntários, formatos longos e curtos, sensibilidade do conteúdo |
| Pequenos empreendedores | Divulgar produtos, serviços e relacionamento | Tempo disponível, conversão esperada, sazonalidade e canais usados |
| Influenciadores iniciantes | Construir consistência e entender desempenho | Frequência, identidade, experimentação e ansiedade por métricas |
| Criadores iniciantes | Transformar ideias em publicações organizadas | Processo criativo, formatos, bloqueios e reaproveitamento |

### Características comuns presumidas

- baixa ou média confiança no uso de ferramentas digitais;
- pouco tempo ou apoio especializado;
- dificuldade em manter consistência;
- uso de mais de uma ferramenta ou canal;
- receio de cometer erros públicos;
- dificuldade para interpretar resultados.

Essas características devem ser verificadas. A escolha de um segmento prioritário é condição para fechar o MVP.

## 5. Jobs to be done

### Trabalho principal

Quando tenho uma mensagem ou objetivo para comunicar, quero receber orientação para transformá-lo em conteúdo adequado, publicá-lo com segurança e entender o resultado, para manter minha presença digital sem depender de conhecimento técnico especializado.

### Trabalhos funcionais

- preparar contas e permissões necessárias;
- estruturar uma ideia em conteúdo;
- adaptar forma e mídia ao destino;
- localizar e saber o estado de cada conteúdo;
- revisar e controlar a publicação;
- verificar o que aconteceu e decidir o próximo passo.

### Trabalhos emocionais e sociais

- sentir segurança antes de tornar algo público;
- evitar parecer despreparado ou pouco profissional;
- manter autenticidade e propriedade sobre a mensagem;
- sentir progresso e autonomia;
- comunicar-se com consistência com sua comunidade ou clientes.

## 6. Princípios de experiência

1. **Uma ação principal por contexto:** o próximo passo deve ser evidente.
2. **Explicar antes de pedir:** permissões, dados e consequências aparecem antes da confirmação.
3. **Progresso persistente:** o usuário pode interromper e retomar sem perda silenciosa.
4. **Revisão antes da ação externa:** publicar, desconectar ou remover exige confirmação proporcional ao impacto.
5. **Erros recuperáveis:** toda falha preserva o trabalho e informa como avançar.
6. **Sugestão não é decisão:** conteúdo sugerido só é adotado mediante ação do usuário.
7. **Complexidade progressiva:** opções avançadas aparecem quando necessárias.
8. **Linguagem concreta:** termos técnicos dos canais são traduzidos ou explicados.

## 7. Objeto central: conteúdo

Um **Conteúdo** representa a intenção editorial principal do usuário. Ele reúne:

- título ou identificação interna;
- objetivo e público;
- mensagem ou corpo principal;
- tipo e mídias associadas;
- autoria e datas;
- versões editáveis;
- versões específicas por canal;
- destinos de publicação;
- estado editorial;
- publicações e tentativas;
- métricas e aprendizados associados.

### Estados editoriais iniciais

| Estado | Significado | Próxima ação típica |
|---|---|---|
| Ideia | Registro inicial ainda não estruturado | Começar criação |
| Rascunho | Conteúdo em elaboração | Continuar editando |
| Precisa de revisão | Há campos, regras ou decisões pendentes | Corrigir pendências |
| Pronto | Conteúdo validado para o destino escolhido | Revisar publicação |
| Em publicação | Envio solicitado e ainda não concluído | Aguardar ou consultar estado |
| Publicado | Ao menos uma publicação foi confirmada | Acompanhar resultados |
| Com falha | Uma tentativa não foi concluída | Corrigir ou tentar novamente |
| Arquivado | Removido do fluxo ativo sem ser apagado | Restaurar se necessário |

O estado editorial resume a situação do conteúdo; cada destino possui também seu próprio estado de publicação. Essa separação evita tratar uma falha em um canal como falha de todos os destinos.

## 8. Jornada de ponta a ponta

### Etapa 1 — Configurar

**Objetivo do usuário:** ficar pronto para usar a plataforma e publicar.

**Tutor:** explica o processo, coleta objetivo e público, orienta a conexão e confirma pendências.

**Saída:** perfil mínimo e ao menos um canal apto.

### Etapa 2 — Criar

**Objetivo do usuário:** transformar uma ideia em conteúdo.

**Tutor:** pergunta intenção, sugere estrutura e aponta requisitos do destino.

**Saída:** rascunho salvo e editável.

### Etapa 3 — Organizar

**Objetivo do usuário:** localizar, preparar e planejar conteúdos.

**Tutor:** apresenta estado, pendências, versões e ação seguinte.

**Saída:** conteúdo pronto para publicação ou guardado para retomada.

### Etapa 4 — Publicar

**Objetivo do usuário:** enviar o conteúdo correto ao destino correto.

**Tutor:** valida requisitos, mostra a prévia, identifica conta e canal e solicita confirmação.

**Saída:** publicação confirmada ou falha recuperável, sem perda do conteúdo.

### Etapa 5 — Acompanhar

**Objetivo do usuário:** entender o resultado e melhorar.

**Tutor:** informa período e limitações, traduz métricas e sugere uma ação prática.

**Saída:** aprendizado registrado e próximo passo compreendido.

## 9. Capacidades do produto

| Domínio | Responsabilidade |
|---|---|
| Identidade e perfil | Acesso, preferências, objetivo, público e consentimentos |
| Tutoria | Progresso, orientação contextual, pendências e próximos passos |
| Canais | Conexão, permissões, capacidades e saúde das contas externas |
| Conteúdo | Criação, edição, estados, versões, requisitos e biblioteca |
| Mídia | Upload, validação, associação e preparação de arquivos |
| Organização | Busca, filtros, calendário e retomada |
| Publicação | Prévia, confirmação, envio, idempotência, estado e recuperação |
| Resultados | Coleta, atualização, explicação e recomendações |
| Notificações | Avisos úteis sobre pendências, falhas e resultados, com preferências |
| Governança | Auditoria, privacidade, segurança, moderação e políticas |

## 10. Escopo proposto do MVP

### Incluído

- um segmento prioritário;
- um canal conectado;
- um tipo principal de conteúdo;
- onboarding e conexão guiados;
- criação assistida e edição de rascunho;
- biblioteca simples com estado;
- validação, prévia e publicação imediata;
- confirmação de sucesso ou falha recuperável;
- métricas essenciais disponíveis no canal;
- uma explicação ou próximo passo simples;
- instrumentação do ciclo principal.

### Fora do MVP

- publicação simultânea em vários canais;
- calendário editorial e agendamento avançados;
- colaboração em equipe e aprovações multinível;
- edição profissional de mídia;
- automações autônomas de publicação;
- análise preditiva ou garantia de desempenho;
- mídia paga, comércio eletrônico ou CRM completo.

## 11. Regras de negócio iniciais

- Um conteúdo pode existir sem canal conectado e nunca é publicado por ser apenas criado ou salvo.
- Toda publicação se refere a uma versão imutável do conteúdo naquele momento.
- O usuário deve identificar a conta e o canal de destino antes de confirmar.
- Uma nova tentativa não pode criar duplicidade sem que o sistema verifique o estado anterior ou peça decisão.
- Desconectar um canal impede novos envios, mas não apaga conteúdos ou histórico local.
- Métricas devem informar fonte, período e última atualização.
- Recomendações não podem afirmar causalidade sem evidência nem prometer resultados.
- Credenciais externas não podem ser expostas ao usuário, aos logs ou a componentes sem necessidade.

## 12. Métricas candidatas

### Valor e ativação

- taxa de conclusão do onboarding;
- taxa de conexão bem-sucedida do primeiro canal;
- tempo até o primeiro rascunho;
- taxa e tempo até a primeira publicação confirmada.

### Qualidade da experiência

- conclusão sem ajuda externa;
- abandono por etapa;
- erros e recuperação por etapa;
- compreensão do estado e do próximo passo;
- percepção de confiança antes da publicação.

### Retenção e valor contínuo

- retorno para criar um segundo conteúdo;
- conteúdos publicados por usuário ativo;
- retorno para consultar resultados;
- adoção de recomendações;
- redução de tempo ou ferramentas percebida pelo usuário.

As definições exatas e metas dependem da linha de base do piloto. Métricas de vaidade, como volume bruto de conteúdo, não devem ser usadas isoladamente como sucesso.

## 13. Hipóteses e experimentos

| Hipótese | Experimento inicial | Sinal favorável |
|---|---|---|
| A fragmentação é uma dor relevante | Entrevistas baseadas em comportamento passado | Padrão recorrente de troca de ferramentas, erro ou abandono |
| A tutoria aumenta autonomia | Teste de tarefa com protótipo | Conclusão com pouca ajuda e próximo passo compreendido |
| Conteúdo é um modelo mental claro | Card sorting e teste da biblioteca | Usuários localizam itens e explicam estados corretamente |
| Usuários confiam na conexão guiada | Teste da tela de permissões | Benefício e permissões são compreendidos antes de aceitar |
| Acompanhamento gera recorrência | Protótipo de resultado e entrevista | Usuários identificam decisão prática e intenção de retorno |
| Há base comum entre segmentos | Síntese comparativa das entrevistas | Necessidades centrais semelhantes e diferenças administráveis |

## 14. Requisitos não funcionais de produto

- fluxo principal acessível por teclado e compatível com tecnologias assistivas;
- linguagem em português simples e mensagens que indiquem correção;
- proteção de dados pessoais e credenciais por padrão;
- rastreabilidade de publicação sem armazenar segredos em logs;
- resiliência a indisponibilidade e limites de canais externos;
- salvamento confiável de rascunhos;
- desempenho adequado a conexões comuns do público-alvo;
- observabilidade do funil e de falhas críticas;
- possibilidade de revogar acesso e solicitar exclusão conforme política aplicável.

## 15. Dependências e restrições

- APIs, políticas, revisão e limites dos canais;
- formatos e métricas diferentes entre destinos;
- consentimento e tratamento de dados pessoais;
- direitos sobre textos e mídias enviados ou sugeridos;
- custo e qualidade de serviços de assistência automatizada;
- disponibilidade de participantes representativos para pesquisa.

## 16. Critérios para revisar este blueprint

Este documento deve ser atualizado quando uma hipótese central for validada ou rejeitada, quando segmento ou canal prioritário mudar, quando o MVP for aprovado ou quando uma limitação técnica alterar a jornada. Toda mudança que afete capacidades deve provocar revisão da arquitetura funcional, do backlog e do roadmap.
