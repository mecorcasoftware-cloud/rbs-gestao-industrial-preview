# RBS Gestão Industrial
## Diretrizes completas do sistema, acessos e fluxos

**Documento de referência para implementação do aplicativo**

---

## 1. Objetivo do sistema

O RBS Gestão Industrial será uma plataforma web multiempresa para integrar orçamentos de peças usinadas, produção, máquinas, manutenção, materiais e relatórios gerenciais.

O sistema deverá permitir que cada empresa trabalhe em uma base isolada, com usuários, máquinas, pedidos, orçamentos, roteiros, Ordens de Produção e Ordens de Serviço próprios. Nenhum usuário poderá consultar ou alterar dados de outra empresa.

A capacidade inicial esperada é de aproximadamente 50 usuários utilizando o sistema simultaneamente, com possibilidade de expansão conforme o crescimento da plataforma.

A aplicação deverá utilizar Supabase para autenticação, banco de dados PostgreSQL, Row Level Security, armazenamento de arquivos e funções seguras no backend.

---

## 2. Identidade e padrão visual

O nome oficial do produto será **RBS Gestão Industrial**. Não utilizar nomes inventados como “Fábrica 1”, “Fábrica 2” ou outras unidades que não tenham sido cadastradas pelo usuário.

O logo oficial RBS Gestão Industrial deverá aparecer na tela inicial e no cabeçalho. O visual deverá utilizar a paleta extraída do logo:

- Azul-marinho para fundos e áreas estruturais;
- Azul médio para botões primários e estados ativos;
- Cinza metálico para elementos secundários;
- Branco para textos principais;
- Verde para indicadores positivos e disponibilidade;
- Amarelo/âmbar para alertas e atenção;
- Vermelho somente para falhas críticas, atrasos e paradas graves.

As abas inativas devem utilizar a mesma aparência. Uma aba só deverá mudar para azul quando estiver realmente selecionada. A aba Produção não deve permanecer destacada quando o usuário estiver em Orçamentos, Manutenção ou outra área.

No celular, o menu deverá manter visíveis as seguintes abas principais:

1. Visão geral;
2. Orçamentos;
3. Manutenção;
4. Produção;
5. Relatórios;
6. Máquinas;
7. Materiais e peças.

---

## 3. Estrutura do menu

### Visão geral

A Visão geral deve ser somente um resumo dos principais indicadores de cada módulo. Ela não deve concentrar todos os detalhes operacionais.

Indicadores resumidos:

- Máquinas monitoradas;
- Máquinas operando;
- Máquinas paradas;
- Ordens de manutenção abertas;
- Ordens críticas e atrasadas;
- Horas paradas;
- Orçamentos em aberto;
- Orçamentos aguardando aprovação;
- Ordens de Produção abertas;
- O.P. aguardando aprovação;
- Alertas de gargalo e sobrecarga.

Cada cartão da Visão geral deverá possuir um atalho para abrir o módulo detalhado correspondente.

### Orçamentos

Área destinada ao MecOrça e aos orçamentos de peças usinadas. Deverá preservar o fluxo de orçamento já existente e permitir:

- Criar orçamento;
- Cadastrar cliente;
- Cadastrar peça ou conjunto;
- Informar material, dimensões, quantidade e operações;
- Calcular custos e preço;
- Enviar para aprovação;
- Aprovar ou reprovar;
- Transformar orçamento aprovado em pedido;
- Encaminhar pedido aprovado para o roteiro de produção;
- Acompanhar orçamento em análise, aprovado, recusado e convertido.

O botão “Orçar peça usinada” deve permanecer disponível para iniciar uma nova cotação.

### Manutenção

Área para solicitações e Ordens de Serviço de manutenção. Todos os usuários autorizados poderão abrir uma solicitação, mas as ações posteriores dependerão da permissão recebida.

O módulo deverá exibir:

- Máquinas paradas;
- Máquinas em manutenção;
- Máquinas aguardando peça ou recurso;
- Solicitações abertas;
- Ordens críticas;
- Tempo de parada;
- Causa da parada;
- Responsável;
- Prazo previsto;
- Histórico de manutenções realizadas.

### Produção

A aba Produção será o local principal das Ordens de Produção. A Visão geral mostrará apenas um resumo; os detalhes deverão ficar nesta aba.

A aba Produção deverá conter:

- Ordens em aberto;
- Gerar O.P.;
- ERP;
- Lotes de produção;
- Produção aguardando aprovação de pedido;
- Nova Ordem de Produção;
- Ordens na fila;
- Ordens liberadas;
- Ordens em produção;
- Ordens concluídas;
- Roteiros de produção;
- Capacidade por máquina;
- Horas disponíveis, alocadas, produzidas e ociosas;
- Sobrecarga e horas negativas;
- O.P. afetadas por parada de máquina.

### Relatórios

Deverá permitir relatórios de:

- Orçamentos realizados;
- Orçamentos aprovados;
- Ordens de Produção;
- Produção planejada x realizada;
- Manutenções realizadas;
- Horas paradas;
- Causas de parada;
- Disponibilidade de máquinas;
- Ocupação e ociosidade;
- Usuários e auditoria.

### Máquinas

Cadastro e acompanhamento dos ativos industriais. Cada máquina deverá possuir histórico, capacidade, calendário e vínculo com produção e manutenção.

### Materiais e peças

Cadastro de materiais, peças, estoque, reserva, compras e necessidades relacionadas ao MRP e às Ordens de Produção.

---

## 4. Hierarquia de usuários

### Usuário master da plataforma

O usuário master possui visão global do sistema e será responsável por liberar e gerenciar as empresas e os administradores.

Permissões do master:

- Cadastrar empresa;
- Aprovar solicitação de nova empresa;
- Bloquear ou reativar empresa;
- Liberar administrador da empresa;
- Revogar acesso de administrador;
- Visualizar usuários e permissões globais;
- Configurar módulos disponíveis por empresa;
- Auditar ações críticas;
- Gerenciar configurações globais da plataforma.

O master não deve ser limitado ao contexto de uma única empresa.

### Administrador da empresa

O administrador é liberado pelo master e fica vinculado a uma empresa específica.

Permissões do administrador:

- Cadastrar usuários da própria empresa;
- Convidar usuários;
- Bloquear ou reativar usuários da própria empresa;
- Definir perfil e permissões;
- Definir quem pode emitir O.P.;
- Definir quem pode criar ou editar roteiros;
- Definir quem pode editar e finalizar manutenção;
- Definir quem pode aprovar orçamento, roteiro e O.P.;
- Visualizar relatórios da própria empresa;
- Nunca acessar dados de outra empresa.

### Usuários operacionais

A empresa poderá criar usuários com permissões diferentes por módulo. Um usuário pode ser editor em Produção e apenas visualizador em Manutenção, por exemplo.

Perfis mínimos:

| Perfil | Permissões gerais |
|---|---|
| Visualizador | Consulta, filtros e gráficos; não cria nem altera registros. |
| Editor | Cria e altera registros dentro dos módulos liberados. |
| Executor | Executa etapas e pode finalizar tarefas autorizadas. |
| Aprovador | Aprova pedidos, roteiros ou ordens liberadas. |
| Administrador | Gerencia usuários e permissões da própria empresa. |
| Master | Gerencia a plataforma inteira. |

---

## 5. Matriz de permissões

As permissões devem ser controladas por módulo e por ação, e não somente pela existência ou ausência de uma tela.

Ações possíveis:

- Visualizar;
- Criar;
- Abrir solicitação;
- Editar;
- Alterar status;
- Aprovar;
- Emitir ordem;
- Executar etapa;
- Finalizar;
- Reabrir;
- Cancelar;
- Exportar;
- Gerenciar cadastros;
- Gerenciar usuários;
- Gerenciar permissões.

Exemplos obrigatórios:

### Ordem de Produção

- Usuário 1 como Editor/Executor: pode abrir, editar, executar e finalizar a O.P. conforme o fluxo autorizado.
- Usuário 2 como Visualizador: pode consultar O.P., status, progresso, prazo e máquina, mas não pode editar nem finalizar.
- Usuário 3 como Roteirizador: pode montar e alterar o roteiro, mas não pode emitir a O.P. final.
- Usuário 4 como Aprovador: pode aprovar a emissão da O.P., sem alterar dados técnicos.

### Manutenção

- Todos os usuários autorizados podem abrir uma solicitação;
- Somente Editor pode alterar dados da solicitação;
- Somente Executor pode executar e registrar a manutenção;
- Somente usuário liberado pode finalizar a Ordem de Serviço;
- Somente Administrador ou usuário autorizado pode alterar uma ordem finalizada;
- Visualizador pode acompanhar status, tempo parado e histórico.

---

## 6. Isolamento entre empresas

Todas as tabelas de negócio deverão possuir `empresa_id` ou vínculo equivalente com a empresa/base.

O isolamento deverá ser aplicado no banco Supabase com Row Level Security e também no backend. Não será suficiente esconder menus na interface.

O sistema deverá impedir que o usuário veja outra empresa por:

- Alteração de URL;
- Alteração de ID no formulário;
- Alteração de filtros;
- Consulta direta à API;
- Exportação;
- Acesso a arquivos;
- Chamada direta de função.

Estrutura mínima de vínculo:

- `auth.users`: identidade de login do Supabase;
- `empresas`: empresas cadastradas;
- `usuarios_empresa`: vínculo entre usuário e empresa;
- `perfis`: perfil ou função;
- `permissoes`: permissões por módulo e ação;
- `usuario_permissoes`: permissões específicas do usuário.

Toda consulta deverá validar o usuário autenticado, a empresa ativa e a permissão necessária.

---

## 7. Fluxo do pedido até a Ordem de Produção

A Ordem de Produção não deve ser criada imediatamente após a aprovação do orçamento. O fluxo obrigatório será:

1. Usuário cria o orçamento de peça usinada;
2. Orçamento é enviado para aprovação;
3. Pedido é aprovado;
4. Sistema abre a etapa de Roteiro de Produção;
5. Usuário autorizado define operações e sequência;
6. Usuário seleciona a máquina de cada operação;
7. Sistema aplica os tempos padrão cadastrados;
8. Sistema calcula duração estimada, capacidade e data prevista;
9. Sistema identifica máquinas sobrecarregadas ou ociosas;
10. Usuário autorizado revisa o roteiro;
11. Aprovador libera o roteiro;
12. Usuário com permissão emite a Ordem de Produção;
13. Produção realiza apontamentos;
14. Sistema atualiza o progresso e a capacidade;
15. Usuário autorizado finaliza a O.P.

---

## 8. Roteiro de produção

O roteiro deverá aceitar sequências como:

- Serra fita → Torno CNC 001;
- Corte laser → Dobra → Pintura → Montagem final;
- Corte → Dobra → Solda → Pintura.

Cada etapa deverá possuir:

- Sequência;
- Operação;
- Máquina ou recurso;
- Tempo de setup;
- Tempo padrão;
- Quantidade ou lote;
- Responsável;
- Status;
- Observações técnicas;
- Dependência de etapa anterior.

Exemplos de operações:

- Corte;
- Serra fita;
- Corte laser;
- Torno CNC;
- Fresa CNC;
- Dobra;
- Solda;
- Pintura;
- Tratamento térmico;
- Montagem;
- Inspeção;
- Embalagem.

Os tempos padrão devem ser cadastrados por operação, máquina, produto ou família de produto. O sistema deverá calcular o tempo total do roteiro e utilizar esse resultado no planejamento.

---

## 9. Capacidade de produção

Com base no roteiro, tempos padrão e calendário da máquina, o sistema deverá calcular:

- Horas disponíveis;
- Horas alocadas;
- Horas reservadas;
- Horas em produção;
- Horas ociosas;
- Horas positivas disponíveis;
- Horas negativas de sobrecarga;
- Ocupação percentual;
- Gargalos;
- Previsão de término;
- O.P. afetadas por excesso de carga.

Cada máquina deverá possuir calendário de trabalho, turnos, paradas planejadas, capacidade diária e disponibilidade.

Os gráficos estilo Power BI deverão permitir selecionar:

- O.P. abertas;
- O.P. pendentes;
- Máquinas paradas;
- Orçamentos em aberto;
- Todas as séries;
- Empresa;
- Período;
- Máquina;
- Operação;
- Status.

Quando “Todas” estiver selecionado, cada indicador deverá aparecer com uma cor diferente dentro da paleta RBS.

---

## 10. Integração entre máquinas, produção e manutenção

As máquinas cadastradas deverão ser usadas simultaneamente no planejamento da produção e no controle de manutenção.

Ao registrar uma parada, o sistema deverá:

- Registrar data e hora de início;
- Registrar data e hora do retorno;
- Calcular duração total;
- Informar causa;
- Identificar parada planejada ou não planejada;
- Vincular solicitação ou Ordem de Serviço;
- Recalcular capacidade da máquina;
- Recalcular horas positivas e negativas;
- Recalcular previsão das O.P.;
- Identificar O.P. afetadas;
- Atualizar o gráfico de disponibilidade;
- Mostrar solicitações em aberto.

Por máquina, mostrar:

- Horas disponíveis;
- Horas alocadas;
- Horas em produção;
- Horas paradas;
- Horas ociosas;
- Horas positivas;
- Horas negativas;
- Ocupação;
- O.P. afetadas;
- Solicitações em aberto;
- Histórico de causas.

---

## 11. Modelo de dados sugerido no Supabase

Tabelas principais:

- `empresas`;
- `usuarios_empresa`;
- `perfis`;
- `permissoes`;
- `usuario_permissoes`;
- `modulos`;
- `clientes`;
- `orcamentos`;
- `orcamento_itens`;
- `pedidos`;
- `maquinas`;
- `operacoes`;
- `tempos_padrao`;
- `roteiros`;
- `roteiro_etapas`;
- `ordens_producao`;
- `ordem_producao_etapas`;
- `lotes_producao`;
- `apontamentos_producao`;
- `calendario_maquinas`;
- `solicitacoes_manutencao`;
- `ordens_servico`;
- `paradas_maquina`;
- `materiais`;
- `estoque_movimentos`;
- `anexos`;
- `auditoria`.

Campos comuns recomendados:

- `id` UUID;
- `empresa_id` UUID quando aplicável;
- `created_at`;
- `updated_at`;
- `created_by`;
- `updated_by`;
- `status`;
- `deleted_at` para exclusão lógica, quando necessário.

---

## 12. Segurança no Supabase

Usar Supabase Auth para login, recuperação de senha, convite e sessão.

Ativar RLS em todas as tabelas de negócio. As políticas deverão validar:

1. Usuário autenticado;
2. Vínculo ativo com a empresa;
3. Permissão do módulo;
4. Ação solicitada;
5. Relação com o registro consultado.

Funções privilegiadas, como aprovação de empresa, criação do primeiro administrador e ações do master, deverão executar no servidor por Edge Function ou função PostgreSQL segura. Nunca expor `service_role_key` no frontend.

Arquivos e anexos deverão utilizar Storage com políticas vinculadas à empresa e ao usuário autorizado.

Toda ação de segurança importante deverá gerar registro na tabela `auditoria`, contendo usuário, empresa, ação, entidade, registro, data, IP quando disponível e alterações realizadas.

---

## 13. Critérios de aceite

A implementação será considerada correta quando:

- O usuário master conseguir aprovar administradores e empresas;
- O administrador conseguir criar usuários somente na própria empresa;
- O administrador conseguir definir permissões por módulo e ação;
- Usuários de empresas diferentes não conseguirem acessar dados uns dos outros;
- O usuário visualizador não conseguir editar ou finalizar registros;
- Um pedido aprovado passar obrigatoriamente pelo roteiro antes da O.P.;
- O roteiro permitir selecionar máquinas e operações em sequência;
- Os tempos padrão influenciarem o cálculo da duração;
- O sistema mostrar sobrecarga, ociosidade e disponibilidade por máquina;
- A parada de uma máquina recalcular a capacidade e as O.P. afetadas;
- A aba Produção concentrar os detalhes das Ordens de Produção;
- A Visão geral mostrar apenas resumos e atalhos;
- O módulo de orçamento de peças usinadas permanecer disponível;
- A paleta e o logo RBS Gestão Industrial serem aplicados de forma consistente;
- O layout funcionar em computador e celular;
- Relatórios respeitarem o filtro de empresa e as permissões do usuário.

---

## 14. Regra de implementação

A prévia visual com dados demonstrativos serve somente para validar layout e experiência. Na versão de produção, os dados deverão vir do Supabase e respeitar autenticação, RLS, permissões e isolamento por empresa.

Nenhuma informação de uma empresa deverá ser exibida como exemplo para outra empresa depois que o ambiente real estiver conectado.
