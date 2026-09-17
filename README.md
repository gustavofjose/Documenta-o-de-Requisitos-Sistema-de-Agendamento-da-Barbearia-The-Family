# Documentação do Sistema: Barbearia The Family

## 1. Caracterização da Organização
A Barbearia The Family é uma empresa privada de pequeno porte, atuando no setor de prestação de serviços de beleza e estética masculina. Contando com 2 cadeiras de atendimento e apenas 1 profissional responsável, o barbeiro Victor. As principais atividades envolvem cortes de cabelo, barba, combos e serviços extras. 

Durante a pesquisa de campo, foram identificados problemas críticos relacionados à gestão da agenda, como a confusão na marcação de horários e o alto índice de cancelamentos de última hora ou ausência de clientes sem aviso prévio. A escolha desta organização justifica-se pela necessidade real de um sistema que organize a agenda do único profissional, evite conflitos de horários e elimine os prejuízos causados pela ociosidade. 

**[FOTO DAS PROVAS QUE TIVEMOS A CONVERSA]**

## 2. Processos Mapeados
O principal processo da barbearia é o de Agendamento e Atendimento. O fluxo inicia-se quando o cliente acessa o sistema e escolhe o serviço desejado (corte, barba, combo, sobrancelha, coloração, progressiva, etc.). Em seguida, o sistema permite a seleção do profissional (Victor) e a escolha da data e horário, verificando a disponibilidade em tempo real. Após a seleção, o cliente define a forma de pagamento, podendo adicionar serviços extras opcionais (hidratação capilar, design de sobrancelha, vaporização facial, óleo para barba, finalização, produtos específicos). Por fim, o sistema confirma o agendamento, envia lembretes e, após a execução do serviço, permite que o cliente registre uma avaliação sobre o atendimento. O sistema deve estar preparado para bloquear automaticamente os agendamentos aos domingos e segundas-feiras, liberando a agenda apenas para os dias de funcionamento.

## 3. Requisitos do Sistema
Para atender às necessidades da barbearia, o sistema deve ser capaz de realizar o cadastro de clientes, registrar agendamentos, bloquear horários já ocupados, registrar a forma de pagamento escolhida e enviar notificações automáticas de confirmação e lembretes (1 dia e 1 hora antes). Além disso, deve gerar relatórios diários de atendimentos para que o barbeiro acompanhe sua meta de 8 a 9 cortes por dia útil, e permitir o registro de avaliações pós-atendimento. O sistema deve possuir uma regra de calendário que impeça a seleção de domingos e segundas-feiras, mas permita a seleção de feriados. 

Como requisitos não funcionais, o sistema precisa garantir a segurança dos dados dos clientes em conformidade com a LGPD, possuir uma interface de usabilidade simples e rápida (já que o barbeiro a acessará entre um corte e outro), estar disponível 24 horas por dia para que os clientes possam agendar a qualquer momento e apresentar um desempenho de verificação de horários em menos de 2 segundos.

## 4. Regras de Negócio
A Barbearia The Family segue regras estritas que devem ser implementadas no sistema:
*   O atendimento é exclusivamente mediante agendamento prévio.
*   Não é permitido pagamento "fiado".
*   As formas de pagamento aceitas são Pix, cartão de crédito, cartão de débito e dinheiro.
*   O horário de funcionamento é das 08:00 às 19:00, de terça-feira a sábado.
*   A barbearia não abre aos domingos e nem às segundas-feiras.
*   Nos feriados, a barbearia funciona normalmente para atendimento.
*   A duração padrão de um corte é de 40 minutos, porém, se houver serviços adicionais (como barba ou sobrancelha), o tempo aumenta para 1 hora.
*   A meta diária é de 8 a 9 cortes por dia útil, e o sistema deve alertar o barbeiro caso a agenda apresente brechas que impeçam o cumprimento dessa meta.
*   Cancelamentos com menos de 1 hora de antecedência devem ser registrados no histórico do cliente para controle.
*   Os serviços e profissionais possuem um campo "ativo", permitindo que sejam desativados sem serem excluídos do banco de dados.

## 5. Dicionário de Dados
Para garantir a integridade do modelo, os dados dos clientes utilizados em exemplos devem ser fictícios. A estrutura preliminar das entidades e seus atributos, alinhada ao modelo lógico apresentado, é detalhada na tabela abaixo:

| Entidade | Atributo | Tipo | Descrição | Regra |
| :--- | :--- | :--- | :--- | :--- |
| **Cliente** | id_cliente | INT | Identificador único do cliente | PK, Obrigatório |
| | nome | VARCHAR(100) | Nome completo do cliente | Obrigatório |
| | telefone | VARCHAR(20) | Telefone para contato/WhatsApp | Obrigatório |
| | email | VARCHAR(100) | E-mail para envio de confirmações | Opcional |
| **Profissional** | id_profissional | INT | Identificador único do barbeiro | PK, Obrigatório |
| | nome | VARCHAR(100) | Nome do profissional (Victor) | Obrigatório |
| | especialidade | VARCHAR(100) | Área de especialidade | Opcional |
| | telefone | VARCHAR(20) | Telefone do profissional | Opcional |
| | ativo | BOOLEAN | Indica se o profissional está ativo | Obrigatório |
| **Agendamento** | id_agendamento | INT | Identificador do agendamento | PK, Obrigatório |
| | id_cliente | INT | Referência ao cliente | FK, Obrigatório |
| | id_profissional | INT | Referência ao profissional | FK, Obrigatório |
| | data | DATE | Data do agendamento | Obrigatório. Deve ser terça a sábado ou feriado. |
| | hora_inicio | TIME | Hora de início | Obrigatório (entre 08:00 e 19:00) |
| | hora_fim | TIME | Hora de término | Obrigatório |
| | status | VARCHAR(30) | Situação (Confirmado, Cancelado, etc.) | Obrigatório |
| | observacao | VARCHAR(255) | Observações gerais | Opcional |
| **Servico** | id_servico | INT | Identificador do serviço | PK, Obrigatório |
| | nome | VARCHAR(100) | Nome do serviço (Corte, Barba, etc.) | Obrigatório |
| | descricao | VARCHAR(255) | Descrição do serviço | Opcional |
| | duracao | INT | Tempo estimado em minutos | Obrigatório |
| | preco | DECIMAL(10,2) | Valor do serviço | Obrigatório |
| | ativo | BOOLEAN | Indica se o serviço está ativo | Obrigatório |
| **Agendamento_Servico** | id_agendamento | INT | Referência ao agendamento | PK, FK, Obrigatório |
| | id_servico | INT | Referência ao serviço | PK, FK, Obrigatório |
| **Servico_Extra** | id_extra | INT | Identificador do serviço extra | PK, Obrigatório |
| | nome | VARCHAR(100) | Nome do extra (Hidratação, etc.) | Obrigatório |
| | descricao | VARCHAR(255) | Descrição do extra | Opcional |
| | preco | DECIMAL(10,2) | Valor do extra | Obrigatório |
| | duracao | INT | Tempo adicional em minutos | Obrigatório |
| | ativo | BOOLEAN | Indica se o extra está ativo | Obrigatório |
| **Agendamento_Extra** | id_agendamento | INT | Referência ao agendamento | PK, FK, Obrigatório |
| | id_extra | INT | Referência ao serviço extra | PK, FK, Obrigatório |
| **Pagamento** | id_pagamento | INT | Identificador da transação | PK, Obrigatório |
| | id_agendamento | INT | Referência ao agendamento | FK, Obrigatório |
| | forma_pagamento | VARCHAR(30) | Forma de pagamento (Pix, Cartão, etc.) | Obrigatório |
| | valor | DECIMAL(10,2) | Valor pago | Obrigatório |
| | status | VARCHAR(30) | Status do pagamento (Pago, Pendente) | Obrigatório |
| | data_pagamento | DATETIME | Data e hora do pagamento | Obrigatório |
| **Avaliacao** | id_avaliacao | INT | Identificador da avaliação | PK, Obrigatório |
| | id_agendamento | INT | Referência ao agendamento | FK, Obrigatório |
| | nota | INT | Nota da avaliação | Obrigatório |
| | comentario | VARCHAR(500) | Comentário do cliente | Opcional |
| | data_avaliacao | DATETIME | Data e hora da avaliação | Obrigatório |

## 6. Modelagem e DER
O Diagrama Entidade-Relacionamento (DER) foi construído com base nas entidades CLIENTE, PROFISSIONAL, AGENDAMENTO, SERVICO, SERVICO_EXTRA, PAGAMENTO e AVALIACAO, além das tabelas associativas AGENDAMENTO_SERVICO e AGENDAMENTO_EXTRA. 

As cardinalidades refletem a realidade da barbearia: um CLIENTE pode realizar vários AGENDAMENTOS (1:N), enquanto cada agendamento pertence a um único cliente. Da mesma forma, um PROFISSIONAL (Victor) pode ter vários agendamentos (1:N). A relação entre AGENDAMENTO e SERVICO é do tipo N:N, resolvida pela tabela associativa AGENDAMENTO_SERVICO. O mesmo ocorre para os serviços extras, onde a tabela AGENDAMENTO_EXTRA resolve a relação N:N entre AGENDAMENTO e SERVICO_EXTRA. Por fim, cada AGENDAMENTO gera um único PAGAMENTO (1:0..1) e pode gerar uma única AVALIACAO (1:0..1). 

## 7. Justificativa Técnica
A modelagem proposta foi estruturada para resolver os problemas identificados na pesquisa de campo e garantir a integridade dos dados. A entidade CLIENTE foi criada para armazenar o histórico de quem frequenta a barbearia, permitindo identificar clientes que cancelam com frequência. A entidade PROFISSIONAL é essencial para gerenciar a agenda do Victor e evitar sobreposição de horários. O AGENDAMENTO atua como a entidade central, conectando cliente, profissional, serviços e pagamento.

A criação das tabelas associativas AGENDAMENTO_SERVICO e AGENDAMENTO_EXTRA justifica-se pela necessidade de permitir que um único agendamento contemple múltiplos serviços (ex: corte + barba) e múltiplos extras (ex: hidratação + sobrancelha), sem ferir a normalização do banco de dados. A separação da entidade PAGAMENTO, com relacionamento 1:0..1, permite que o sistema controle o status financeiro do agendamento (Pago, Pendente) e respeite a regra de negócio "não aceita fiado". Da mesma forma, a entidade AVALIACAO, também com relacionamento 1:0..1, permite que o cliente avalie o serviço prestado, contribuindo para a melhoria contínua e para o controle de qualidade da barbearia. O campo "ativo" presente nas entidades SERVICO, SERVICO_EXTRA e PROFISSIONAL garante que registros antigos não sejam excluídos permanentemente, preservando o histórico de agendamentos passados.

A inclusão da restrição de dias de funcionamento (fechamento aos domingos e segundas, abertura em feriados) no modelo lógico e nas regras de negócio é fundamental para evitar que o sistema permita agendamentos em dias em que a barbearia não opera. Embora o DER não tenha uma entidade específica para "dias de funcionamento", essa regra é aplicada diretamente na validação do campo data da entidade AGENDAMENTO, garantindo que a data escolhida pelo cliente seja válida antes da confirmação.

## 8. Uso de IA
Para a elaboração desta documentação, foi utilizada a ferramenta de IA Gemini e Deepseek. A IA foi empregada na estruturação do README, na criação dos requisitos funcionais e não funcionais e no auxílio à modelagem do DER com base nas regras de negócio fornecidas. O prompt solicitava a estruturação de um sistema de agendamento com base nas regras da Barbearia The Family (corte de 40 min, sem fiado, fechamento aos domingos/segundas). 

A IA respondeu com uma estrutura padrão de README e sugestões de modelagem. Aproveitamos a organização dos tópicos e o alinhamento conceitual com o DER. O documento foi posteriormente revisado e ajustado manualmente pelo grupo para refletir exatamente o Diagrama Lógico criado no BRMW, incluindo as tabelas associativas e a entidade de avaliação. Foram corrigidas e rejeitadas as tentativas da IA de inventar nomes de clientes e preços específicos.

