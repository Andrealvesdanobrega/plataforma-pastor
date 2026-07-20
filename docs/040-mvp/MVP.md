# Produto Mínimo Viável

**Versão:** 0.1

**Fase:** Sprint 2 — Domain Model

**Atualizado em:** 19 de julho de 2026

## 1. Objetivo do MVP

Validar se uma pessoa com pouca familiaridade tecnológica consegue completar, em uma única experiência guiada, o ciclo entre configurar a plataforma, conectar uma conta, criar e organizar um Conteúdo, publicá-lo e compreender os primeiros resultados.

O MVP não busca validar todas as automações ou todos os Canais. Ele valida a proposta central do produto com um ciclo pequeno, real e mensurável.

## 2. Hipótese principal

Se a plataforma apresentar uma única porta de entrada e um Tutor que explique cada etapa, então usuários iniciantes conseguirão publicar Conteúdo com maior autonomia e confiança e saberão qual próximo passo realizar depois de consultar os resultados.

## 3. Princípio do escopo

O **Conteúdo é a entidade central do MVP**. A configuração prepara o contexto do Conteúdo; a Biblioteca o organiza; o Canal e a Conta Conectada permitem sua distribuição; a Publicação registra o envio; o Relatório apresenta seu resultado; e o Tutor transforma esse resultado em orientação.

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
| Conta Conectada | Conta real autorizada | Uma ou mais contas do mesmo Canal, conforme viabilidade |
| Integração | Autorização, publicação e métricas | Um conector operacional |
| Publicação | Envio e resultado por destino | Publicação imediata e tentativas rastreáveis |
| Relatório | Resultados essenciais | Uma visão por Conteúdo ou Publicação |
| Tutor | Orientação do ciclo | Regras, contexto e recomendações simples |
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

| Dependência ou risco | Tratamento antes do desenvolvimento |
|---|---|
| Plataforma de App não escolhida | Validar hábitos, dispositivos e forma de distribuição do segmento |
| Canal e formato não definidos | Selecionar a partir de pesquisa comportamental |
| Revisão e limites da API | Fazer prova de autorização, publicação, estado e métricas |
| Confiança para conectar contas | Testar explicação de permissões e revogação |
| Escopo do Tutor crescer demais | Limitar a etapas, regras e recomendações simples no MVP |
| Métricas demorarem ou variarem | Definir expectativa, atualização e estado parcial do Relatório |
| Publicação duplicada | Implementar idempotência e reconciliação antes do piloto real |
| Dificuldade com instalação | Testar distribuição e primeiro acesso com dispositivos reais |

## 14. Critérios de entrada em desenvolvimento

- segmento e caso de uso prioritários confirmados;
- plataforma do App, Canal e formato selecionados;
- protótipo do fluxo completo testado com usuários;
- Integração considerada viável por prova técnica;
- permissões, estados e erros do Canal mapeados;
- métricas do piloto e eventos de coleta definidos;
- políticas mínimas de privacidade, retenção e suporte aprovadas;
- escopo obrigatório separado das evoluções opcionais.

## 15. Critérios de saída do MVP

O MVP será considerado validado quando o grupo piloto completar publicações reais com segurança, a maioria dos participantes compreender o fluxo e o próximo passo sem suporte especializado, as falhas críticas forem recuperáveis e houver evidência de retorno para acompanhar resultados ou criar novo Conteúdo.

Os limiares quantitativos devem ser aprovados antes do piloto. Caso os sinais não sejam atingidos, a decisão correta pode ser revisar segmento, linguagem, jornada, Canal ou proposta de valor em vez de ampliar funcionalidades.
