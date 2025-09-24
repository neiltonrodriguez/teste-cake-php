# Plano de Execução - Sistema de Gerenciamento de Visitas

## **Etapa 1: Configuração Inicial do Ambiente**
### 1.1 Preparação do Ambiente
- [ ] Clonar o repositório base fornecido
- [ ] Verificar se Docker e Docker Compose estão instalados
- [ ] Executar `docker-compose up` para subir os containers
- [ ] Testar conectividade com MySQL
- [ ] Verificar se o CakePHP está funcionando corretamente

### 1.2 Análise da Base de Código
- [ ] Examinar a estrutura atual do projeto
- [ ] Identificar arquivos de configuração (database, routes, etc.)
- [ ] Revisar o arquivo SQL na raiz do projeto
- [ ] Verificar dependências no composer.json

---

## **Etapa 2: Estrutura do Banco de Dados**
### 2.1 Criação das Migrations
- [ ] Criar migration para tabela `addresses`
  - Campos: id, postal_code, sublocality, street, street_number, complement, created, modified
- [ ] Criar migration para tabela `workdays`
  - Campos: id, date, visits, completed, duration, created, modified
- [ ] Criar migration para tabela `visits`
  - Campos: id, date, status, forms, products, completed, duration, address_id, created, modified
- [ ] Executar as migrations: `bin/cake migrations migrate`

### 2.2 Validação da Estrutura
- [ ] Verificar se todas as tabelas foram criadas corretamente
- [ ] Confirmar relacionamentos entre tabelas
- [ ] Testar conexões e foreign keys

---

## **Etapa 3: Models e Relacionamentos**
### 3.1 Model Address
- [ ] Criar model `Address` em `src/Model/Table/AddressesTable.php`
- [ ] Definir campos acessíveis: postal_code, sublocality, street, street_number, complement
- [ ] Implementar validações básicas
- [ ] Criar método para formatação de CEP com máscara

### 3.2 Model Workday
- [ ] Criar model `Workday` em `src/Model/Table/WorkdaysTable.php`
- [ ] Definir campos acessíveis: date
- [ ] Implementar relacionamentos com Visits
- [ ] Criar métodos para cálculos automáticos (visits, completed, duration)

### 3.3 Model Visit
- [ ] Criar model `Visit` em `src/Model/Table/VisitsTable.php`
- [ ] Definir campos acessíveis: date, status, forms, products, address
- [ ] Implementar relacionamento belongsTo com Address
- [ ] Criar validações para campos obrigatórios

### 3.4 Relacionamentos
- [ ] Configurar associação Visit belongsTo Address
- [ ] Configurar associação Workday hasMany Visits
- [ ] Testar integridade referencial

---

## **Etapa 4: Integração com APIs Externas**
### 4.1 Serviço de Consulta de CEP
- [ ] Criar classe `CepService` em `src/Service/CepService.php`
- [ ] Implementar método para API República Virtual
- [ ] Implementar método para API Via CEP
- [ ] Implementar fluxo de fallback entre APIs
- [ ] Adicionar tratamento de erros e timeouts

### 4.2 Integração no Model Address
- [ ] Implementar beforeSave para consulta automática de CEP
- [ ] Validar se CEP foi alterado
- [ ] Preservar dados já preenchidos pelo usuário
- [ ] Implementar tratamento de erro "CEP não encontrado"

---

## **Etapa 5: Lógica de Negócio - Address**
### 5.1 Validações e Regras
- [ ] Implementar validação de formato do CEP
- [ ] Criar método para aplicar máscara no CEP na consulta
- [ ] Implementar lógica de preservação de campos alterados pelo usuário
- [ ] Adicionar eventos beforeSave e afterFind

### 5.2 Testes da Funcionalidade
- [ ] Testar consulta de CEP válido
- [ ] Testar fallback entre APIs
- [ ] Testar preservação de dados do usuário
- [ ] Testar erro de CEP não encontrado

---

## **Etapa 6: Lógica de Negócio - Workdays**
### 6.1 Cálculos Automáticos
- [ ] Implementar método para contar visitas por data
- [ ] Implementar método para contar visitas concluídas por data
- [ ] Implementar método para somar duração total por data
- [ ] Criar validação de limite de 8 horas (480 minutos)

### 6.2 Eventos e Triggers
- [ ] Implementar beforeSave para validação de duração
- [ ] Criar métodos para atualização automática dos campos calculados
- [ ] Adicionar tratamento de erro "Limite de horas atingido"

---

## **Etapa 7: Lógica de Negócio - Visits**
### 7.1 Cálculo de Duração
- [ ] Implementar método para calcular duração baseado em formulários e produtos
  - Formulários: 15 minutos cada
  - Produtos: 5 minutos cada
- [ ] Implementar beforeSave para atualizar duração automaticamente

### 7.2 Gerenciamento de Endereços
- [ ] Implementar lógica para substituir (não editar) endereços
- [ ] Criar método para deletar endereço antigo e criar novo
- [ ] Implementar validação de endereço obrigatório

### 7.3 Integração com Workdays
- [ ] Implementar afterSave para atualizar/criar workday correspondente
- [ ] Implementar lógica para atualização quando data é alterada
- [ ] Criar método para recalcular workdays afetados

---

## **Etapa 8: Controllers e Endpoints**
### 8.1 Controller Visits
- [ ] Criar `VisitsController` em `src/Controller/VisitsController.php`
- [ ] Implementar método `add()` para criação de visitas
- [ ] Implementar método `edit()` para edição de visitas
- [ ] Implementar método `index()` com filtro obrigatório por data
- [ ] Adicionar validações e tratamento de erros

### 8.2 Controller Workdays
- [ ] Criar `WorkdaysController` em `src/Controller/WorkdaysController.php`
- [ ] Implementar método `index()` para listagem
- [ ] Implementar método `close()` para fechar dia de trabalho
- [ ] Implementar lógica de realocação de visitas pendentes

### 8.3 Validação de Entrada
- [ ] Validar parâmetros obrigatórios em cada endpoint
- [ ] Implementar sanitização de dados
- [ ] Adicionar validação de formatos de data

---

## **Etapa 9: Rotas RESTful**
### 9.1 Configuração das Rotas
- [ ] Configurar rotas em `config/routes.php`
- [ ] Definir rotas para Visits: POST /visits, PUT /visits/:id, GET /visits
- [ ] Definir rotas para Workdays: GET /workdays, POST /workdays/close
- [ ] Implementar middleware para validação de API

### 9.2 Documentação das Rotas
- [ ] Documentar parâmetros esperados
- [ ] Documentar formatos de resposta
- [ ] Adicionar exemplos de uso

---

## **Etapa 10: Funcionalidade de Fechar Dia**
### 10.1 Lógica de Realocação
- [ ] Implementar método para identificar visitas pendentes
- [ ] Criar algoritmo para realocação respeitando limite de horas
- [ ] Implementar busca pelo próximo dia disponível
- [ ] Adicionar validação de capacidade de dias subsequentes

### 10.2 Testes da Funcionalidade
- [ ] Testar realocação simples (próximo dia)
- [ ] Testar realocação em cascata (múltiplos dias)
- [ ] Testar cenários limite de capacidade

---

## **Etapa 11: Validações e Tratamento de Erros**
### 11.1 Sistema de Validação CakePHP
- [ ] Implementar validators customizados para cada model
- [ ] Adicionar validações de formato (CEP, datas)
- [ ] Implementar validações de regras de negócio

### 11.2 Tratamento de Exceções
- [ ] Implementar tratamento para erros de API externa
- [ ] Adicionar tratamento para erros de banco de dados
- [ ] Criar responses padronizados para erros
- [ ] Implementar logging de erros

---

## **Etapa 12: Eventos e Otimizações**
### 12.1 Sistema de Eventos CakePHP
- [ ] Implementar eventos BeforeSave nos models necessários
- [ ] Implementar eventos AfterSave para atualizações em cascata
- [ ] Implementar eventos BeforeFind para aplicar máscaras

### 12.2 Otimizações de Performance
- [ ] Implementar eager loading para relacionamentos
- [ ] Otimizar queries de cálculos agregados
- [ ] Adicionar índices necessários no banco
- [ ] Implementar cache onde apropriado

---

## **Etapa 13: Testes e Validação**
### 13.1 Testes Unitários
- [ ] Criar testes para models principais
- [ ] Testar validações e regras de negócio
- [ ] Testar cálculos e fórmulas

### 13.2 Testes de Integração
- [ ] Testar endpoints via Postman/Insomnia
- [ ] Validar fluxos completos de criação e edição
- [ ] Testar cenários de erro

### 13.3 Testes de Casos Extremos
- [ ] Testar limites de duração
- [ ] Testar realocação complexa de visitas
- [ ] Testar falhas de API externa

---

## **Etapa 14: Documentação e Finalização**
### 14.1 Documentação Técnica
- [ ] Documentar APIs criadas
- [ ] Comentar código complexo
- [ ] Criar README com instruções de uso

### 14.2 Preparação para Entrega
- [ ] Revisar todos os requisitos
- [ ] Testar instalação limpa
- [ ] Publicar em repositório público
- [ ] Preparar email de entrega

---

## **Checklist Final de Requisitos**
- [ ] ✅ Migrations criadas e funcionando
- [ ] ✅ Integração com APIs de CEP (República Virtual → Via CEP)
- [ ] ✅ Cálculo automático de duração de visitas
- [ ] ✅ Limite de 8 horas por dia útil
- [ ] ✅ Substituição (não edição) de endereços
- [ ] ✅ Atualização automática de workdays
- [ ] ✅ Endpoints RESTful funcionando
- [ ] ✅ Funcionalidade de fechar dia implementada
- [ ] ✅ Validações e tratamento de erros
- [ ] ✅ Uso de eventos CakePHP
- [ ] ✅ Código bem documentado e organizado

---

## **Observações Importantes**
- **Não usar IA para geração de código** - Implementar tudo manualmente
- **Seguir padrões CakePHP** - Usar convenções do framework
- **Focar na qualidade** - Código limpo, bem estruturado e documentado
- **Testar thoroughly** - Validar todos os fluxos antes da entrega