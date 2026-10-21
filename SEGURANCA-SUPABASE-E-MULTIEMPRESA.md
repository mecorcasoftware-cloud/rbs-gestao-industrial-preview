# Segurança criptográfica e proteção de dados — RBS Gestão Industrial

## Objetivo

Proteger os dados das empresas, usuários, orçamentos, máquinas, Ordens de Produção, manutenções, materiais e relatórios contra acesso não autorizado, inclusive por chamadas diretas à API.

Nenhuma solução consegue impedir a cópia de um `index.html` público, pois o navegador precisa receber o código da interface. A proteção deve estar no backend, no Supabase e nas regras de autorização. O frontend público nunca deve conter segredos nem confiar apenas em menus escondidos.

## Regras obrigatórias

### Supabase Auth

- Usar Supabase Auth para login, recuperação de senha, convite e encerramento de sessão.
- Exigir senha forte e permitir MFA para master e administradores.
- Aplicar expiração e renovação segura de sessão.
- Revogar sessões ao bloquear usuário ou empresa.
- Nunca armazenar senha no banco da aplicação.
- Nunca enviar senha ou token em URL.

### Chaves e segredos

- A chave `service_role` nunca pode ser enviada ao navegador, incluída no `index.html`, publicada no GitHub ou armazenada em variáveis públicas.
- O frontend pode usar somente a chave pública anon, com RLS obrigatoriamente ativo.
- Segredos devem ficar em variáveis de ambiente do servidor, Edge Functions ou ambiente seguro de deploy.
- Não deixar tokens, URLs privadas, credenciais ou chaves em commits do GitHub.
- Se uma chave for exposta, revogar e gerar outra imediatamente.

### Row Level Security

Ativar RLS em todas as tabelas de negócio e criar políticas que validem o usuário autenticado, o vínculo com a empresa e a permissão para a ação.

Toda entidade deverá conter `empresa_id` ou vínculo equivalente. Uma política nunca deve depender apenas de um filtro enviado pelo frontend.

A regra lógica mínima é:

```text
usuário autenticado
E usuário ativo
E usuário vinculado à empresa do registro
E usuário possui permissão para o módulo e ação
```

O usuário master terá funções administrativas protegidas e não deverá receber acesso global por uma regra pública genérica. A validação do master deve ocorrer em função segura no servidor.

### Isolamento entre empresas

- Cada consulta deve respeitar `empresa_id`.
- Um usuário não pode acessar registro de outra empresa alterando o ID na URL ou no payload.
- Relatórios, exportações, arquivos, gráficos e downloads devem filtrar por empresa.
- O Storage deve usar pastas e políticas vinculadas à empresa.
- Convites devem ser associados a uma empresa antes de liberar acesso.
- Troca de empresa ativa deve ser validada no servidor.

### Criptografia

- Usar TLS/HTTPS em toda comunicação.
- Usar criptografia em repouso fornecida pelo Supabase e pelo provedor de infraestrutura.
- Criptografar campos especialmente sensíveis antes de persistir, quando aplicável.
- Manter as chaves de criptografia fora do banco e fora do frontend.
- Usar rotação de chaves e controle de acesso às chaves.
- Não criptografar senhas manualmente; o Supabase Auth deve cuidar do armazenamento seguro.
- Para arquivos sensíveis, usar URLs assinadas com prazo curto, em vez de links públicos permanentes.

### API e Edge Functions

- Operações críticas devem passar por Edge Functions ou backend seguro.
- Validar autenticação e permissão no servidor antes de criar, alterar, aprovar ou finalizar qualquer registro.
- Validar novamente `empresa_id` recebido no payload.
- Aplicar rate limit em login, convites, exportações e funções administrativas.
- Usar idempotência em emissão de Ordem de Produção e aprovação para evitar duplicidade.
- Registrar falhas de autorização sem revelar informações sensíveis.

### Auditoria

Registrar na tabela `auditoria`:

- Usuário;
- Empresa;
- Ação;
- Módulo;
- Entidade e registro afetado;
- Data e hora;
- Resultado;
- IP e dispositivo quando disponíveis;
- Valores alterados, sem guardar segredos.

Auditar especialmente criação de usuários, alteração de permissões, troca de empresa, aprovação de orçamento, emissão/finalização de O.P., encerramento de manutenção, alterações de máquinas e exportações.

### Backups e recuperação

- Ativar backups automáticos do banco.
- Testar restauração periodicamente.
- Manter histórico de migrações versionadas.
- Não guardar backup aberto em repositório público.
- Criptografar arquivos de backup e limitar o acesso.
- Separar permissões de operação, desenvolvimento e administração.

### GitHub e prévia pública

A prévia pública deve conter somente dados fictícios. Não incluir:

- Dados de clientes reais;
- Usuários reais;
- E-mails ou telefones reais;
- Tokens;
- Chaves Supabase;
- URLs assinadas;
- Dados de máquinas reais;
- Exportações ou backups.

O GitHub Pages é adequado para apresentação visual, mas não para hospedar a API, o banco ou segredos.

### Monitoramento

Configurar alertas para:

- Muitas falhas de login;
- Tentativas de acesso a outra empresa;
- Aumento anormal de exportações;
- Alterações de permissão;
- Uso de função administrativa fora do padrão;
- Erros repetidos de RLS;
- Chaves ou variáveis vazadas em pipeline.

## Resultado esperado

Mesmo que alguém copie a interface pública ou tente chamar a API diretamente, não deverá conseguir consultar ou alterar dados sem autenticação, vínculo com a empresa e permissão específica. A segurança efetiva será aplicada no Supabase e no backend, não no HTML público.
