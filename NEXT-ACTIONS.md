# Próximas Ações

**Fase de referência:** após Sprint 5 — Decisões de Engenharia do MVP

**Atualizado em:** 20 de julho de 2026

## Objetivo imediato

Iniciar a implementação incremental do núcleo sobre a baseline aprovada e, em paralelo, fechar por evidência as decisões de produto, Integração e governança que bloqueiam o piloto.

As escolhas técnicas não compensam a ausência de validação de segmento, Canal ou fluxo. Adaptadores simulados liberam o núcleo; nenhum simulador autoriza Publicação real ou piloto.

## Prioridade 0 — Bloqueios de produto

### 1. Selecionar o recorte do piloto

- Concluir a pesquisa com os segmentos candidatos.
- Escolher um segmento, um caso de uso e um tipo de Conteúdo.
- Identificar o Canal realmente usado pelo segmento.
- Confirmar quais resultados ajudam o Usuário a tomar uma decisão.
- Registrar o que permanece fora do MVP.

**Saída esperada:** recorte único do piloto, promessa e métricas de valor.

**Decisões fechadas:** segmento, formato, Canal e métricas essenciais.

### 2. Validar Web responsivo como cliente primário

- Levantar dispositivos, conectividade, hábito de instalação e uso de arquivos.
- Testar primeiro acesso Web, código por e-mail, retorno de autorização e seleção de mídia no dispositivo real.
- Identificar qualquer bloqueio que não possa ser resolvido no navegador.
- Reabrir a decisão de Mobile somente se a evidência demonstrar dependência de instalação ou recurso nativo.

**Saída esperada:** compatibilidade e requisitos mínimos do Web confirmados, ou evidência explícita para revisar ENG-01 e ENG-04.

## Prioridade 1 — Provas de viabilidade

### 3. Provar a Integração do primeiro Canal

- Criar ou obter ambiente e conta oficial de teste.
- Verificar autorização, permissões mínimas, renovação e revogação.
- Validar formato e limites do Conteúdo e da mídia.
- Validar Publicação, identificador externo, estado incerto e consulta posterior.
- Validar assinatura, repetição e ordem de webhooks quando existirem.
- Confirmar métricas, definições, atraso e limites de coleta.
- Mapear revisão de aplicativo, termos, custos e prazos externos.

**Saída esperada:** matriz de capacidades, erros normalizados, riscos e recomendação de seguir ou trocar o Canal.

**Critério de aprovação:** o ciclo pode ser executado com confirmação, idempotência e reconciliação.

### 4. Provar armazenamento conectado

- Confirmar qual serviço é usado pelo segmento.
- Testar autorização com o menor escopo disponível.
- Selecionar arquivo, obter metadados, ler temporariamente e revogar.
- Testar arquivo alterado, removido, grande, incompatível e malicioso.
- Medir tempo, memória, transferência e custo da Publicação sem cópia permanente.
- Comprovar a limpeza de qualquer cópia temporária.

**Saída esperada:** contrato do adaptador, limite de tamanho e política de arquivo temporário.

### 5. Provar identidade e recuperação aprovadas

- Testar método de entrada com participantes do segmento.
- Validar Amazon Cognito, código por e-mail, fluxo OIDC, sessão Web, recuperação, revogação e bloqueio.
- Medir entrega de código, abandono, acessibilidade e comportamento em troca de dispositivo ou navegador.
- Definir autenticação reforçada para administração.
- Fechar duração, renovação e encerramento da sessão.

**Saída esperada:** fluxo aprovado ou evidência registrada para revisar ENG-07 antes do piloto.

## Prioridade 2 — Segurança e dados

### 6. Realizar modelagem de ameaças específica

- Atualizar fronteiras com provedores escolhidos.
- Modelar tomada de conta, isolamento, token, webhook, arquivo, Tutor e Publicação.
- Classificar severidade e responsável por tratamento.
- Definir critérios que bloqueiam o piloto.

**Saída esperada:** ameaças atualizadas e plano de tratamento com responsáveis.

### 7. Aprovar governança de dados

- Inventariar dado, finalidade, origem, destinatário e subprocessador.
- Definir fundamento aplicável e textos de transparência com revisão qualificada.
- Aprovar retenção de perfil, Conteúdo, mídia, token, Publicação, métrica, Tutor, auditoria, logs e cópias.
- Definir exportação, correção, revogação, anonimização e exclusão.
- Definir residência e transferência de dados.
- Nomear responsáveis por privacidade e incidente.

**Saída esperada:** política operacional de dados pronta para implementação e piloto.

### 8. Validar o Tutor determinístico

- Construir e testar a jornada determinística como referência.
- Medir intenções não compreendidas, retorno às opções guiadas e utilidade das sugestões controladas.
- Impedir que regras tentem cobrir casos fora do ciclo aprovado.
- Manter o contrato de assistência desabilitado e sem envio de dados a provedor de IA.
- Registrar hipótese de experimento futuro somente se a rigidez impedir valor relevante.

**Saída esperada:** evidência de que o Tutor determinístico sustenta a primeira versão utilizável, ou problema mensurado para decisão posterior ao MVP.

## Prioridade 3 — Detalhamento para implementação

### 9. Aplicar a baseline da plataforma

- Organizar o monorepositório em Web, API, worker e contratos internos sem misturar regras de domínio.
- Preparar desenvolvimento com Node.js 24 LTS, PostgreSQL local e adaptadores simulados.
- Materializar a topologia AWS aprovada para homologação antes de usar Integrações reais.
- Definir esquema físico, migrações, outbox, locks, tentativas e índices com Prisma e SQL revisado.
- Detalhar auditoria no PostgreSQL e logs, métricas, rastreamento e alertas no CloudWatch.
- Estimar custo por Usuário, Publicação, transferência, armazenamento e uso de IA.

**Saída esperada:** baseline reproduzível e verificada, com custo inicial e limites observáveis; custo de IA é zero na primeira versão.

### 10. Detalhar contratos dos módulos

- Definir comandos, consultas e eventos de cada módulo.
- Definir estados, transições e códigos de erro.
- Formalizar contrato dos adaptadores de Canal, armazenamento, identidade e IA.
- Definir idempotência, outbox, tentativa e reconciliação.
- Definir escopo de autorização por caso de uso.
- Definir dados mínimos de auditoria e telemetria.

**Saída esperada:** contratos testáveis sem dependência da interface do provedor.

### 11. Planejar ambientes e operação

- Criar isolamento por conta entre homologação e produção e manter desenvolvimento local sem dados reais.
- Definir contas de teste, callbacks, chaves e dados permitidos por ambiente.
- Definir cópias, recuperação, rotação e limpeza temporária.
- Aprovar metas iniciais de disponibilidade, recuperação e capacidade.
- Criar alertas com responsável e ação inicial.
- Preparar procedimentos de desligar Canal, revogar token e manter o adaptador de IA desabilitado.

**Saída esperada:** plano operacional e de continuidade anterior à produção.

### 12. Converter o MVP em incrementos

Ordem aprovada:

1. Identidade, Projeto e autorização.
2. Tutor determinístico e Orquestrador da jornada, sem IA generativa.
3. Conteúdo, versões e Biblioteca.
4. Conta Conectada e adaptador do Canal.
5. Confirmação, Publicação, outbox, worker e reconciliação.
6. Métricas, Relatório e recomendação simples.
7. Armazenamento conectado e mídia do formato escolhido.
8. Endurecimento do Web responsivo nos dispositivos do piloto.

Cada incremento deve terminar com fluxo demonstrável, testes, auditoria, logs seguros e tratamento de falha.

**Saída esperada:** backlog técnico priorizado com critérios de pronto e dependências.

## Prioridade 4 — Validação antes do piloto

### 13. Testar a experiência completa

- Executar onboarding, configuração, criação, Biblioteca, Publicação, Resultados e Tutor.
- Usar participantes do público e dispositivos reais.
- Simular conta expirada, arquivo removido, perda de rede, timeout e métrica atrasada.
- Verificar compreensão de destino, salvamento, confirmação e limites do Tutor.
- Corrigir todo problema crítico definido no MVP.

**Saída esperada:** evidência de usabilidade e decisão de liberar ou revisar.

### 14. Executar verificação técnica e de segurança

- Testar autorização entre Projetos e objetos relacionados.
- Testar sessão Web e Mobile conforme o cliente aprovado.
- Testar webhook forjado e repetido.
- Testar Publicação duplicada, timeout e reconciliação.
- Testar arquivo malicioso, excessivo e temporário vencido.
- Testar injeção no Tutor e saída inválida de IA, se usada.
- Verificar dependências, segredos, configuração e telemetria.
- Restaurar uma cópia em ambiente isolado.
- Exercitar vazamento de token e suspensão do Canal.

**Saída esperada:** relatório de verificação, riscos residuais e aprovação formal do piloto.

## Decisões que bloqueiam o piloto, não o núcleo

- segmento e tipo de Conteúdo;
- Canal, formato, permissões e métricas;
- armazenamento conectado e limites de arquivo;
- política de dados, retenção, exclusão, fundamento e subprocessadores;
- metas de recuperação, disponibilidade, capacidade e custo;
- responsáveis por segurança, privacidade, suporte e operação;
- limiares quantitativos do piloto.

## Decisões que podem esperar até depois do MVP

- separação em microserviços;
- múltiplos Canais e lote multicanal;
- Agendamento e Campanha;
- colaboração e múltiplos papéis;
- motor de busca dedicado;
- armazém analítico;
- múltiplas regiões ativas;
- edição e transcodificação profissional;
- Tutor com personalização avançada;
- IA generativa e escolha de seu provedor;
- aplicativo Mobile;
- broker dedicado, salvo se a fila no banco falhar nos limites medidos.

## Critérios para iniciar desenvolvimento

- [x] Cliente primário e stack aprovados.
- [x] Identidade, fila, armazenamento próprio, Publicação e IA inicial decididos.
- [x] Desenvolvimento, homologação e produção definidos.
- [ ] Baseline aceita pela equipe executora.
- [ ] Contratos internos, estados e erros detalhados para o primeiro incremento.
- [ ] Ambiente local reproduzível com simuladores e dados fictícios.
- [ ] Backlog técnico priorizado.
- [ ] Auditoria, campos de log e métricas técnicas do primeiro incremento definidos.

## Critérios para iniciar piloto real

- [ ] Fluxo completo testado com Usuários iniciantes.
- [ ] Nenhum problema crítico de UX aberto.
- [ ] Nenhum risco crítico de segurança sem tratamento aprovado.
- [ ] Publicação idempotente e reconciliável comprovada.
- [ ] Tokens, arquivos e temporários protegidos.
- [ ] Autorização entre Projetos verificada.
- [ ] Logs testados sem dados proibidos.
- [ ] Cópia e restauração comprovadas.
- [ ] Alertas e responsáveis operacionais ativos.
- [ ] Resposta a incidente exercitada.
- [ ] Privacidade, retenção e suporte aprovados.
