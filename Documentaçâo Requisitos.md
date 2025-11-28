#  1\. Requisitos do Sistema

## 1.1 Requisitos Funcionais

### RF01: Gerenciamento de Login

Permitir autenticação via [GOV.BR](http://GOV.BR)

Validar cadastro de usuário no sistema

Redirecionar para página inicial após login válido

Gerenciar seleção de perfil para usuários com múltiplos perfis

### RF02: Gerenciamento de Logout

Encerrar sessão do sistema

Encerrar sessão do GOV.BR

Redirecionar para página inicial

### RF03: Gerenciamento de Dados Pessoais

Exibir dados do usuário (Nome, CPF, Telefone, E-mail, UF, Município, Anexos)

Permitir alteração de campos editáveis (Telefone, E-mail)

Validar campos obrigatórios

Confirmar alterações com mensagem de sucesso

### RF04: Solicitação de Acesso

Exibir formulário de solicitação para primeiro acesso

Validar campos obrigatórios (Nome, CPF, E-mail, UF, Município, Perfil, Arquivos)

Gerenciar adição/exclusão de municípios

Gerenciar upload de arquivos com validação de formato e tamanho

Enviar solicitação para análise do administrador

### RF05: Seleção de Perfil

Permitir seleção de perfil para usuários com múltiplos perfis

Redirecionar para página inicial após seleção

### RF06: Gerenciamento de Usuários

Listar usuários com filtros e ordenação

Cadastrar novos usuários (internos e externos)

Alterar dados de usuários existentes

Visualizar dados de usuários

Ativar/Inativar usuários com justificativa

Pesquisar usuários por diversos parâmetros

Manter histórico de alterações

Analisar solicitações de acesso

Exportar relatórios (PDF, XLS)

### RF07: Solicitação de Alteração

Permitir solicitação de alteração de ente federado

Gerenciar adição/remoção de municípios

Validar e gerenciar anexos

Enviar solicitação para análise

### RF08: Análise de Solicitações

Aprovar/reprovar solicitações de acesso e alteração

Gerenciar justificativas para reprovação

Operações em lote

Envio de e-mails de notificação

---

## 1.2 Requisitos Não Funcionais 

### RNF01: Disponibilidade

O sistema deve estar disponível 24 horas por dia, 7 dias por semana

### RNF02: Performance

Tempo de resposta inferior a 3 segundos para consultas

Suporte a múltiplos usuários concorrentes

### RNF03: Segurança

Autenticação via GOV.BR

Controle de acesso baseado em perfis

Registro de logs de auditoria

Validação de dados de entrada

### RNF04: Usabilidade

Interface intuitiva e responsiva

Mensagens de erro claras e objetivas

Validação em tempo real de campos

### RNF05: Compatibilidade

Suporte aos principais navegadores web

Integração com API do IBGE para UFs e municípios

Integração com GOV.BR para autenticação

---

# 2\. Casos de Uso

## 2.1 LOGIN

### 2.1.1 Realizar Login

**Atores**: Usuário do sistema 

**Pré-condições**: Usuário possui cadastro válido no GOV.BR  

**Fluxo Principal**:

1\. Usuário acessa o endereço virtual do sistema

2\. Usuário aciona a opção "Entrar com Gov.br"

3\. Sistema redireciona para autenticação GOV.BR

4\. Usuário preenche dados de login

5\. Sistema realiza autenticação

6\. Sistema verifica conformidade com RNG-02, RNG-03, RNG-56 e RNG-57

7\. Usuário é redirecionado para página inicial

**Fluxo Alternativo**:

6a. Usuário possui múltiplos perfis: sistema direciona para seleção de perfil

6b. Usuário inativo: sistema exibe MSG-02 e nega acesso

**Pós-condições**: Usuário autenticado na página inicial

### 

### 2.1.2 Realizar Logout

**Atores**: Usuário do sistema 

**Pré-condições**: Usuário está em sessão ativa  

**Fluxo Principal**:

1\. Usuário aciona opção "Logout"

2\. Sistema realiza logout

3\. Sistema encerra sessão do GOV.BR

4\. Sistema redireciona para endereço virtual do sistema

**Pós-condições**: Sessão encerrada, usuário na página inicial

### 2.1.3 Gerenciar Dados Pessoais

**Atores**: Usuário do sistema   

**Pré-condições**: Usuário autenticado  

**Fluxo Principal**:

1\. Usuário aciona opção "Meus Dados"

2\. Sistema exibe formulário com campos:

   Nome (não editável, obrigatório)

   CPF (não editável, obrigatório)

   Telefone (editável, não obrigatório)

   E-mail (editável, obrigatório)

   UF (não editável, obrigatório)

   Município (não editável, obrigatório)

   Anexos (não editável, obrigatório, com download)

3\. Usuário altera campos editáveis

4\. Usuário aciona "Salvar"

5\. Sistema salva dados conforme RNG-17

6\. Sistema exibe MSG-11

7\. Sistema redireciona para página inicial

**Fluxo Alternativo**:

4a. Usuário aciona "Cancelar" sem alterações: sistema redireciona para página inicial

4b. Usuário aciona "Cancelar" com alterações: sistema exibe MSG-10 com opções SIM/NÃO

**Pós-condições**: Dados atualizados ou alterações descartadas

### 2.1.4 Solicitar Acesso

**Atores**: Usuário externo  

**Pré-condições**: Usuário não cadastrado no sistema  

**Fluxo Principal**:

1\. Usuário acessa sistema e autentica via GOV.BR

2\. Sistema identifica que usuário não está cadastrado (RNG-02)

3\. Sistema exibe formulário de solicitação de acesso

4\. Usuário preenche campos obrigatórios:

   Nome, CPF, E-mail, UF, Município, Perfil, Arquivos

5\. Usuário adiciona municípios e arquivos

6\. Usuário aciona "Enviar solicitação"

7\. Sistema envia solicitação para administrador

8\. Sistema exibe MSG-01

9\. Sistema redireciona para página de login

**Fluxo Alternativo**:

6a. Usuário cancela: sistema exibe MSG-03 com confirmação

6b. Arquivo com extensão inválida: sistema exibe MSG-04

6c. Arquivo excede tamanho: sistema exibe MSG-04

**Pós-condições**: Solicitação enviada para análise

---

## 2.2 PERSISTIR USUÁRIOS

### 2.2.1 Listar Usuários

**Atores**: Administrador  

**Pré-condições**: Usuário autenticado com perfil Administrador  

**Fluxo Principal**:

1\. Usuário acessa opção "Usuários"

2\. Sistema apresenta lista com campos:

   Nome, Perfil, Localidade, Status, Último acesso

3\. Sistema ordena por nome, priorizando aguardando aprovação (RNG-10)

**Pós-condições**: Lista de usuários exibida

### 2.2.2 Cadastrar Usuário

**Atores**: Administrador  

**Pré-condições**: Usuário autenticado com perfil Administrador  

**Fluxo Principal**:

1\. Usuário seleciona "Cadastrar"

2\. Sistema exibe opção de tipo de usuário (Interno/Externo)

3\. Usuário seleciona tipo e preenche campos obrigatórios

4\. Sistema valida CPF (RNG-13, RNG-14) e E-mail (RNG-15, RNG-18)

5\. Usuário aciona "Cadastrar"

6\. Sistema salva dados (RNG-16, RNG-17)

7\. Sistema exibe MSG-05

8\. Sistema envia E-mail-01

9\. Sistema retorna para lista de usuários

**Fluxo Alternativo**:

4a. CPF inválido: sistema exibe MSG-08

4b. CPF já cadastrado: sistema exibe MSG-06

4c. E-mail inválido: sistema exibe MSG-09

4d. E-mail já cadastrado: sistema exibe MSG-07

**Pós-condições**: Usuário cadastrado no sistema

### 2.2.3 Alterar Usuário

**Atores**: Administrador  

**Pré-condições**: Usuário autenticado com perfil Administrador  

**Fluxo Principal**:

1\. Usuário seleciona "Alterar" em usuário da lista

2\. Sistema exibe formulário com campos editáveis

3\. Usuário altera dados

4\. Usuário aciona "Salvar"

5\. Sistema salva dados (RNG-17)

6\. Sistema exibe MSG-11

7\. Sistema envia E-mail-02

8\. Sistema retorna para lista de usuários

**Pós-condições**: Dados do usuário atualizados

### 2.2.4 Ativar/Inativar Usuário

**Atores**: Administrador  

**Pré-condições**: Usuário autenticado com perfil Administrador  

**Fluxo Principal**:

1\. Usuário seleciona "Ativar" em usuário inativo

2\. Sistema exibe MSG-12 e campo "Justificativa"

3\. Usuário preenche justificativa

4\. Usuário aciona "Ativar"

5\. Sistema ativa usuário (RNG-16, RNG-17)

6\. Sistema exibe MSG-14

7\. Sistema envia E-mail-05

8\. Sistema atualiza lista de usuários

**Pós-condições**: Usuário ativado no sistema

### 2.2.5 Analisar Solicitação

**Atores**: Administrador  

**Pré-condições**: Solicitação pendente de análise  

**Fluxo Principal**:

1\. Usuário seleciona "Analisar Solicitação"

2\. Sistema exibe dados da solicitação (CPF, Nome, Telefone, E-mail, Municípios, Anexos, Perfil)

3\. Usuário aprova/reprova municípios

4\. Para reprova, usuário preenche justificativa

5\. Usuário aciona "Salvar"

6\. Sistema atualiza status dos municípios

7\. Sistema exibe MSG-05 (aprovado) ou MSG-14 (reprovado)

8\. Sistema envia E-mail-03 (aprovado) ou E-mail-04 (reprovado)

9\. Sistema retorna para lista de usuários

**Pós-condições**: Solicitação analisada e usuário notificado

---

# 3\. Regras de Negócio

## 3.1 Regras de Acesso e Autenticação

### RNG-01: Controle de Acesso por Perfil

O sistema apresenta menus e ações de acordo com o perfil do usuário logado

Administrador: Acesso completo a usuários

Gestor: Visualização e análise

Observador/Consultor: Apenas visualização

Cadastrador: Acesso apenas ao seu município

### RNG-02: Verificação de Cadastro

O sistema deve verificar se o CPF está cadastrado na base de dados do sistema

### RNG-03: Verificação de Perfil Ativo

O sistema deve verificar se o usuário possui perfil ativo

### 

### RNG-04: Integração com GOV.BR

O sistema deverá buscar e exibir as informações consultadas no GOV.br

---

## 3.2 Regras de Dados e Validação

### RNG-05: Consulta de UFs e Municípios

O sistema deverá consultar e exibir as UFs e Municípios utilizando a integração com API do IBGE

### 

### RNG-06: Filtro de Municípios por UF

O sistema deverá filtrar os municípios de acordo com a UF selecionada

### RNG-07: Formato de Arquivos

O sistema deve permitir o cadastro dos seguintes tipos de anexos: JPEG, JPG, PNG, PDF, DOC e DOCX

### RNG-08: Tamanho de Arquivos

O sistema deve permitir o upload de arquivos de até 100MB

### RNG-13: Validação de CPF

O sistema deve consultar se o CPF é válido na API e preencher o campo Nome

### RNG-14: CPF Único

O sistema não deve permitir o cadastro de usuários com o mesmo CPF

### RNG-15: E-mail Único para Internos

O sistema não deve permitir o cadastro de usuários internos com o mesmo e-mail

### RNG-18: Validação de E-mail

O sistema deve verificar se o e-mail informado é válido

---

## 3.3 Regras de Interface e Comportamento

### RNG-09: Ações por Situação

Analisar Solicitação: Para status "Aguardando aprovação" e "Aguardando análise"

Alterar, Visualizar, Inativar e Histórico: Para status "Ativo"

Ativar e Histórico: Para status "Inativo"

Histórico: Para status "Reprovado"

### RNG-10: Ordenação de Registros

O sistema deverá exibir, por padrão, os registros em ordem alfabética, utilizando a primeira coluna como critério de organização, destacando prioritariamente os registros que estão aguardando aprovação

### RNG-11: Paginação

O sistema deverá apresentar, por padrão, um contabilizador com o total de registros e um paginador com no máximo 20 registros por página

### 

### RNG-16: Ativação de Registro

O sistema deve atribuir o status ATIVO ao registro

### RNG-17: Armazenamento de Logs

O sistema armazena logs para: Cadastrar, Alterar, Ativar, Inativar, Aprovar Solicitação, Reprovar Solicitação, Alterar meus dados, Excluir, Enviar Solicitação

### RNG-19: Campos Obrigatórios

O sistema habilita opções (Cadastrar, Salvar, Ativar, etc.) apenas quando todos os campos obrigatórios estão preenchidos

### RNG-20: Inativação de Registro

O sistema deve atribuir o status INATIVO ao registro

### RNG-22: Ações na Análise de Solicitação

Aprovar e Reprovar: Para status "Aguardando análise"

Reprovar: Para status "Aprovado"

Aprovar e Visualizar justificativa: Para status "Reprovado"

### RNG-23: Justificativa para Reprovação

Quando o usuário aciona a opção Reprovar, o sistema habilita o campo "Justificativa"

### RNG-24: Pesquisa com Parâmetros

O sistema só habilita a opção Pesquisar após o preenchimento de pelo menos um dos parâmetros

### RNG-25: Exibição de Registros Ativos

O sistema deverá exibir, por padrão, apenas os registros ativos

### RNG-26: Ordem do Histórico

O sistema deverá exibir os registros de histórico em ordem decrescente, por data/hora da ação

---

# 4\. Mensagens do Sistema

## 4.1 Mensagens de Interface (MSG)

**MSG-01**: "Sua solicitação foi enviada com sucesso e está em processo de análise. Assim que a avaliação for concluída, entraremos em contato. Durante esse período, o acesso ao sistema não estará disponível."

**MSG-02**: "Acesso negado\! Você não possui mais acesso ao sistema. Para mais esclarecimentos, entre em contato com @cultura.gov.br."

**MSG-03**: "Deseja realmente cancelar? Para obter acesso ao sistema, é necessário preencher o formulário e enviá-lo para análise. Sem essa etapa, o cadastro não será processado."

**MSG-04**: "Os arquivos devem ser no formato JPEG, JPG, PNG, PDF, DOC ou DOCX e ter no máximo 100MB."

**MSG-05**: "Usuário cadastrado com sucesso\!"

**MSG-06**: "Já existe um usuário no sistema com o mesmo CPF informado."

**MSG-07**: "Já existe um usuário no sistema com o mesmo e-mail informado."

**MSG-08**: "CPF inválido."

**MSG-09**: "E-mail inválido."

**MSG-10**: "As alterações realizadas não serão salvas. Deseja realmente cancelar?"

**MSG-11**: "Alteração realizada com sucesso."

**MSG-12**: "Deseja realmente ativar? Se sim, justifique."

**MSG-13**: "Deseja realmente inativar? Se sim, justifique."

**MSG-14**: "Operação realizada com sucesso\!"

**MSG-15**: "Download realizado com sucesso\!"

**MSG-16**: "Deseja realmente excluir o arquivo?"

**MSG-17**: "Sua solicitação foi enviada com sucesso e está em processo de análise. Assim que a avaliação for concluída, entraremos em contato."

**MSG-18**: "Atenção\! Ao confirmar o cadastro do novo cadastrador, o cadastrador anterior será automaticamente inativado. Deseja continuar?"

**MSG-19**: "O CPF informado pertence ao cadastrador ativo neste ente federado. Por favor, verifique os dados e tente novamente."

---

## 4.2 Mensagens de E-mail

### E-mail-01: Cadastro de usuário

Assunto:  Cadastro de usuário

Corpo: Notificação de cadastro realizado com sucesso com link de acesso

### E-mail-02: Alteração de dados cadastrais

Assunto:  Alteração dos dados cadastrados do usuário

Corpo: Notificação de alteração nos dados cadastrais

### E-mail-03: Aprovação de solicitação de acesso

Assunto:  Solicitação de acesso Análise concluída

Corpo: Notificação de aprovação de cadastro

### E-mail-04: Reprovação de solicitação de acesso

Assunto:  Solicitação de acesso Análise concluída

Corpo: Notificação de reprovação com justificativa

### E-mail-05: Ativação de cadastro

Assunto:  Cadastro ativo

Corpo: Notificação de ativação de cadastro com justificativa

### E-mail-06: Inativação de cadastro

Assunto:  Cadastro inativo

Corpo: Notificação de inativação de cadastro com justificativa

---

# 5\. Listas de Valores (LOV)

## 5.1 Domínios Fixos

### LOV-01: Status do Usuário

* Ativo  
* Inativo  
* Aguardando aprovação  
* Aguardando análise  
* Reprovado

### 

### LOV-03: Perfis de Usuários Internos

* Administrador  
* Gestor MinC  
* Observador/Consultor

### LOV-04: Perfis de Usuários Externos

* Cadastrador  
* Gestor de Cultura  
* Prefeito

### LOV-09: Tipo de Usuário

* Interno  
* Externo

---

# 6\. Estrutura de Dados

## 6.1 Campos do Sistema

### Formulário de Meus Dados

**Nome**: Texto (100 chars, não editável, obrigatório)

**CPF**: Texto numérico (11 chars, máscara 000.000.000-00, não editável, obrigatório)

**Telefone**: Texto numérico (11 chars, máscara (xx) xxxxx-xxxx, editável, não obrigatório)

**E-mail**: Texto (50 chars, editável, obrigatório)

**UF**: Seleção múltipla (50 chars, não editável, obrigatório)

**Município**: Seleção múltipla (100 chars, não editável, obrigatório)

**Anexos**: Label/Arquivo (100 chars, não editável, obrigatório, ação: Download)

### Formulário de Solicitação de Acesso

**Nome**: Texto (100 chars, não editável, obrigatório, RNG-04)

**CPF**: Texto numérico (11 chars, máscara 000.000.000-00, não editável, obrigatório, RNG-04)

**Telefone**: Texto numérico (11 chars, máscara (xx) xxxxx-xxxx, editável, não obrigatório, RNG-04)

**E-mail**: Texto (50 chars, editável, obrigatório, RNG-04, RNG-18)

**UF**: Seleção múltipla (50 chars, editável, obrigatório, RNG-05)

**Município**: Seleção múltipla (100 chars, editável, obrigatório, RNG-05, RNG-06)

**Perfil**: Seleção múltipla (50 chars, editável, obrigatório, LOV-04, RNG-62)

**Envio de arquivos**: Texto/Arquivo (100 chars, editável, obrigatório, RNG-07, RNG-08)

