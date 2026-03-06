# PLANO DE TESTE

## Sistema SRL – Sistema de Registro e Licenciamento

*Versão 1.0 | Março de 2026*

| Campo | Informação |
| --- | --- |
| Projeto | Sistema SRL |
| Versão do Sistema | 1.0.0 |
| Documento elaborado por | [Mateus de Jesus] |
| Data de elaboração | 03/03/2026 |
| Data de revisão | [03/03/2026] |
| Status | Concluído |

### 1. Escopo

O presente plano de teste abrange as atividades de verificação e validação do Sistema de Registro e Licenciamento (SRL), desenvolvido para gerenciar o ciclo de vida de licenças, cadastros de usuários, emissão de documentos e controle de permissões.

### 1.1 Funcionalidades no Escopo

* Módulo de Cadastro de Usuários (criação, edição, exclusão)
* Módulo de Cadastro de salas de laboratório
* Módulo de Reservas de salas
* Módulo de Relatórios e Consultas
* Módulo de Registro de incidentes técnicos

### 2. Objetivo

O objetivo deste plano de teste é definir como os testes serão executados na prática (abordagem), os recursos, o cronograma e as atividades necessárias para garantir que o Sistema SRL atenda aos requisitos funcionais e não funcionais especificados, assegurando a qualidade antes da implantação em produção.

**Os objetivos específicos são:**

* As operações automatizadas por perfil ADM / USER funcionem corretamente
* As regras de validação de dados sejam aplicadas corretamente
* Os retornos (401, 403) sejam adequados a cada cenário
* Conflitos de reservas e restrições de horário sejam detectados pelo sistema
* Incidentes sigam o ciclo de vida

### 3. Estratégia de Teste

A estratégia adotada combina diferentes níveis e tipos de teste para garantir cobertura abrangente do sistema.

### 3.1 Tipos de Teste

| Tipo | Objetivo |
| --- | --- |
| Teste Funcional | Verificar se as funcionalidades atendem aos requisitos especificados |
| Teste de Regressão | Garantir que novas alterações não quebrem funcionalidades existentes |
| Teste de Desempenho | Avaliar o comportamento do sistema sob carga e stress |
| Teste de Segurança | Identificar vulnerabilidades de acesso e proteção de dados |
| Teste de Usabilidade | Avaliar a experiência do usuário e a interface do sistema |
| Teste de Compatibilidade | Verificar o funcionamento em diferentes browsers e sistemas operacionais |

### 3.2 Técnicas de Teste

* Partição de Equivalência – classes válidas e inválidas para cada campo
* Análise de Valor Limite – limites mínimos/máximos de campos numéricos e horários
* Tabela de Decisão – combinações de perfil x operação (matriz de permissões)
* Teste de Transição de Estado – ciclo de vida de reservas e incidentes

### 4. Ambiente de Teste

### 4.1 Infraestrutura

| Componente | Especificação |
| --- | --- |
| URL Base da API | http://localhost:8080 (ou ambiente de homologação definido) |
| Formato de dados | JSON (Content-Type: application/json) |
| Autenticação | Bearer Token JWT no header Authorization |
| Banco de Dados | Instância de homologação isolada (dados fictícios) |
| Padrão de datas | ISO 8601: YYYY-MM-DD |

### 4.2 Usuários de Teste

| ID | Nome | Email | Senha | Perfil | Uso |
| --- | --- | --- | --- | --- | --- |
| U01 | Admin Teste | admin@srl.edu | Admin123 | ADMIN | Operações exclusivas de ADMIN |
| U02 | Professor Teste | prof@srl.edu | Senha456 | USER | Operações de USER autenticado |
| U03 | Aluno Teste | aluno@srl.edu | Senha789 | USER | Segundo USER (testes de isolamento) |

### 5. Critérios de Entrada

Os testes somente serão iniciados quando todas as condições abaixo forem satisfeitas:

| # | Critérios | Status |
| --- | --- | --- |
| CE01 | API implantada e acessível no ambiente de homologação | [ ] Pendente |
| CE02 | Documentação de requisitos (RF e RN) revisada e aprovada | [ ] Pendente |
| CE03 | Banco de dados limpo e com massa de dados de teste carregada | [ ] Pendente |
| CE04 | Ferramentas de teste configuradas (Postman / cURL) | [ ] Pendente |
| CE05 | Casos de teste revisados pelo líder de QA | [ ] Pendente |
| CE06 | API implantada e acessível no ambiente de homologação | [ ] Pendente |

### 6. Critérios de Saída

Os testes serão encerrados e o sistema poderá ser aprovado para implantação quando os critérios abaixo forem atingidos:

| # | Critério de Saída | Meta |
| --- | --- | --- |
| CS01 | Percentual de casos de teste executados | ≥ 95% |
| CS02 | Casos de teste com resultado PASSOU | ≥ 90% |
| CS03 | Defeitos críticos (bloqueadores) em aberto | 0 (zero) |
| CS04 | Todos os RF cobertos por ao menos 1 caso de teste positivo | 100% |
| CS05 | Todas as RN cobertos por ao menos 1 caso de teste negativo | 100% |

### 7. Responsáveis

| Nome | Cargo / Papel | Responsabilidades |
| --- | --- | --- |
| [Israel] | Gerente de Projeto | Acompanhamento geral, aprovação de critérios de saída |
| [Mateus] | Líder de Qualidade (QA Lead) | Coordenação dos testes, revisão do plano, reporte de status |
| [Caio] | Analista de Teste Júnior | Execução de casos de teste e suporte à automação |
