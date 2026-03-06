# Casos de Teste

## CT01 – Criação de novo usuário com sucesso
**Requisito:** Módulo de Cadastro de Usuários
**Descrição:** Verificar se o sistema permite a criação de um novo usuário quando dados válidos são fornecidos.
**Pré-condição:** Usuário autenticado com perfil ADMIN (U01) e token JWT válido.
**Entrada:** Dados válidos (Nome, Email, Senha, Perfil) informados no formulário ou payload da requisição.
**Resultado Esperado:** O sistema deve criar o usuário, retornar status 201 (Created) e o novo registro deve constar no banco de dados.

## CT02 – Validação de restrição de acesso (Erro 403)
**Requisito:** Módulo de Cadastro de Usuários / Segurança
**Descrição:** Garantir que usuários sem privilégios não consigam realizar ações administrativas, como excluir outros usuários.
**Pré-condição:** Usuário autenticado com perfil USER (U02 ou U03) e token JWT válido.
**Entrada:** Acionar a ação ou requisição de exclusão de um usuário existente.
**Resultado Esperado:** O sistema deve bloquear a ação e retornar o status HTTP 403 Forbidden, respeitando a matriz de permissões.

## CT03 – Reserva de sala disponível
**Requisito:** Módulo de Reservas de salas
**Descrição:** Validar a criação de uma reserva para uma sala de laboratório em um horário totalmente livre.
**Pré-condição:** Usuário autenticado (USER - U02). Sala "Lab A" cadastrada. Nenhuma reserva existente para a data/hora escolhida.
**Entrada:** Selecionar "Lab A", definir a data para o dia seguinte e o horário das 14:00 às 16:00. Confirmar reserva.
**Resultado Esperado:** Reserva concluída com sucesso. O status do horário da respectiva sala deve ser alterado para "Reservado" no sistema.

## CT04 – Bloqueio por conflito de horário
**Requisito:** Módulo de Reservas de salas
**Descrição:** Verificar se o sistema impede a reserva de uma sala que já possui outra reserva no mesmo período (sobreposição).
**Pré-condição:** Usuário autenticado (USER - U03). A sala "Lab A" já possui uma reserva confirmada das 14:00 às 16:00 para o dia seguinte.
**Entrada:** Selecionar "Lab A", definir a mesma data do dia seguinte e tentar reservar das 15:00 às 17:00.
**Resultado Esperado:** O sistema deve impedir a reserva, emitir um alerta de conflito de horário e não salvar o novo registro.

## CT05 – Abertura de incidente
**Requisito:** Módulo de Registro de incidentes técnicos
**Descrição:** Validar a capacidade de um usuário registrar um novo problema técnico referente a um laboratório.
**Pré-condição:** Usuário autenticado no sistema (USER - U02).
**Entrada:** Acessar o módulo de incidentes, preencher a descrição "Projetor quebrado no Lab B" e salvar.
**Resultado Esperado:** Incidente criado com sucesso. O status inicial do chamado deve ser definido automaticamente pelo sistema como "Aberto".

## CT06 – Ciclo de vida: Transição de Estado de incidente
**Requisito:** Módulo de Registro de incidentes técnicos
**Descrição:** Verificar se as mudanças de status do incidente seguem o fluxo definido no ciclo de vida.
**Pré-condição:** Usuário autenticado como ADMIN (U01). O incidente do CT05 deve estar criado e com o status "Aberto".
**Entrada:** Acessar o incidente, alterar o status para "Em Atendimento" e salvar. Em seguida, alterar para "Resolvido" e salvar.
**Resultado Esperado:** O sistema deve permitir as transições de estado na ordem correta, registrando o histórico de mudanças de cada etapa.

## CT07 – Validação de Autenticação (Erro 401)
**Requisito:** Segurança e Infraestrutura (API)
**Descrição:** Garantir que endpoints e páginas protegidas exijam um token de autenticação válido para acesso.
**Pré-condição:** Não possuir token de autenticação no Header (ou utilizar um token expirado/inválido).
**Entrada:** Tentar fazer uma requisição GET para listar as salas de laboratório.
**Resultado Esperado:** O sistema deve rejeitar a requisição e retornar o status HTTP 401 Unauthorized.

## CT08 – Cadastro de sala de laboratório com sucesso
**Requisito:** Módulo de Cadastro de salas de laboratório
**Descrição:** Validar a inserção de uma nova sala de laboratório com as especificações corretas no sistema.
**Pré-condição:** Usuário autenticado com perfil ADMIN (U01).
**Entrada:** Acessar módulo de salas, preencher "Nome" (ex: Lab C) e "Capacidade" (ex: 30). Salvar o registro.
**Resultado Esperado:** A sala deve ser criada com sucesso (Status 201) e ser exibida corretamente nas listagens de consultas de salas.

## CT09 – Validação de capacidade da sala (Valor Limite)
**Requisito:** Módulo de Cadastro de salas de laboratório
**Descrição:** Testar os limites de valores permitidos para a capacidade da sala, impedindo valores nulos ou negativos.
**Pré-condição:** Usuário autenticado com perfil ADMIN (U01).
**Entrada:** Acessar módulo de cadastro de salas, preencher "Nome" e definir "Capacidade" como 0 ou -5. Tentar salvar.
**Resultado Esperado:** O sistema deve aplicar a regra de validação, impedir o cadastro e retornar erro (Status 400 Bad Request) indicando que a capacidade deve ser maior que zero.

## CT10 – Tentativa de cadastro com e-mail duplicado
**Requisito:** Módulo de Cadastro de Usuários
**Descrição:** Garantir a restrição de unicidade para o campo e-mail durante a criação de novas contas.
**Pré-condição:** Usuário autenticado como ADMIN (U01). O usuário com e-mail "prof@srl.edu" (U02) já existe no banco de dados.
**Entrada:** Tentar criar um novo usuário informando o e-mail "prof@srl.edu" e enviar a requisição.
**Resultado Esperado:** O sistema deve rejeitar o cadastro por violação de unicidade e retornar um erro apropriado (Status 409 Conflict).

## CT11 – Cancelamento de reserva própria
**Requisito:** Módulo de Reservas de salas
**Descrição:** Validar se o sistema permite que um usuário cancele ativamente uma reserva futura feita por ele mesmo.
**Pré-condição:** Usuário autenticado como USER (U02). O usuário possui uma reserva ativa para uma data futura.
**Entrada:** Acessar a lista de "Minhas Reservas", selecionar a reserva futura e acionar a opção de cancelamento.
**Resultado Esperado:** A reserva deve ser cancelada com sucesso e a sala deve voltar a constar como disponível para o horário liberado.

## CT12 – Restrição de horário de reserva (Regra de Negócio)
**Requisito:** Módulo de Reservas de salas
**Descrição:** Verificar se o sistema bloqueia tentativas de reservas fora do horário de funcionamento configurado para a instituição.
**Pré-condição:** Usuário autenticado (USER - U03). Sala "Lab A" cadastrada.
**Entrada:** Selecionar "Lab A" e tentar confirmar uma reserva em um horário fora do expediente (ex: 23:00 às 02:00).
**Resultado Esperado:** O sistema deve detectar a restrição de horário, impedir a gravação da reserva e exibir um erro de validação para o usuário.

## CT13 – Geração de relatório de reservas por período
**Requisito:** Módulo de Relatórios e Consultas
**Descrição:** Validar a extração correta de dados de reservas aplicando um filtro de período de datas.
**Pré-condição:** Usuário autenticado como ADMIN (U01). Devem existir reservas cadastradas no banco de dados para o mês atual.
**Entrada:** Acessar módulo de Relatórios, aplicar o filtro informando a data inicial e final do mês atual e solicitar a geração.
**Resultado Esperado:** O sistema deve processar a requisição e retornar a lista de reservas contendo apenas os dados restritos ao período solicitado (Status 200).

## CT14 – Restrição de acesso a relatórios gerenciais
**Requisito:** Módulo de Relatórios e Consultas / Segurança
**Descrição:** Garantir que usuários comuns não possam visualizar ou baixar relatórios que contêm dados globais da instituição.
**Pré-condição:** Usuário autenticado como USER (U02).
**Entrada:** Tentar acessar via URL direta ou requisição de API o endpoint de relatórios gerenciais globais.
**Resultado Esperado:** O sistema deve acionar a matriz de permissões, retornar o status 403 Forbidden e impedir totalmente o acesso aos dados.

## CT15 – Transição de estado inválida de incidente
**Requisito:** Módulo de Registro de incidentes técnicos
**Descrição:** Validar se o sistema impede que um incidente já encerrado ("Resolvido") retorne indevidamente a um status operacional sem seguir regras de reabertura.
**Pré-condição:** Usuário autenticado como ADMIN (U01). Deve existir um incidente no banco com o status "Resolvido".
**Entrada:** Acessar os detalhes deste incidente "Resolvido" e tentar alterar o status diretamente para "Aberto" ou "Em Atendimento".
**Resultado Esperado:** O sistema deve bloquear a operação para manter o ciclo de vida íntegro, retornando uma mensagem de erro por transição de estado não permitida.