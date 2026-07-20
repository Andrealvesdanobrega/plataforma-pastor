# Próximas Ações

**Fase de referência:** após Sprint 6 — Primeiro Fluxo Vertical do MVP

**Atualizado em:** 20 de julho de 2026

## Objetivo imediato

Transformar o fluxo **pastor convidado → aviso textual → Página do Facebook → confirmação e link** em um incremento demonstrável com simulador e, em paralelo, validar as hipóteses e a Integração que bloqueiam seu uso real.

As escolhas técnicas não compensam a ausência de validação de segmento, Canal ou fluxo. Adaptadores simulados liberam o núcleo; nenhum simulador autoriza Publicação real ou piloto.

## Prioridade 0 — Bloqueios de produto

### 1. Validar o primeiro Usuário e a dor

- Entrevistar pastores que cuidam pessoalmente da comunicação de pequenas igrejas.
- Verificar exemplos passados de avisos, frequência, ferramentas, ajuda recebida e falhas reais.
- Confirmar que administram uma Página do Facebook elegível e publicam texto sem mídia.
- Testar se confirmação, link e próximo passo resolvem a necessidade imediata.
- Registrar evidência favorável ou rejeitar a hipótese sem adicionar funcionalidades.

**Saída esperada:** decisão sustentada de manter ou revisar pastor, aviso textual e Facebook Pages.

**Critério favorável:** existe tarefa recorrente, acesso legítimo à Página e disposição para uma Publicação controlada.

### 2. Validar Web responsivo como cliente primário

- Levantar dispositivos, conectividade, hábito de instalação e uso de arquivos.
- Testar primeiro acesso Web, código por e-mail e retorno de autorização no dispositivo real.
- Identificar qualquer bloqueio que não possa ser resolvido no navegador.
- Reabrir a decisão de Mobile somente se a evidência demonstrar dependência de instalação ou recurso nativo.

**Saída esperada:** compatibilidade e requisitos mínimos do Web confirmados, ou evidência explícita para revisar ENG-01 e ENG-04.

## Prioridade 1 — Provas de viabilidade

### 3. Provar Facebook Pages

- Criar ou obter ambiente e conta oficial de teste.
- Verificar autorização, permissões mínimas, renovação e revogação.
- Validar Publicação textual sem mídia e seus limites.
- Validar Publicação, identificador externo, estado incerto e consulta posterior.
- Validar assinatura, repetição e ordem de webhooks quando existirem.
- Confirmar link externo e se existe uma métrica simples útil, com definição, atraso e permissão.
- Mapear revisão de aplicativo, termos, custos e prazos externos.

**Saída esperada:** matriz de capacidades, permissões, erros normalizados, requisitos de revisão e recomendação de seguir ou rejeitar Facebook Pages.

**Critério de aprovação:** o ciclo pode ser executado com confirmação, idempotência e reconciliação.

### 4. Confirmar que mídia pode esperar

- Examinar avisos reais usados pelo público.
- Verificar se texto sem mídia entrega valor suficiente para o primeiro ciclo.
- Registrar pedidos de imagem ou vídeo sem implementá-los.
- Reabrir armazenamento conectado somente se a hipótese textual for rejeitada e um novo recorte for aprovado.

**Saída esperada:** evidência de que mídia e armazenamento não são necessários no primeiro fluxo, ou bloqueio explícito para revisão de escopo.

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
- Modelar tomada de conta, isolamento, token de Página, webhook se usado, Conteúdo religioso, Tutor e Publicação.
- Classificar severidade e responsável por tratamento.
- Definir critérios que bloqueiam o piloto.

**Saída esperada:** ameaças atualizadas e plano de tratamento com responsáveis.

### 7. Aprovar governança de dados

- Inventariar dado, finalidade, origem, destinatário e subprocessador do fluxo textual.
- Definir fundamento aplicável e textos de transparência com revisão qualificada.
- Aprovar retenção de perfil, aviso, token, Publicação, métrica, Tutor, auditoria, logs e cópias.
- Definir exportação, correção, revogação, anonimização e exclusão.
- Definir residência e transferência de dados.
- Nomear responsáveis por privacidade e incidente.

**Saída esperada:** política operacional de dados pronta para implementação e piloto.

### 8. Validar o Tutor determinístico

- Construir e testar a jornada determinística como referência.
- Medir intenções não compreendidas, retorno às opções guiadas e utilidade das sugestões controladas.
- Impedir que regras tentem cobrir casos fora do ciclo aprovado.
- Manter o contrato de assistência desabilitado e sem envio de dados a provedor de IA.
- Limitar as intenções a **Publicar um aviso**, continuar, revisar, resolver e ver Publicação.
- Registrar hipótese futura somente se a rigidez impedir esse fluxo.

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

Ordem do primeiro fluxo:

1. Convite, entrada por código, Usuário, Projeto e autorização.
2. Tutor determinístico com **Publicar um aviso**.
3. Aviso textual, Versão, salvamento e Biblioteca simples.
4. Contrato e simulador de Facebook Pages.
5. Prévia, confirmação, Publicação, outbox, worker e reconciliação.
6. Estado, link, Resultado mínimo e próximo passo.
7. Adaptador real após prova e aprovação.
8. Endurecimento do Web nos dispositivos do primeiro público.

Cada incremento deve terminar com fluxo demonstrável, testes, auditoria, logs seguros e tratamento de falha.

**Saída esperada:** backlog técnico priorizado com critérios de pronto e dependências.

## Prioridade 4 — Validação antes do piloto

### 13. Testar a experiência completa

- Executar convite, entrada, configuração mínima, conexão da Página, aviso textual, Biblioteca, confirmação, Publicação, resultado e Tutor.
- Usar pastores responsáveis pela comunicação e seus dispositivos reais.
- Simular código vencido, conta expirada, perda de rede, timeout e resultado atrasado.
- Verificar compreensão de destino, salvamento, confirmação e limites do Tutor.
- Corrigir todo problema crítico definido no MVP.

**Saída esperada:** evidência de usabilidade e decisão de liberar ou revisar.

### 14. Executar verificação técnica e de segurança

- Testar autorização entre Projetos e objetos relacionados.
- Testar sessão Web conforme o cliente aprovado.
- Testar webhook forjado e repetido.
- Testar Publicação duplicada, timeout e reconciliação.
- Testar texto excessivo, script no Conteúdo e dado sensível em telemetria.
- Testar intenção não reconhecida e tentativa de fazer o Tutor publicar.
- Verificar dependências, segredos, configuração e telemetria.
- Restaurar uma cópia em ambiente isolado.
- Exercitar vazamento de token e suspensão do Canal.

**Saída esperada:** relatório de verificação, riscos residuais e aprovação formal do piloto.

## Decisões que bloqueiam o piloto, não o fluxo simulado

- validação do pastor responsável pela comunicação e do aviso textual;
- prova de Facebook Pages, permissões, texto, estado, revogação, link e revisão externa;
- definição de resultado mínimo e possível métrica externa;
- política de dados, retenção, exclusão, fundamento e subprocessadores;
- metas de recuperação, disponibilidade, capacidade e custo;
- responsáveis por segurança, privacidade, suporte e operação;
- limiares quantitativos do piloto.

## Decisões que podem esperar até depois do MVP

- mídia, upload e armazenamento conectado;
- separação em microserviços;
- múltiplos Canais e lote multicanal;
- Agendamento e Campanha;
- colaboração e múltiplos papéis;
- motor de busca dedicado;
- armazém analítico;
- múltiplas regiões ativas;
- edição e transcodificação profissional;
- Tutor com personalização avançada;
- cadastro público irrestrito e notificações externas;
- busca, filtros avançados, etiquetas, duplicação e ações em massa;
- IA generativa e escolha de seu provedor;
- aplicativo Mobile;
- broker dedicado, salvo se a fila no banco falhar nos limites medidos.

## Critérios para iniciar desenvolvimento

- [x] Cliente primário e stack aprovados.
- [x] Identidade, fila, armazenamento próprio, Publicação e IA inicial decididos.
- [x] Desenvolvimento, homologação e produção definidos.
- [x] Primeiro Usuário, dor, objetivo e fluxo vertical definidos como hipótese.
- [x] Telas, entidades, dados, Integrações e métricas mínimas delimitados.
- [ ] Baseline aceita pela equipe executora.
- [ ] Contratos internos, estados e erros do fluxo textual detalhados.
- [ ] Ambiente local reproduzível com simuladores e dados fictícios.
- [ ] Incrementos do fluxo vertical priorizados.
- [ ] Auditoria, campos de log e métricas técnicas do primeiro incremento definidos.

## Critérios para iniciar piloto real

- [ ] Fluxo completo testado com Usuários iniciantes.
- [ ] Hipótese de pastor, aviso textual e Facebook Pages validada.
- [ ] Facebook Pages aprovado por prova técnica e requisitos externos.
- [ ] Nenhum problema crítico de UX aberto.
- [ ] Nenhum risco crítico de segurança sem tratamento aprovado.
- [ ] Publicação idempotente e reconciliável comprovada.
- [ ] Tokens, texto, sessão e segredos protegidos.
- [ ] Autorização entre Projetos verificada.
- [ ] Logs testados sem dados proibidos.
- [ ] Cópia e restauração comprovadas.
- [ ] Alertas e responsáveis operacionais ativos.
- [ ] Resposta a incidente exercitada.
- [ ] Privacidade, retenção e suporte aprovados.
