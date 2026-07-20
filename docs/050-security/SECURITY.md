# Segurança do MVP

**Versão:** 0.1

**Fase:** Sprint 4 — Arquitetura Técnica do MVP

**Atualizado em:** 19 de julho de 2026

**Estado:** baseline de segurança; controles ainda não implementados nem testados

## 1. Objetivo

Definir os requisitos, controles, responsabilidades e critérios mínimos de segurança e privacidade do MVP. Este documento cobre Web, Mobile, backend, Tutor, Orquestrador, dados, armazenamento e Integrações externas.

Segurança existe para preservar a autonomia e a confiança do Usuário: proteger Conteúdos e contas, impedir ações externas não autorizadas, tornar incidentes detectáveis e permitir recuperação.

## 2. Escopo e premissas

### Incluído

- identidade, sessão, recuperação e autorização;
- isolamento entre Usuários e Projetos;
- Conteúdo, versões, mídias, Biblioteca e Relatórios;
- Contas Conectadas, tokens e permissões;
- Tutor, IA opcional e Orquestrador;
- Publicação, idempotência, webhooks e métricas;
- Web, Mobile, API, worker, banco, armazenamento e observabilidade;
- administração, auditoria, cópias e resposta a incidentes;
- privacidade, retenção, exportação e exclusão.

### Premissas

- Um Usuário proprietário e um Projeto principal compõem o caminho do MVP.
- O MVP integra um Canal e pode integrar um serviço de armazenamento do Usuário.
- Vídeos e arquivos grandes permanecem preferencialmente em Google Drive, OneDrive, Dropbox ou serviço equivalente conectado.
- A plataforma não guarda senhas de serviços externos.
- Tutor é a única porta de entrada da experiência, mas não é fronteira de autorização.
- Serviços externos, dispositivos e todo Conteúdo recebido são considerados não confiáveis.
- Nenhum controle é considerado efetivo antes de implementado e verificado.

## 3. Objetivos de segurança

1. Somente a pessoa autorizada acessa seu Projeto e Conteúdo.
2. Nenhuma Publicação acontece sem confirmação explícita e rastreável.
3. Tokens externos são usados somente para a finalidade e o destino autorizados.
4. Falhas, repetição de requisição e timeout não produzem envio duplicado inseguro.
5. Conteúdo malicioso não se transforma em comando para Tutor, sistema ou navegador.
6. Dados coletados são mínimos, protegidos e retidos por prazo justificado.
7. Logs e suporte permitem diagnóstico sem expor Conteúdo ou segredo.
8. Eventos sensíveis possuem auditoria íntegra.
9. Cópias e procedimentos permitem recuperar dados próprios dentro de metas aprovadas.
10. Um serviço externo comprometido ou indisponível tem impacto limitado.

## 4. Princípios

- Negar por padrão.
- Menor privilégio e permissão incremental.
- Separar identidade da plataforma de autorização externa.
- Validar no backend, independentemente do que a interface mostrar.
- Tratar entrada e saída como não confiáveis.
- Não guardar segredo onde metadado ou referência bastar.
- Não guardar arquivo grande quando uma referência autorizada bastar.
- Confirmar ações de impacto com objeto, conta e destino claros.
- Minimizar coleta, exposição, retenção e acesso humano.
- Registrar evento de segurança sem registrar o dado protegido.
- Falhar de forma segura e recuperável.
- Revisar e testar controles proporcionalmente ao risco.

## 5. Ativos protegidos

| Ativo | Sensibilidade | Impacto de exposição ou alteração |
|---|---|---|
| Identidade e sessão | Crítica | Tomada de conta e acesso a todos os objetos permitidos |
| Token de Conta Conectada | Crítica | Publicação, leitura ou ação indevida em serviço externo |
| Segredo de Integração | Crítica | Comprometimento amplo do conector ou da plataforma |
| Conteúdo e versões | Alta | Exposição de material inédito, alteração ou perda autoral |
| Mídias e referências externas | Alta | Exposição de arquivo, metadado ou acesso indireto |
| Confirmação de Publicação | Alta | Ação pública indevida e perda de confiança |
| Projeto, objetivo e público | Moderada/alta | Perfil comportamental ou comercial do Usuário |
| Interações com o Tutor | Alta | Conteúdo livre, intenção e contexto potencialmente sensíveis |
| Métricas e Relatórios | Moderada | Exposição de desempenho e estratégia |
| Auditoria | Alta | Perda de responsabilização ou exposição de operação |
| Logs e telemetria | Variável | Correlação de atividade ou vazamento acidental |
| Cópias e exportações | Crítica | Concentração ampla de dados |

## 6. Classificação de dados

| Classe | Exemplos | Controles mínimos |
|---|---|---|
| Público | Nome oficial do Canal, textos públicos do produto | Integridade e origem controladas |
| Interno | Configuração não secreta, catálogo de capacidades | Acesso por equipe autorizada |
| Confidencial | Perfil, Conteúdo, Relatório, identificadores externos | Criptografia, autorização por Projeto e retenção |
| Restrito | Tokens, segredos, códigos de recuperação, chaves | Cofre, acesso mínimo, rotação e proibição em logs |

Dados pessoais e sensíveis podem aparecer no Conteúdo ou texto livre sem marcação prévia. Por isso, esses campos são tratados como Confidenciais por padrão.

## 7. Fronteiras de confiança

```text
Dispositivo do Usuário
   │ não confiável
   ▼
Borda e Cliente Web/Mobile
   │ requisição autenticada e validada
   ▼
API / Backend
   ├── Banco privado
   ├── Armazenamento privado
   ├── Cofre de segredos
   └── Fila / Worker
           │ chamadas autenticadas, limitadas e auditadas
           ▼
     Serviços externos não confiáveis
     Canal, Drive, identidade, IA e notificações
```

Fronteiras específicas:

- navegador para API;
- aplicativo instalado para API;
- API para worker;
- aplicação para banco, armazenamento e cofre;
- backend para cada provedor externo;
- webhook externo para backend;
- Conteúdo do Usuário para Tutor ou gerador;
- equipe operacional para produção;
- produção para cópias e exportações.

## 8. Ameaças prioritárias

| Ameaça | Cenário | Controle principal |
|---|---|---|
| Tomada de conta | Sessão ou recuperação comprometida | Provedor maduro, sessão curta, proteção contra abuso e revogação |
| Acesso entre Projetos | Identificador de outro objeto é manipulado | Autorização por participação em todo caso de uso e testes negativos |
| Roubo de token externo | Token aparece em banco aberto, cliente ou log | Cofre ou criptografia separada, redação e menor escopo |
| Publicação sem intenção | Interface, Tutor ou repetição dispara envio | Confirmação explícita, autorização backend, idempotência e auditoria |
| Repetição de webhook | Evento válido é reenviado | Assinatura, janela temporal, identificador único e processamento idempotente |
| Resultado incerto | Timeout leva a nova Publicação | Reconciliação externa antes de repetir |
| Arquivo malicioso | Upload explora parser ou navegador | Limite, detecção de tipo, varredura, isolamento e download seguro |
| Acesso indevido a arquivo conectado | Escopo amplo ou endereço duradouro vaza | Permissão mínima, referência sem URL pública e acesso temporário |
| Injeção no Tutor | Conteúdo manda ignorar regras ou usar ferramentas | Separação entre dados e instruções, ferramentas restritas e validação |
| Exfiltração por IA | Contexto excessivo enviado ao provedor | Minimização, filtro, contrato e proibição de segredos |
| Script no Conteúdo | Texto é renderizado como código no Web | Escape contextual, sanitização e política de conteúdo |
| Abuso de recursos | Upload, IA ou Publicação em massa | Limites por identidade/Projeto e orçamento operacional |
| Dependência comprometida | Pacote ou imagem maliciosa entra na entrega | Fixação, verificação, inventário e atualização controlada |
| Operador abusivo | Pessoa interna acessa ou exporta dados | Função separada, acesso temporário, auditoria e revisão |
| Cópia exposta | Backup concentra dados e segredos | Criptografia, acesso separado, retenção e teste controlado |

Uma modelagem de ameaças específica deve ser refeita quando Canal, identidade, armazenamento e IA forem escolhidos.

## 9. Identidade e autenticação

### 9.1 Provedor

Preferir serviço gerenciado de identidade com protocolos abertos, proteção contra abuso, recuperação segura, revogação e trilha operacional. A plataforma não implementará armazenamento próprio de senha no MVP sem justificativa e revisão específica.

### 9.2 Web

- Sessão em cookie `Secure`, `HttpOnly` e com política `SameSite` apropriada.
- Renovação de identificador após entrada e mudança de privilégio.
- Proteção contra falsificação em requisições que alteram estado.
- Expiração por tempo e revogação em encerramento, redefinição ou incidente.
- Token de sessão proibido em URL, armazenamento local, analytics e logs.

### 9.3 Mobile

- Autorização realizada pelo navegador do sistema, não por tela que imite o provedor.
- Proteção apropriada para cliente público e validação do retorno.
- Credencial renovável somente no armazenamento seguro do sistema operacional.
- Credencial curta em memória sempre que possível.
- Nenhum segredo compartilhado embutido no pacote.
- Logout remove estado local e revoga sessão no servidor conforme capacidade.

### 9.4 Recuperação e abuso

- Resposta não confirma existência de uma conta a pessoa não autenticada.
- Código ou link é curto, de uso único, expira e fica vinculado à intenção.
- Tentativas são limitadas e monitoradas.
- Mudança relevante invalida sessões conforme risco.
- Suporte não pede senha, token ou código completo.

### 9.5 Administração

- Identidade administrativa separada ou nível claramente distinto.
- Autenticação reforçada obrigatória.
- Acesso limitado por função e ambiente.
- Acesso emergencial documentado, temporário, alertado e revisado.

## 10. Autorização e isolamento

### 10.1 Regra central

Toda leitura ou escrita valida:

1. identidade ativa;
2. sessão válida;
3. participação ativa no Projeto;
4. ação permitida para o papel;
5. recurso pertencente ao mesmo Projeto;
6. estado do recurso compatível com a ação.

No MVP há somente o papel proprietário. A existência de um identificador não concede acesso.

### 10.2 Controles

- Verificação central e também no caso de uso sensível.
- Consultas sempre escopadas pelo Projeto autorizado.
- Relações cruzadas validadas: Conteúdo, Versão, Conta e Publicação devem pertencer ao mesmo contexto.
- Worker opera com identidade de serviço e recebe somente o identificador necessário.
- Acesso direto ao banco é restrito; conta da aplicação não possui privilégio administrativo.
- Defesa adicional no banco pode ser aplicada, mas não substitui regra do backend.
- Cache inclui escopo de Projeto na chave e nunca mistura respostas autenticadas.

### 10.3 Testes obrigatórios

- troca de `projeto_id`, `conteudo_id`, `conta_id`, `publicacao_id` e `relatorio_id`;
- enumeração e identificadores inexistentes;
- acesso depois de encerrar sessão ou participação;
- ação em estado inválido;
- leitura por URL antiga ou cache;
- worker recebendo objeto de outro Projeto.

## 11. Sessão, API e borda

- HTTPS obrigatório e configuração de transporte atualizada.
- Redirecionamento seguro e política de origem restrita.
- Limite de corpo, campos, arquivo, frequência e concorrência.
- Validação de esquema no limite da API e validação de regra no domínio.
- Mensagens externas não revelam pilha, consulta, segredo ou existência indevida de recurso.
- Identificador de correlação sem dado pessoal bruto.
- Operações perigosas aceitam chave de idempotência vinculada a ator, intenção e destino.
- Documentação e endpoints administrativos não ficam públicos em produção sem controle.
- Borda pode bloquear padrão abusivo, mas autorização continua no backend.

## 12. Segurança do Frontend Web

- Conteúdo do Usuário é escapado no contexto de saída.
- HTML rico, se futuramente permitido, usa sanitização estrita e lista aprovada.
- Política de conteúdo restringe scripts, conexões, quadros e origens.
- Aplicação não pode ser embutida por origem não autorizada.
- Links externos são identificados e abertos com proteção apropriada.
- Prévia de arquivo não executa conteúdo ativo no mesmo contexto da aplicação.
- Formulários não expõem dados sensíveis em endereço ou histórico.
- Analytics recebe somente eventos e propriedades aprovados.
- Dependências e ativos têm origem controlada.
- Erro do cliente é redigido antes do envio à telemetria.

## 13. Segurança do Aplicativo Mobile

- Pacote é assinado e distribuído por canal oficial definido.
- Configuração de produção não habilita diagnóstico detalhado ou endpoint de teste.
- Dados em cache são mínimos e protegidos pelo armazenamento oferecido pelo sistema.
- Captura de tela e área de transferência são avaliadas para telas realmente sensíveis, sem prejudicar acessibilidade sem necessidade.
- Links recebidos validam esquema, origem, estado e recurso antes de navegar.
- Arquivo compartilhado para o App é tratado como não confiável.
- Permissões do dispositivo são solicitadas no momento de uso e com finalidade clara.
- A API não confia em versão, dispositivo ou estado declarado pelo cliente.
- Bloqueio de dispositivo comprometido não é garantia do MVP; controles do servidor limitam impacto.

## 14. Contas Conectadas e segredos

### 14.1 OAuth e consentimento externo

- Estado de autorização aleatório, curto, de uso único e ligado à sessão.
- Retorno permitido somente em endereços registrados.
- Permissões mínimas; ampliação exige nova explicação e consentimento.
- Troca de código por token somente no backend quando o cliente for confidencial.
- Identidade externa confirmada antes de marcar Conta como ativa.
- Cancelamento ou erro não cria conexão parcial utilizável.
- Revogação e expiração atualizam o estado e bloqueiam novos trabalhos.

### 14.2 Armazenamento de tokens

Ordem de preferência:

1. cofre de segredos ou serviço de credenciais com referência no banco;
2. token criptografado por envelope com chave gerenciada separadamente;
3. nunca texto aberto no banco, configuração, cliente, fila ou log.

Metadados permitidos no banco: provedor, conta, permissões, expiração, estado, versão da credencial e referência protegida.

### 14.3 Chaves e segredos da plataforma

- Criados e entregues pelo cofre, não inseridos em código.
- Separados por ambiente e finalidade.
- Rotação definida e testada.
- Acesso por identidade de serviço e menor privilégio.
- Uso e falha de acesso monitorados.
- Valor nunca aparece em saída de ferramenta, alerta ou suporte.

## 15. Integrações externas e webhooks

### 15.1 Saída

- Lista de destinos externos permitidos por adaptador.
- Resolução e conexão protegidas contra acesso a rede interna quando endereço puder ser influenciado.
- Timeout, tamanho máximo de resposta e número de redirecionamentos limitados.
- Certificado e nome do serviço validados.
- Resposta externa validada antes de virar estado de domínio.
- Erro sanitizado antes de persistir ou mostrar.

### 15.2 Entrada por webhook

- Assinatura verificada sobre os bytes corretos conforme contrato do provedor.
- Segredo separado por ambiente e, quando possível, por Integração.
- Carimbo de tempo e janela contra repetição.
- Identificador do evento deduplicado.
- Corpo e frequência limitados antes de trabalho caro.
- Resposta rápida; processamento de domínio assíncrono.
- Evento desconhecido não altera estado e é observado sem registrar dado indevido.

### 15.3 Dependência e indisponibilidade

- Saúde por Integração observada separadamente.
- Circuito de proteção e limite de concorrência evitam cascata.
- Nova tentativa só para erro temporário e com orçamento.
- Resultado incerto entra em reconciliação.
- Tutor explica indisponibilidade sem expor detalhe interno.

## 16. Segurança de arquivos e armazenamento

### 16.1 Princípio

Vídeos e arquivos grandes permanecem preferencialmente no serviço conectado pelo Usuário. A plataforma guarda metadados e referência. Isso reduz cópia, mas não elimina responsabilidade sobre permissões, processamento temporário e exposição de metadados.

### 16.2 Arquivo conectado

- Pedir acesso ao menor conjunto possível, preferindo seleção explícita.
- Persistir identificador estável e versão, nunca endereço público permanente.
- Confirmar Conta, Projeto e finalidade antes da leitura.
- Validar tipo, tamanho, integridade e estado antes de Publicar.
- Não seguir URL arbitrária fornecida como se fosse arquivo conectado.
- Revogação ou remoção torna a referência indisponível.

### 16.3 Objeto hospedado pela plataforma

- Contêiner privado e bloqueio de acesso público.
- Criptografia em repouso com chave gerenciada.
- Nome interno aleatório, sem confiar no nome original como caminho.
- Envio e download por autorização curta, vinculada a objeto e operação.
- Validação do tipo real, tamanho e extensão.
- Varredura ou isolamento proporcional ao formato.
- Prévia servida sem permitir execução ativa no domínio principal.
- Remoção conforme retenção e exclusão do Projeto.

### 16.4 Processamento temporário

- Área efêmera isolada por trabalho.
- Tamanho e duração máximos.
- Criptografia e acesso somente pelo worker responsável.
- Exclusão após sucesso, falha final ou prazo máximo.
- Monitoramento de objetos vencidos e processo de limpeza de segurança.
- Conteúdo temporário proibido em cópias não intencionais e logs.

### 16.5 Riscos específicos de mídia

- arquivo comprimido excessivo;
- formato declarado diferente do real;
- metadado com dado pessoal ou localização;
- parser vulnerável;
- miniatura maliciosa;
- conteúdo protegido por direito de terceiros;
- custo ou tempo de transferência abusivo.

O MVP evita edição e transcodificação complexa, reduzindo a superfície.

## 17. Segurança do Conteúdo e da Biblioteca

- Conteúdo sempre associado a Projeto autorizado.
- Versão Publicada é imutável.
- Edição concorrente usa versão para evitar perda silenciosa.
- Arquivar não apaga; excluir segue política e confirma impacto.
- Busca não retorna trechos de outro Projeto.
- Índice, cache e miniatura preservam o mesmo controle do objeto de origem.
- Exportação exige revalidação, auditoria e arquivo protegido.
- Corpo do Conteúdo não aparece em nome de arquivo operacional, chave de cache ou log.

## 18. Segurança do Tutor e de IA

### 18.1 Fronteira

Tutor conduz a experiência, mas toda permissão pertence ao backend. Texto do Usuário, Conteúdo, resultado externo e saída de modelo são entradas não confiáveis.

### 18.2 Ameaças

- instrução no Conteúdo tentando substituir regras do Tutor;
- pedido para revelar outro Projeto ou segredo;
- saída que inventa resultado ou autorização;
- sugestão nociva, discriminatória ou inadequada ao público;
- envio excessivo de dado pessoal ao provedor;
- uso da conversa para treinamento não autorizado;
- modelo induzindo chamada de ferramenta perigosa;
- custo abusivo por entradas longas ou repetidas.

### 18.3 Controles

- Separar instruções internas, contexto confiável e dados do Usuário.
- Montar contexto no backend após autorização, nunca a partir de identificador aceito cegamente.
- Remover segredos e limitar campos, tamanho e histórico enviados.
- Usar lista de intenções e ferramentas permitidas.
- Definir esquema de saída e validar tipo, referência e ação.
- Tratar resposta como proposta, não comando.
- Exigir confirmação na interface e autorização no caso de uso.
- Não oferecer ao modelo ferramenta de consentir, publicar, excluir, desconectar ou mudar permissão autonomamente.
- Limitar frequência, tamanho, custo e tempo.
- Manter caminho determinístico quando IA falhar.
- Registrar provedor, modelo lógico, política e resultado, sem guardar conversa integral por padrão.

### 18.4 Dados e provedor

Antes de usar IA no piloto, aprovar:

- finalidade e base aplicável;
- categorias de dado enviadas;
- retenção e uso ou não para treinamento;
- região e subprocessadores;
- procedimento de exclusão;
- qualidade e segurança para português e público escolhido;
- custo máximo e degradação;
- revisão humana e comunicação de que se trata de sugestão.

Sem essa aprovação, Tutor opera com regras e opções guiadas.

## 19. Segurança do Orquestrador e da Publicação

### 19.1 Orquestrador

- Recebe intenção normalizada e contexto autenticado.
- Só chama casos de uso em lista permitida.
- Não aceita nome de função arbitrário vindo do Tutor ou cliente.
- Valida pré-condições e confirmação antes do comando.
- Persiste operação externa antes de executá-la.
- Não guarda estado crítico somente em memória.

### 19.2 Confirmação de Publicação

A confirmação registra:

- Usuário e Projeto;
- Conteúdo e Versão exata;
- Conta Conectada e Canal;
- instante e origem da solicitação;
- chave de idempotência;
- resultado inicial da autorização.

Qualquer mudança de Versão ou destino após a prévia exige nova confirmação.

### 19.3 Idempotência e reconciliação

- A mesma intenção repetida retorna a mesma Publicação enquanto válida.
- Chave é vinculada a ator, Conteúdo, Versão e destino.
- Tentativa externa tem sequência e resultado próprio.
- Timeout produz estado incerto, não nova Publicação automática.
- Consulta externa ou webhook reconcilia antes de nova tentativa.
- Estado final não volta a processamento.

### 19.4 Multicanal futuro

- Uma Publicação por destino.
- Confirmação lista todas as contas e adaptações.
- Falha parcial não desfaz sucesso externo.
- Permissões e limites independentes por Canal.
- Métricas não são somadas sem semântica compatível.

O MVP implementa somente um Canal e não expõe lote multicanal.

## 20. Auditoria

### 20.1 Diferença entre auditoria e log

- **Auditoria:** quem realizou qual ação sensível, sobre qual recurso, quando e com qual resultado.
- **Log:** informação técnica para operar e diagnosticar.
- **Evento de produto:** comportamento agregado para aprender sobre uso.

Uma categoria não substitui a outra e cada uma possui acesso e retenção próprios.

### 20.2 Eventos mínimos

- entrada, recuperação relevante, bloqueio e encerramento;
- consentimento concedido, alterado ou retirado;
- participação e acesso administrativo;
- Conta Conectada iniciada, ativada, renovada, falhou ou foi revogada;
- Versão congelada;
- Publicação confirmada, tentada, reconciliada, concluída ou falhou;
- segredo, política ou configuração sensível alterada;
- exportação, exclusão ou anonimização;
- restauração e acesso emergencial.

### 20.3 Integridade e acesso

- Registro append-only por caminho da aplicação.
- Serviço e operadores comuns não alteram eventos.
- Acesso por necessidade e consulta auditada.
- Exportação protegida e com prazo.
- Alertas para lacuna, alteração indevida ou volume anômalo.
- Retenção aprovada por tipo de evento.

## 21. Logs, métricas e rastreamento

### 21.1 Lista proibida

Não entram em logs, erros, rastros ou métricas:

- senha, código, token, cookie e cabeçalho de autorização;
- URL assinada ou endereço com segredo;
- corpo integral de Conteúdo;
- arquivo ou miniatura privada;
- texto livre do Tutor por padrão;
- resposta completa de provedor que possa conter dado pessoal;
- chave de criptografia ou configuração secreta.

### 21.2 Controles

- Biblioteca central de redação e campos permitidos.
- Estrutura de log definida, evitando concatenação livre.
- Ambientes inferiores também não aceitam segredo real em log.
- Acesso a observabilidade por função e autenticação reforçada.
- Retenção curta para diagnóstico e agregação para tendência.
- Alerta e procedimento de remoção se dado proibido for detectado.
- Rastreamento usa correlação opaca e amostragem controlada.

## 22. Privacidade e ciclo de vida

### 22.1 Requisitos antes do piloto

- Mapear categorias de dados, finalidade, origem, destinatários e retenção.
- Definir fundamento jurídico aplicável com responsável qualificado.
- Publicar explicações simples e mecanismo de consentimento quando necessário.
- Definir controlador, operadores e subprocessadores.
- Formalizar atendimento a acesso, correção, exportação, oposição e exclusão conforme obrigação aplicável.
- Avaliar transferência e residência internacional de dados.
- Definir tratamento de dados potencialmente sensíveis inseridos em Conteúdo.
- Definir contato e processo de incidente de privacidade.

### 22.2 Minimização

- Familiaridade digital é opcional e usada somente para adaptar a experiência.
- Objetivo e público não devem exigir categoria sensível.
- Tutor recebe somente etapa e objetos necessários.
- Analytics não recebe corpo de Conteúdo nem texto livre.
- Relatório guarda observações necessárias, não cópia irrestrita da resposta do Canal.
- Conta desconectada perde credencial utilizável e mantém apenas histórico justificado.

### 22.3 Retenção inicial a decidir

Categorias separadas:

- perfil e Projeto ativos;
- Conteúdo e versões;
- arquivos próprios e temporários;
- tokens revogados;
- Publicações e métricas;
- interações e recomendações do Tutor;
- auditoria;
- logs e rastros;
- cópias;
- pedidos de suporte e exportações.

Nenhum prazo deve ser inventado sem finalidade, necessidade operacional e validação legal.

## 23. Criptografia e gestão de chaves

- Transporte criptografado entre clientes, serviços e provedores.
- Banco, objeto, cópia e fila criptografados em repouso.
- Chaves gerenciadas fora do código e separadas por ambiente.
- Chave de token externo separada de chave de armazenamento quando viável.
- Rotação com versão de chave e procedimento de recriptografia quando necessário.
- Acesso a chave auditado e restrito a identidade de serviço.
- Material de chave nunca exportado para estação de desenvolvimento sem procedimento aprovado.
- Falha de descriptografia gera incidente observável e não exposição de valor bruto.

## 24. Infraestrutura e rede

- Banco, fila, cofre e armazenamento sem exposição pública desnecessária.
- Entrada pública somente por borda aprovada e webhooks necessários.
- Saída externa restrita por destino ou mecanismo equivalente quando viável.
- Identidade de serviço por processo e ambiente.
- Conta de implantação separada da conta de execução.
- Configuração e infraestrutura versionadas e revisadas.
- Imagens ou ambientes mínimos, atualizados e sem ferramenta desnecessária.
- Horário confiável para token, auditoria e webhook.
- Ambientes isolados por conta, projeto ou fronteira equivalente.

## 25. Segurança da entrega e dependências

- Mudança revisada por outra pessoa quando equipe permitir.
- Verificação automática de testes, segredo, dependência e configuração.
- Dependências diretas reduzidas, fixadas e atualizadas por processo controlado.
- Inventário de componentes e licenças.
- Artefato assinado ou com integridade verificável.
- Credenciais de entrega curtas e sem acesso amplo permanente.
- Produção promovida a partir de artefato verificado, não construída manualmente.
- Migração revisada quanto a perda, exposição e reversibilidade.
- Correção crítica possui prioridade e procedimento de exceção auditável.
- Função nova de alto risco pode ser ativada gradualmente e desligada sem nova implantação.

## 26. Cópias, continuidade e recuperação

- Cópia automática do banco com criptografia e retenção aprovada.
- Proteção equivalente para objetos próprios necessários.
- Segredos não dependem apenas da cópia do banco.
- Restauração testada em ambiente isolado antes do piloto.
- Teste confirma integridade, autorização e ausência de exposição.
- Procedimento cobre perda de banco, objeto, segredo, Integração e região.
- Objetivos de recuperação e perda aceitável devem ser aprovados antes da produção.
- Arquivo grande externo não é coberto pela cópia da plataforma; isso é explicado ao Usuário.

## 27. Monitoramento e detecção

Alertas mínimos:

- entradas ou recuperações anômalas;
- falhas repetidas de autorização entre Projetos;
- volume atípico de conexão, Publicação, exportação ou exclusão;
- uso anômalo de token externo;
- assinatura inválida ou repetição de webhook;
- Publicação incerta ou fila parada;
- acesso administrativo ou ao cofre fora do padrão;
- detecção de segredo ou dado proibido em telemetria;
- falha de cópia, limpeza temporária ou restauração;
- custo ou volume anormal de IA e armazenamento.

Alerta deve ter responsável, severidade, contexto seguro, ação inicial e regra de encerramento. Alerta sem resposta definida não é controle completo.

## 28. Resposta a incidentes

### 28.1 Processo mínimo

1. Detectar e registrar o incidente por canal restrito.
2. Classificar impacto, dados, Usuários, Canais e ambientes afetados.
3. Conter: revogar sessão ou token, interromper Integração, bloquear Publicação ou isolar componente.
4. Preservar evidência com acesso controlado.
5. Corrigir causa e restaurar de fonte confiável.
6. Validar segurança antes de reativar.
7. Comunicar internamente e a pessoas ou autoridades quando aplicável.
8. Registrar aprendizado, responsável e prazo de prevenção.

### 28.2 Ações preparadas

- revogar todas as sessões de um Usuário;
- revogar uma Conta Conectada;
- rotacionar segredo de Integração;
- suspender um adaptador ou Publicações;
- invalidar URLs temporárias;
- desligar Tutor assistido por IA e voltar a regras;
- bloquear versão vulnerável de Web ou Mobile quando possível;
- restaurar banco e objetos próprios;
- localizar eventos por correlação sem buscar Conteúdo bruto.

## 29. Acesso operacional e suporte

- Suporte vê estado traduzido e códigos sanitizados, não token.
- Acesso a Conteúdo não é padrão para diagnóstico.
- Acesso excepcional exige motivo, escopo, prazo, aprovação proporcional e auditoria.
- Produção não é consultada por credencial compartilhada.
- Exportação local de dado produtivo é proibida sem processo específico.
- Sessão administrativa expira e requer autenticação reforçada.
- Saída ou mudança de função revoga acessos prontamente.

## 30. Verificação de segurança

### Antes do desenvolvimento real

- modelagem de ameaça do Canal, identidade, armazenamento e Tutor;
- definição de dados, retenção e fundamento aplicável;
- contratos de segurança dos adaptadores;
- arquitetura de segredos e ambientes;
- critérios de log e auditoria.

### Antes do piloto

- teste de autorização entre Projetos;
- teste de sessão Web e Mobile;
- teste do retorno de autorização externa;
- teste de webhook forjado, antigo e repetido;
- teste de idempotência e timeout de Publicação;
- teste de arquivo malicioso, excessivo e removido;
- teste de injeção no Tutor e de saída inválida;
- varredura de dependência, segredo e configuração;
- restauração de cópia;
- exercício de resposta a vazamento de token;
- revisão independente dos fluxos de maior risco.

### Continuamente

- atualizar ameaças e dependências;
- revisar acessos e segredos;
- acompanhar alertas e custos;
- verificar limpeza de temporários;
- testar restauração e rotação;
- corrigir achados conforme severidade aprovada.

## 31. Critérios mínimos para produção

- Nenhum risco crítico conhecido sem tratamento aprovado.
- Provedor de identidade e recuperação testados.
- Autorização por Projeto coberta por testes positivos e negativos.
- Tokens externos protegidos, rotacionáveis e redigidos.
- Publicação idempotente, auditada e reconciliável.
- Webhooks verificados e deduplicados.
- Arquivos privados sem acesso público e temporários com limpeza comprovada.
- Tutor incapaz de executar ação sensível sem confirmação e autorização do backend.
- Logs livres de campos proibidos em testes.
- Cópia e restauração comprovadas.
- Alertas críticos com responsáveis.
- Resposta a incidente exercitada.
- Política de privacidade, retenção e exclusão aprovada.

## 32. Limites de segurança do MVP

- Um papel de Usuário reduz a matriz de autorização; colaboração fica fora.
- Um Canal limita o número de tokens e webhooks; multicanal fica desativado.
- Publicação imediata evita Agendamento e seus riscos temporais.
- Tutor determinístico pode operar sem IA; ferramenta autônoma fica proibida.
- Arquivo grande fica em serviço conectado; edição e transcodificação ficam fora.
- Uma região reduz complexidade; recuperação regional é processual, não ativa.
- Operação offline completa fica fora.
- Proteção contra dispositivo comprometido é limitada; servidor permanece autoritativo.

## 33. Decisões abertas de segurança

| ID | Decisão | Evidência ou responsável necessário |
|---|---|---|
| SEG-01 | Provedor de identidade, método de entrada e autenticação reforçada | Pesquisa de UX, análise de risco e capacidade operacional |
| SEG-02 | Provedor de nuvem, região e residência dos dados | Requisitos legais, público e orçamento |
| SEG-03 | Cofre, gestão de chaves e forma de proteção dos tokens | Prova de arquitetura e modelo operacional |
| SEG-04 | Permissões exatas do primeiro Canal | Documentação e prova oficial da Integração escolhida |
| SEG-05 | Serviço de armazenamento e escopo de acesso | Hábitos do segmento e prova de seleção/leitura |
| SEG-06 | Uso de IA, provedor, região, retenção e treinamento | Avaliação de privacidade, segurança, qualidade e contrato |
| SEG-07 | Prazos de retenção por categoria | Finalidade, obrigação, suporte e aprovação jurídica |
| SEG-08 | Objetivos de recuperação e disponibilidade | Impacto do piloto e orçamento |
| SEG-09 | Solução de auditoria imutável e observabilidade | Provedor, custo e requisitos de investigação |
| SEG-10 | Processo e responsável por incidentes e privacidade | Governança organizacional antes da produção |
| SEG-11 | Limites máximos de arquivo, IA e chamadas | Canal, formato, custo e teste de abuso |
| SEG-12 | Necessidade de teste independente especializado | Classificação final de risco e escopo do piloto |

## 34. Riscos de segurança abertos

| ID | Risco | Severidade inicial | Tratamento requerido |
|---|---|---:|---|
| RS-01 | Escopo de token externo maior que o necessário | Alta | Escolher permissão mínima e provar revogação |
| RS-02 | Publicação duplicada ou não autorizada | Crítica | Confirmação, idempotência, reconciliação e auditoria |
| RS-03 | Falha de isolamento entre Projetos | Crítica | Autorização central, escopo em consulta e testes negativos |
| RS-04 | Segredo ou Conteúdo em logs | Alta | Lista permitida, redação, teste e alerta |
| RS-05 | Injeção de instrução no Tutor | Alta | Separação, ferramentas restritas, validação e confirmação |
| RS-06 | Provedor de IA usar ou reter dado indevidamente | Alta | Não habilitar antes de avaliação e contrato |
| RS-07 | Arquivo externo removido, alterado ou malicioso | Alta | Fixar versão quando possível, validar e falhar com segurança |
| RS-08 | Cópia temporária de vídeo não ser apagada | Alta | TTL, isolamento, limpeza e monitoramento |
| RS-09 | Webhook forjado ou repetido alterar estado | Alta | Assinatura, tempo, deduplicação e processamento idempotente |
| RS-10 | Recuperação de conta permitir tomada | Alta | Provedor maduro, limitação, alerta e teste |
| RS-11 | Operador acessar dados sem necessidade | Alta | Menor privilégio, acesso temporário e auditoria |
| RS-12 | Política de retenção e base aplicável indefinidas | Alta | Revisão jurídica e de privacidade antes do piloto |
| RS-13 | Dois clientes ampliarem superfície sem capacidade de teste | Moderada/alta | Definir cliente primário e critérios equivalentes |
| RS-14 | Dependência vulnerável comprometer entrega | Alta | Inventário, verificação, atualização e artefato controlado |
| RS-15 | Custo abusivo causar indisponibilidade | Moderada/alta | Limite por Projeto, orçamento e degradação |

## 35. Revisão

Este documento deve ser atualizado quando provedor de identidade, Canal, armazenamento, IA, nuvem ou cliente primário forem escolhidos; quando surgir nova categoria de dados; antes do primeiro piloto real; e após qualquer incidente, teste ou mudança de risco relevante.
