# Produto Mínimo Viável

**Versão:** 0.6

**Fase:** Sprint 7 — Modelo de Comunicação Assistida

**Atualizado em:** 20 de julho de 2026

## 1. Objetivo do MVP

Validar se uma pessoa responsável pela própria comunicação consegue transformar uma Intenção em Conteúdo organizado e em uma versão adequada a um Destino, compreendendo recomendações e mantendo o controle final, sem precisar dominar tecnologia ou marketing digital.

O MVP valida o **Sistema Operacional de Comunicação Assistida**, não um publicador de redes sociais. Nenhum primeiro canal é escolhido nesta Sprint.

## 2. Hipótese principal

Se o Tutor compreender a Intenção, reutilizar contexto válido, perguntar somente o necessário, explicar suas recomendações e conduzir até uma prévia adaptada, então o Usuário conseguirá organizar uma comunicação útil com menos esforço, reconhecerá sua intenção no resultado e saberá decidir o próximo passo.

## 3. Primeiro Usuário e dor

O primeiro recorte de validação continua sendo um pastor ou líder de pequena igreja que cuida pessoalmente da comunicação e tem baixa ou média confiança digital. É hipótese de teste, não segmento definitivamente aprovado.

### Dor

Ele sabe o que precisa comunicar, mas enfrenta decisões de estrutura, clareza, formato, ferramenta e distribuição que não domina. Isso gera insegurança, retrabalho, comunicação irregular e dependência de alguém mais técnico.

### Primeiro objetivo

Transformar um aviso que tem em mente em uma comunicação clara, organizada e pronta para um Destino, entendendo as sugestões e mantendo a palavra final.

## 4. Princípio do escopo

Todo Conteúdo nasce como Intenção. O Tutor é o núcleo da experiência; a Presença Digital organiza ambientes conectados; e os canais são apenas provedores de Destinos de Publicação.

**A complexidade tecnológica pertence ao sistema, nunca ao Usuário.** O sistema esconde a mecânica, mas mostra consequências, adaptações materiais, Destino e confirmação.

### 4.1 Baseline técnica preservada

ENG-01 a ENG-15 continuam vigentes sem alteração: Web responsiva; TypeScript e Node.js 24 LTS; Next.js; NestJS; PostgreSQL com Prisma; Amazon Cognito; outbox e fila no PostgreSQL; AWS em `sa-east-1`; Tutor determinístico; sem IA generativa e sem aplicativo Mobile na primeira versão utilizável.

## 5. Menor fluxo assistido

```text
Entrar por convite
→ Tutor pergunta “O que você quer fazer hoje?”
→ Usuário expressa a Intenção de comunicar um aviso
→ Tutor recupera o que já sabe e pergunta somente o que falta
→ Usuário confirma o entendimento
→ Sistema cria Conteúdo textual recuperável
→ Tutor sugere uma melhoria e explica o motivo
→ Usuário aceita, ajusta ou rejeita
→ Sistema cria uma versão para um Destino simulado
→ Usuário vê a adaptação e seu motivo
→ Usuário confirma que a versão representa sua Intenção
→ Sistema registra o ciclo e apresenta o próximo passo
```

Esse fluxo não produz ação externa. Depois de escolhido e comprovado um primeiro Destino real, a mesma jornada continuará com confirmação de destino, Publicação assíncrona e resultado, conforme ENG-10 e ENG-15.

## 6. Etapas e entregas mínimas

### 6.1 Entrada e contexto mínimo

- acesso Web por convite e código de uso único;
- nome e Projeto principal;
- explicação de que nada será enviado sem confirmação;
- consentimentos essenciais e continuidade entre sessões.

### 6.2 Captura da Intenção

- abertura obrigatória do Tutor: “O que você quer fazer hoje?”;
- texto curto em linguagem comum;
- objetivo, público e momento recuperados do contexto quando válidos;
- uma pergunta por vez somente para lacuna material;
- resumo do entendimento para correção.

### 6.3 Conteúdo fonte

- título interno e aviso textual;
- vínculo rastreável com a Intenção;
- salvamento, edição e retomada;
- uma recomendação editorial determinística;
- explicação breve e contextual da recomendação;
- aceitar, adaptar ou rejeitar sem perder a fonte.

### 6.4 Versão para Destino simulado

- um perfil genérico de capacidades e limites;
- adaptação determinística de estrutura ou comprimento;
- comparação clara entre fonte e proposta;
- indicação do que mudou e por quê;
- confirmação de que a proposta preserva a Intenção;
- nenhuma chamada externa.

### 6.5 Encerramento e aprendizado

- Conteúdo e versão disponíveis na Biblioteca;
- decisão e feedback do Usuário registrados de forma minimizada;
- próximo passo explicado;
- possibilidade de corrigir conhecimento desatualizado.

## 7. Telas do fluxo

| Tela | Responsabilidade mínima |
|---|---|
| Acesso | Convite, código, sessão e consentimentos |
| Início / Tutor | Capturar Intenção e mostrar próximo passo |
| Contexto assistido | Confirmar somente informações necessárias |
| Criar Conteúdo | Editar fonte, ver sugestão e decidir |
| Biblioteca | Salvar, localizar e retomar |
| Presença Digital | Explicar o conceito e mostrar Destino simulado |
| Revisão da versão | Comparar fonte, adaptação, motivo e Destino |
| Conclusão | Confirmar preservação da Intenção e mostrar próximo passo |

A navegação pode combinar etapas na mesma tela desde que as responsabilidades e estados permaneçam claros.

## 8. Entidades participantes

| Entidade ou conceito | Uso no MVP |
|---|---|
| Usuário | Identidade, preferências e controle |
| Projeto | Contexto e isolamento |
| Intenção | Origem humana confirmada |
| Conhecimento do Usuário | Contexto mínimo, autorizado e corrigível |
| Tutor | Condução, explicação e aprendizado determinísticos |
| Conteúdo | Fonte editorial textual |
| Versão de Conteúdo | Fonte e proposta adaptada rastreáveis |
| Biblioteca | Retomada e organização simples |
| Presença Digital | Organização conceitual dos ambientes |
| Destino de Publicação | Perfil simulado de capacidades |
| Recomendação | Sugestão, motivo e resposta |

Publicação, Tentativa e Integração permanecem contratadas, mas não executam serviço externo até um Destino real ser aprovado.

## 9. Informações armazenadas

- identidade, sessão, consentimentos e marcos da jornada;
- Projeto, objetivo, público e identidade editorial mínima;
- Intenção original, campos confirmados, autor e datas;
- itens de conhecimento autorizados, origem, finalidade e atualização;
- Conteúdo, versões, estados e histórico;
- perfil simulado do Destino e regras aplicadas;
- recomendação, explicação e resposta do Usuário;
- eventos mínimos de experiência e auditoria sem corpo integral em telemetria.

Não são armazenados segredos de canal, texto livre integral do Tutor por padrão, inferências sensíveis ou dados sem finalidade definida.

## 10. Integrações necessárias

Para o fluxo assistido inicial:

- Amazon Cognito para identidade, conforme decisão aprovada;
- e-mail transacional associado ao código de entrada;
- adaptadores simulados de Destino e Publicação.

Nenhuma integração com Instagram, Facebook, YouTube, WhatsApp, Telegram ou futuro provedor é necessária nem aprovada nesta Sprint.

## 11. Capacidades obrigatórias

### Tutor e aprendizagem

- compreender uma Intenção prevista ou voltar a opções guiadas;
- consultar contexto válido e evitar perguntas repetidas;
- explicar o motivo da recomendação;
- ensinar uma boa prática dentro da tarefa;
- adaptar profundidade sem criar caminhos incompatíveis;
- nunca executar ação sensível.

### Conteúdo e organização

- preservar o enunciado original;
- criar, editar, salvar, retomar e arquivar;
- manter versões rastreáveis;
- distinguir fonte de versão adaptada;
- registrar decisão do Usuário.

### Presença e adaptação

- explicar Presença Digital sem exigir conhecimento de redes;
- modelar Destino por capacidade, não por posição central na navegação;
- gerar proposta determinística;
- tornar mudanças visíveis e reversíveis;
- impedir confirmação quando a adaptação altera o sentido sem decisão.

## 12. Princípios obrigatórios de aceite

- reduzir decisões técnicas;
- explicar recomendações;
- preservar a Intenção original;
- adaptar automaticamente a comunicação quando a regra for segura;
- manter o Usuário no controle final;
- ensinar sem parecer um curso;
- ser simples para iniciantes e poderosa para Usuários avançados;
- nunca exigir conhecimento de API, algoritmo, formato, rede social, marketing digital ou IA.

## 13. Métricas do piloto de experiência

### Compreensão e autonomia

- conclusão do fluxo sem instrução externa;
- Intenção corretamente resumida pelo Usuário e pelo Tutor;
- perguntas feitas por ciclo e perguntas consideradas desnecessárias;
- dados já informados solicitados novamente;
- tempo e abandono por etapa.

### Preservação e controle

- Usuário reconhece a mensagem original na proposta;
- alterações materiais percebidas antes da confirmação;
- recomendações aceitas, adaptadas ou rejeitadas conscientemente;
- distinção correta entre salvar, adaptar, confirmar e publicar;
- ação externa, perda silenciosa ou duplicidade: meta zero.

### Valor e aprendizagem

- redução percebida de esforço e insegurança;
- compreensão do motivo da recomendação;
- capacidade de aplicar a boa prática em nova tarefa;
- intenção de voltar para outra comunicação;
- percepção de que a ferramenta organiza a Presença Digital, não apenas publica.

Metas quantitativas serão definidas após a primeira linha de base observada.

## 14. Fora do MVP

- seleção de um canal ou provedor específico;
- ação externa real antes da prova e aprovação do Destino;
- múltiplos Destinos e distribuição simultânea;
- imagem, áudio, vídeo, arquivo e armazenamento conectado;
- IA generativa, geração autônoma e ferramenta autônoma;
- Agendamento, Campanha, colaboração e múltiplos Projetos na interface;
- notificações externas, busca avançada, lote e calendário editorial;
- métricas de engajamento, comparação multicanal e recomendações de desempenho;
- anúncios, CRM, comércio eletrônico e curso formal.

## 15. Critérios da primeira versão utilizável

Há dois marcos distintos.

### 15.1 Incremento utilizável para validação assistida

- um representante do público entra e conclui o fluxo com dados de teste;
- o Tutor compreende ou recupera de forma guiada sem perder estado;
- pergunta somente quando necessário e não repete contexto válido;
- o Usuário corrige o entendimento e controla o conhecimento reutilizado;
- Intenção, Conteúdo fonte e versão adaptada permanecem rastreáveis;
- uma recomendação é explicada e compreendida;
- a versão simulada preserva a Intenção e não é confundida com Publicação;
- Biblioteca e retomada funcionam sem perda silenciosa;
- acessibilidade, autorização, privacidade, auditoria e logs seguros do recorte são verificados.

### 15.2 Primeira versão utilizável para piloto real

Além do marco anterior, exige cumulativamente:

- primeiro Destino aprovado por evidência de usuário e prova oficial;
- identidade, permissão, revogação, capacidades, limites, envio, estado e recuperação comprovados;
- prévia identifica versão e Destino e exige confirmação explícita;
- Publicação assíncrona, idempotente, auditável e reconciliável;
- resultado factual compreensível;
- retenção, exclusão, suporte, restauração, alertas e resposta a incidente aprovados;
- todos os critérios de ENG-15 atendidos;
- pelo menos um representante conclui o ciclo real sem ajuda durante a tarefa e sem perda, duplicidade ou ação não confirmada.

Não escolher um Destino agora não reduz ENG-15; apenas separa validação do núcleo assistido da liberação de uma Integração real.

## 16. Riscos e dependências

| Risco | Tratamento |
|---|---|
| Tutor parecer questionário ou curso | Perguntar uma coisa por vez, medir necessidade e ensinar no contexto |
| Conhecimento absorvido ficar incorreto ou invasivo | Origem, confirmação, correção, exclusão e minimização |
| Adaptação determinística não entregar qualidade | Teste com exemplos reais; manter edição e rejeição |
| Usuário não reconhecer a Intenção | Comparação fonte–versão e bloqueio de mudança material silenciosa |
| Presença Digital ser conceito abstrato demais | Protótipo com linguagem e exemplos, sem escolher provedor |
| Simulador mascarar restrições externas | Não tratar simulação como prova; exigir POC antes do adaptador real |
| Documentos de dados e segurança ainda usarem Canal como centro | Atualizar em Sprint autorizada antes da implementação física |
| Primeiro segmento não ter dor suficiente | Entrevistas comportamentais comparativas antes do piloto |
