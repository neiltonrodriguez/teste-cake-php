# Plano de Execução - Sistema de Gerenciamento de Visitas (Frontend React)

## **Etapa 1: Configuração Inicial do Projeto**
### 1.1 Preparação do Ambiente
- [ ] Clonar o repositório base fornecido
- [ ] Verificar se Docker e Node.js estão instalados
- [ ] Executar `docker-compose up` para subir os containers
- [ ] Analisar estrutura do projeto React existente
- [ ] Verificar configurações do TypeScript

### 1.2 Instalação de Dependências
- [ ] Instalar React Hook Form: `npm install react-hook-form`
- [ ] Instalar Styled Components: `npm install styled-components @types/styled-components`
- [ ] Instalar Axios: `npm install axios`
- [ ] Verificar se TypeScript está configurado corretamente
- [ ] Configurar ESLint e Prettier (se necessário)

---

## **Etapa 2: Estruturação de Tipos e Interfaces**
### 2.1 Definição de Tipos TypeScript
- [ ] Criar `src/types/Visit.ts`:
  ```typescript
  interface Visit {
    id: string;
    date: string;
    status: 'pendente' | 'concluida';
    forms: number;
    products: number;
    address: Address;
    duration: number; // em minutos
  }
  ```
- [ ] Criar `src/types/Address.ts`:
  ```typescript
  interface Address {
    cep: string;
    city: string;
    state: string;
    street: string;
    neighborhood: string;
    number: string;
  }
  ```
- [ ] Criar `src/types/GroupedVisits.ts` para agrupamentos
- [ ] Criar `src/types/ViaCepResponse.ts` para API

### 2.2 Tipos para Formulários
- [ ] Criar interfaces para React Hook Form
- [ ] Definir tipos para validações
- [ ] Criar tipos para estados do modal

---

## **Etapa 3: Configuração de Serviços e API**
### 3.1 Configuração do Axios
- [ ] Criar `src/services/api.ts`:
  - Configurar base URL para ViaCEP
  - Configurar interceptors
  - Configurar timeout e retry
- [ ] Criar `src/services/cepService.ts`:
  - Implementar função para consulta de CEP
  - Adicionar tratamento de erros
  - Implementar validação de CEP

### 3.2 Serviços de Persistência
- [ ] Criar `src/services/storageService.ts`:
  - Implementar save/load no localStorage
  - Adicionar serialização/deserialização
  - Implementar backup de dados

---

## **Etapa 4: Context API e Gerenciamento de Estado**
### 4.1 Context para Visitas
- [ ] Criar `src/contexts/VisitsContext.tsx`:
  - State para lista de visitas
  - Funções para CRUD de visitas
  - Função para agrupamento por data
  - Função para cálculo de durações

### 4.2 Context para UI
- [ ] Criar `src/contexts/UIContext.tsx`:
  - State para controle de modais
  - State para loading states
  - State para mensagens de erro/sucesso

### 4.3 Hooks Customizados
- [ ] Criar `src/hooks/useVisits.ts`
- [ ] Criar `src/hooks/useLocalStorage.ts`
- [ ] Criar `src/hooks/useCep.ts`

---

## **Etapa 5: Styled Components Base**
### 5.1 Theme e Design System
- [ ] Criar `src/styles/theme.ts`:
  - Definir paleta de cores (vermelho, verde, azul)
  - Definir breakpoints responsivos
  - Definir espaçamentos e tipografia

### 5.2 Componentes Base
- [ ] Criar `src/components/styled/Button.tsx`:
  - Variantes (primary, secondary, danger)
  - Estados (disabled, loading)
- [ ] Criar `src/components/styled/Input.tsx`:
  - Tipos (text, number, date)
  - Estados (disabled, error)
- [ ] Criar `src/components/styled/Modal.tsx`:
  - Backdrop e overlay
  - Animações de entrada/saída

---

## **Etapa 6: Componente de Header**
### 6.1 Estrutura do Header
- [ ] Criar `src/components/Header/Header.tsx`:
  - Logo/título da aplicação
  - Botão "Nova Visita"
  - Layout responsivo

### 6.2 Estilização
- [ ] Implementar design com Styled Components
- [ ] Adicionar hover effects
- [ ] Tornar responsivo

---

## **Etapa 7: Modal de Criação/Edição**
### 7.1 Estrutura do Modal
- [ ] Criar `src/components/VisitModal/VisitModal.tsx`:
  - Integração com React Hook Form
  - Campos conforme especificação
  - Validações em tempo real

### 7.2 Lógica do CEP
- [ ] Implementar campo CEP com máscara
- [ ] Implementar limpeza de campos ao alterar CEP
- [ ] Implementar desabilitação de campos durante requisição
- [ ] Implementar preenchimento automático após consulta
- [ ] Tratar casos onde bairro/logradouro vêm vazios

### 7.3 Validações e Controles
- [ ] Validar todos os campos obrigatórios
- [ ] Habilitar botão apenas com campos válidos
- [ ] Implementar validação de limite de horas por data
- [ ] Calcular duração: formulários (15min) + produtos (5min)

---

## **Etapa 8: Lista de Visitas Agrupadas**
### 8.1 Componente de Agrupamento
- [ ] Criar `src/components/VisitGroup/VisitGroup.tsx`:
  - Agrupar visitas por data
  - Formatar data (Ter. 28/08/2023)
  - Calcular estatísticas do grupo

### 8.2 Indicadores Visuais
- [ ] Criar barra de progresso para minutos agendados vs máximo (8h)
- [ ] Criar indicador de conclusão com cores:
  - < 60% = VERMELHO
  - > 90% = VERDE
  - Entre 60-90% = AZUL

### 8.3 Funcionalidade Fechar Dia
- [ ] Implementar botão "Fechar Dia"
- [ ] Lógica para realocar visitas pendentes
- [ ] Algoritmo para distribuir em próximos dias
- [ ] Respeitar limite de 8h por dia

---

## **Etapa 9: Item Individual de Visita**
### 9.1 Componente VisitItem
- [ ] Criar `src/components/VisitItem/VisitItem.tsx`:
  - Exibir quantidade de formulários e produtos
  - Mostrar duração estimada
  - Formatar endereço: "Logradouro, Número - CEP"
  - Formatar localização: "Bairro, Cidade - UF"

### 9.2 Estados e Ações
- [ ] Indicador visual de status (pendente/concluída)
- [ ] Botão "Editar" (apenas para pendentes)
- [ ] Botão "Concluir" (apenas para pendentes)
- [ ] Animações de transição de estado

---

## **Etapa 10: Lógica de Negócio**
### 10.1 Cálculos de Duração
- [ ] Implementar função para calcular duração total
- [ ] Validar limite de 8 horas por data
- [ ] Calcular percentuais de ocupação

### 10.2 Algoritmo de Realocação
- [ ] Implementar função para encontrar próximo dia disponível
- [ ] Distribuir visitas respeitando limites
- [ ] Atualizar datas das visitas realocadas

### 10.3 Persistência de Dados
- [ ] Salvar automaticamente no localStorage
- [ ] Carregar dados na inicialização
- [ ] Implementar backup/restore

---

## **Etapa 11: Funcionalidades Avançadas**
### 11.1 Loading States
- [ ] Implementar spinners para requisições CEP
- [ ] Loading durante realocação de visitas
- [ ] Skeleton loading para lista

### 11.2 Animações e Transições
- [ ] Animação de abertura/fechamento do modal
- [ ] Transições suaves entre estados
- [ ] Micro-animações nos botões

### 11.3 Tratamento de Erros
- [ ] Toast notifications para erros
- [ ] Validação de CEP inválido
- [ ] Feedback visual para erros de validação

---

## **Etapa 12: Portal para Modal**
### 12.1 Implementação de Portal
- [ ] Usar React.createPortal para modal
- [ ] Renderizar modal fora da hierarquia principal
- [ ] Gerenciar z-index e overlay

### 12.2 Acessibilidade
- [ ] Implementar trap de foco
- [ ] Fechar modal com ESC
- [ ] ARIA labels apropriados

---

## **Etapa 13: Responsividade e UX**
### 13.1 Design Responsivo
- [ ] Adaptar layout para mobile
- [ ] Ajustar modal para telas pequenas
- [ ] Otimizar lista para touch

### 13.2 Melhorias de UX
- [ ] Implementar filtros por status/data
- [ ] Busca por endereço
- [ ] Ordenação das visitas

---

## **Etapa 14: Testes e Validação**
### 14.1 Testes Manuais
- [ ] Testar criação de visitas
- [ ] Testar edição e conclusão
- [ ] Testar consulta de CEP
- [ ] Testar realocação de visitas
- [ ] Testar persistência de dados

### 14.2 Testes de Cenários
- [ ] Limite de 8 horas excedido
- [ ] CEP inválido/não encontrado
- [ ] Realocação em múltiplos dias
- [ ] Responsividade em diferentes telas

---

## **Etapa 15: Otimizações e Performance**
### 15.1 Performance
- [ ] Implementar React.memo onde necessário
- [ ] Otimizar re-renders desnecessários
- [ ] Debounce na consulta de CEP

### 15.2 Bundle Optimization
- [ ] Verificar tamanho do bundle
- [ ] Implementar code splitting se necessário
- [ ] Otimizar imports

---

## **Etapa 16: Documentação e Entrega**
### 16.1 Documentação
- [ ] Comentar código complexo
- [ ] Criar README com instruções
- [ ] Documentar componentes principais

### 16.2 Preparação para Entrega
- [ ] Revisar todos os requisitos
- [ ] Testar build de produção
- [ ] Publicar em repositório público
- [ ] Preparar email de entrega

---

## **Checklist Final de Requisitos**

### Tecnologias Obrigatórias
- [ ] ✅ React Hook Form implementado
- [ ] ✅ Styled Components para toda estilização
- [ ] ✅ Axios para requisições
- [ ] ✅ TypeScript com tipagem completa

### Funcionalidades Core
- [ ] ✅ Modal de criação/edição com todos os campos
- [ ] ✅ Consulta automática de CEP (viacep.com.br)
- [ ] ✅ Validação de limite de 8 horas por data
- [ ] ✅ Agrupamento de visitas por data
- [ ] ✅ Indicadores visuais de progresso
- [ ] ✅ Funcionalidade "Fechar Dia" com realocação
- [ ] ✅ Lista de visitas com formatação específica

### Regras de Negócio
- [ ] ✅ Duração: 15min/formulário + 5min/produto
- [ ] ✅ Campos de endereço desabilitados durante requisição CEP
- [ ] ✅ Bairro/logradouro habilitados se API não retornar
- [ ] ✅ Realocação automática respeitando limites

### Diferenciais Implementados
- [ ] 🎨 Estilização atrativa e moderna
- [ ] 📝 Código bem comentado
- [ ] ⚛️ Context API e Portals
- [ ] 🔄 Loading states e animações
- [ ] 💾 Persistência no localStorage
- [ ] 📱 Design responsivo

---

## **Estrutura de Pastas Sugerida**
```
src/
├── components/
│   ├── Header/
│   ├── VisitModal/
│   ├── VisitGroup/
│   ├── VisitItem/
│   └── styled/
├── contexts/
├── hooks/
├── services/
├── styles/
├── types/
└── utils/
```

---

## **Observações Importantes**
- **Proibido uso de IA** - Todo código deve ser escrito manualmente
- **Foco na experiência do usuário** - Priorizar UX fluida e intuitiva
- **Código limpo e tipado** - Maximizar uso do TypeScript
- **Performance** - Otimizar re-renders e carregamentos
- **Responsividade** - Funcionar bem em diferentes dispositivos