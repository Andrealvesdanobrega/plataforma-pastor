# Próximas Ações

**Fase:** Transição para implementação do núcleo assistido

**Atualizado em:** 20 de julho de 2026

## Objetivo imediato

Validar e detalhar o ciclo **Intenção → Tutor → Conteúdo → versão para Destino simulado**, preparando a implementação do núcleo sem escolher canal, criar integração real ou alterar ENG-01 a ENG-15.

## Prioridade 0 — Validar a filosofia com Usuários

### 1. Validar dor e linguagem

- Entrevistar o primeiro recorte e ao menos um recorte comparável.
- Investigar comportamento passado, esforço, dependência, erro e consequência.
- Pedir que expliquem “Comunicação Assistida”, “Presença Digital” e “Destino de Publicação”.
- Rejeitar linguagem que pareça publicador, curso ou automação sem controle.

**Saída:** dor, segmento e vocabulário confirmados ou revistos sem ampliar o MVP.

### 2. Testar o Tutor

- Prototipar a captura de uma Intenção curta.
- Medir quais perguntas realmente mudam o resultado.
- Verificar se o Tutor reutiliza contexto sem parecer invasivo.
- Testar recomendação, motivo, ensino contextual e possibilidade de recusa.
- Comparar experiência iniciante com controle progressivo para Usuário avançado.

**Saída:** contrato de perguntas, explicações e limites validado.

### 3. Testar preservação da Intenção

- Usar avisos reais anonimizados ou criados para teste.
- Comparar fonte e versão adaptada a um Destino simulado.
- Pedir ao Usuário que identifique mudanças, motivo e mensagem principal.
- Bloquear regra que altere sentido sem decisão explícita.

**Saída:** conjunto mínimo de regras determinísticas aprovado ou hipótese rejeitada.

## Prioridade 0 — Sincronizar arquitetura antes do modelo físico

### 4. Atualizar documentos ainda não autorizados

Em Sprint com escopo explícito, atualizar:

- `DATA-MODEL.md` para Intenção, Conhecimento do Usuário, Presença Digital, Destino e versão derivada;
- `TECHNICAL-ARCHITECTURE.md` apenas no vocabulário e limites de módulos, preservando ENG-01 a ENG-15;
- `SECURITY.md` para ameaças, consentimento, correção, retenção e exclusão do conhecimento absorvido;
- `OPEN-DECISIONS.md` para retirar a escolha antecipada de canal e registrar critérios de Destino;
- `BACKLOG.md` para substituir POCs específicas por incrementos do núcleo e portões genéricos.

**Saída:** documentação sem modelos concorrentes antes de migrações ou contratos físicos.

### 5. Fechar governança do conhecimento

- Definir quais fatos podem ser absorvidos e reutilizados.
- Registrar origem, finalidade, escopo, atualidade e responsável.
- Definir correção, exclusão, consentimento e retenção.
- Proibir segredos, conversa integral e inferência sensível por padrão.

**Saída:** política funcional e de segurança aprovada antes de persistir conhecimento.

## Prioridade 1 — Implementar o núcleo com simulador

### 6. Organizar incrementos verticais

1. convite, identidade e Projeto;
2. Tutor determinístico e Intenção;
3. contexto mínimo e conhecimento controlado;
4. Conteúdo textual, versões e Biblioteca;
5. Presença Digital mínima e Destino simulado;
6. adaptação determinística e explicação;
7. revisão, confirmação do entendimento, eventos e auditoria.

Cada incremento deve incluir acessibilidade, autorização por Projeto, retomada, logs seguros e testes de estados.

### 7. Definir o simulador de Destino

- Declarar capacidades e limites genéricos suficientes para o aviso textual.
- Simular sucesso, incompatibilidade e indisponibilidade sem serviço externo.
- Registrar regras aplicadas e diferenças entre fonte e versão.
- Não simular prova de permissão, idempotência externa ou política de provedor.

**Saída:** contrato substituível, sem preferência por canal.

### 8. Instrumentar as hipóteses

- Medir intenção compreendida, perguntas, repetição, abandono e retomada.
- Registrar aceitação, adaptação e rejeição de recomendações sem coletar texto integral em analytics.
- Medir reconhecimento da Intenção, compreensão do motivo e aprendizado.
- Manter meta zero para perda e ação externa.

## Prioridade 2 — Preparar a decisão futura de Destino

### 9. Definir critérios, sem escolher candidato

- adoção real pelo segmento;
- tarefa e formato compatíveis;
- autorização e permissão mínima;
- publicação, estado, referência, revogação e recuperação;
- idempotência ou reconciliação compatível com ENG-10;
- custos, revisão, políticas, privacidade e suporte.

### 10. Executar POC somente após evidência de uso

- Escolher um único candidato por decisão aprovada.
- Usar conta e dados oficiais de teste.
- Provar o contrato completo antes de implementar adaptador real.
- Rejeitar o candidato se não cumprir confirmação e reconciliação; não ampliar escopo por impulso.

## Portões de passagem

### Iniciar núcleo com simulador

- [x] Filosofia e missão consolidadas.
- [x] ENG-01 a ENG-15 preservadas.
- [x] Fluxo assistido mínimo definido.
- [ ] Documentos de dados e segurança sincronizados em escopo autorizado.
- [ ] Governança do conhecimento aprovada.

### Considerar primeiro Destino real

- [ ] Dor e tarefa confirmadas por entrevistas.
- [ ] Necessidade de distribuição real demonstrada.
- [ ] Critérios genéricos aprovados.
- [ ] Um candidato selecionado sem centralizar o produto.
- [ ] POC oficial completa aprovada.

### Iniciar piloto real

- [ ] Incremento assistido concluído e testado com representantes.
- [ ] Integração real segura, auditável e reconciliável.
- [ ] Privacidade, retenção, exclusão, suporte e incidente aprovados.
- [ ] Todos os critérios cumulativos de ENG-15 atendidos.

## Decisões que continuam adiadas

- primeiro Destino de Publicação;
- segundo Destino e distribuição simultânea;
- IA generativa;
- mídia e armazenamento conectado;
- Agendamento, Campanha e colaboração;
- Mobile, broker dedicado, microserviços e múltiplas regiões;
- métricas avançadas, anúncios, CRM e comércio eletrônico.
