# Próximas Ações

**Fase de referência:** após Sprint 4 — Arquitetura Técnica do MVP

**Atualizado em:** 19 de julho de 2026

## Objetivo imediato

Transformar a arquitetura de referência em um plano implementável, fechando primeiro as decisões que mudam o produto, os contratos externos, a segurança e o custo do MVP.

Nenhuma escolha de tecnologia deve compensar a ausência de validação de segmento, Canal ou fluxo.

## Prioridade 0 — Bloqueios de produto

### 1. Selecionar o recorte do piloto

- Concluir a pesquisa com os segmentos candidatos.
- Escolher um segmento, um caso de uso e um tipo de Conteúdo.
- Identificar o Canal realmente usado pelo segmento.
- Confirmar quais resultados ajudam o Usuário a tomar uma decisão.
- Registrar o que permanece fora do MVP.

**Saída esperada:** recorte único do piloto, promessa e métricas de valor.

**Decisões fechadas:** segmento, formato, Canal e métricas essenciais.

### 2. Escolher o cliente primário

- Levantar dispositivos, conectividade, hábito de instalação e uso de arquivos.
- Comparar Web, Mobile e entrega combinada limitada.
- Testar primeiro acesso, retorno de autorização e seleção de mídia no dispositivo real.
- Definir qual cliente recebe o ciclo completo primeiro e qual profundidade terá o segundo.

**Saída esperada:** decisão Web/Mobile, forma de distribuição e requisitos mínimos do dispositivo.

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

### 5. Validar identidade e recuperação

- Testar método de entrada com participantes do segmento.
- Comparar provedores gerenciados segundo segurança, acessibilidade, custo e operação.
- Validar Web, Mobile, recuperação, revogação e bloqueio.
- Definir autenticação reforçada para administração.
- Documentar sessão, expiração e encerramento.

**Saída esperada:** provedor, fluxo e requisitos de sessão aprovados.

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

### 8. Decidir o papel de IA no Tutor

- Construir e testar a jornada determinística como referência.
- Comparar benefício real da compreensão livre e sugestão assistida.
- Avaliar qualidade, custo, retenção, treinamento, região e subprocessadores.
- Definir contexto máximo, campos proibidos, validação de saída e degradação.
- Manter Publicação, consentimento, conexão e exclusão fora das ferramentas autônomas.

**Saída esperada:** decisão de usar ou não IA no MVP e, se usada, contrato de segurança e dados.

## Prioridade 3 — Detalhamento para implementação

### 9. Fechar escolhas da plataforma

- Selecionar linguagem e frameworks a partir da capacidade da equipe.
- Selecionar nuvem, região, banco gerenciado, armazenamento privado e cofre.
- Decidir se o outbox no banco é suficiente ou se haverá fila gerenciada.
- Definir solução de auditoria, logs, métricas, rastreamento e alertas.
- Estimar custo por Usuário, Publicação, transferência, armazenamento e uso de IA.

**Saída esperada:** conjunto tecnológico aprovado com custo inicial e limites.

### 10. Detalhar contratos dos módulos

- Definir comandos, consultas e eventos de cada módulo.
- Definir estados, transições e códigos de erro.
- Formalizar contrato dos adaptadores de Canal, armazenamento, identidade e IA.
- Definir idempotência, outbox, tentativa e reconciliação.
- Definir escopo de autorização por caso de uso.
- Definir dados mínimos de auditoria e telemetria.

**Saída esperada:** contratos testáveis sem dependência da interface do provedor.

### 11. Planejar ambientes e operação

- Isolar desenvolvimento, homologação e produção.
- Definir contas de teste, callbacks, chaves e dados permitidos por ambiente.
- Definir cópias, recuperação, rotação e limpeza temporária.
- Aprovar metas iniciais de disponibilidade, recuperação e capacidade.
- Criar alertas com responsável e ação inicial.
- Preparar procedimentos de desligar Canal, revogar token e desabilitar IA.

**Saída esperada:** plano operacional e de continuidade anterior à produção.

### 12. Converter o MVP em incrementos

Ordem recomendada:

1. Identidade, Projeto e autorização.
2. Tutor determinístico e Orquestrador da jornada.
3. Conteúdo, versões e Biblioteca.
4. Conta Conectada e adaptador do Canal.
5. Confirmação, Publicação, outbox, worker e reconciliação.
6. Métricas, Relatório e recomendação simples.
7. Armazenamento conectado e mídia do formato escolhido.
8. Mobile ou Web secundário, se aprovado no recorte.

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

## Decisões que bloqueiam implementação

- segmento, tipo de Conteúdo, Canal e métricas;
- cliente primário e forma de distribuição;
- provedor de identidade e recuperação;
- armazenamento conectado e limites de arquivo;
- uso e provedor de IA;
- nuvem, região e gestão de segredos;
- política de dados, retenção e exclusão;
- metas de recuperação, disponibilidade e custo;
- responsáveis por segurança, privacidade e operação.

## Decisões que podem esperar até depois do MVP

- separação em microserviços;
- múltiplos Canais e lote multicanal;
- Agendamento e Campanha;
- colaboração e múltiplos papéis;
- motor de busca dedicado;
- armazém analítico;
- múltiplas regiões ativas;
- edição e transcodificação profissional;
- Tutor com personalização avançada.

## Critérios para iniciar desenvolvimento

- [ ] Recorte do piloto aprovado.
- [ ] Cliente primário aprovado.
- [ ] Canal e armazenamento considerados viáveis.
- [ ] Identidade e recuperação aprovadas.
- [ ] Uso de IA decidido.
- [ ] Ameaças e dados revisados.
- [ ] Plataforma e ambientes definidos.
- [ ] Contratos e estados fechados.
- [ ] Backlog técnico priorizado.
- [ ] Métricas, auditoria e logs definidos.

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
