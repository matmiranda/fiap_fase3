## Apresentação

Link do vídeo: [Inserir link do vídeo]

## Descrição

Este projeto é composto por uma arquitetura de microsserviços, desenvolvida para gerenciamento de contatos. Ele segue as melhores práticas de desenvolvimento, garantindo escalabilidade, desempenho e facilidade de manutenção.

## API Gateway no Azure

Para facilitar a comunicação entre os microsserviços, foi configurado um **API Gateway no Azure**, que expõe as seguintes rotas:

- **`PUT /contato`** – Atualiza um contato pelo ID.  
  - Repositório: [producer-update-contact](https://github.com/matmiranda/microservice-update-contact)  
- **`POST /contato`** – Cria um novo contato.  
  - Repositório: [producer-create-contact](https://github.com/matmiranda/microservice-create-contact)  
- **`DELETE /contato/{id}`** – Deleta um contato pelo ID.  
  - Repositório: [producer-delete-contact](https://github.com/matmiranda/microservice-delete-contact)  
- **`GET /contato/{id}`** – Obtém um contato pelo ID. *(Usa Azure Functions)*  
  - Repositório: [azure-functions-consulta-contato-id](https://github.com/matmiranda/azure-functions-consulta-contato-id)  
- **`POST /contato/validar-duplicidade`** – Valida a duplicidade de um contato com base no e-mail. *(Usa Azure Functions)*  
  - Repositório: [azure-functions-valida-duplicidade](https://github.com/matmiranda/azure-functions-valida-duplicidade)  

Os três primeiros endpoints direcionam as requisições para os respectivos microsserviços de **CREATE, UPDATE e DELETE**, enquanto os dois últimos são implementados usando **Azure Functions**, garantindo maior escalabilidade e eficiência no processamento.


Cada operação foi separada em microserviços independentes para maior desacoplamento e eficiência:

- **Producer CREATE**: Responsável por criar novos contatos e enviar mensagens para a fila.  
  - Repositório: [producer-create-contact](https://github.com/matmiranda/microservice-create-contact)
- **Producer UPDATE**: Responsável por atualizar contatos e enviar mensagens para a fila.  
  - Repositório: [producer-update-contact](https://github.com/matmiranda/microservice-update-contact)
- **Producer DELETE**: Responsável por excluir contatos e enviar mensagens para a fila.  
  - Repositório: [producer-delete-contact](https://github.com/matmiranda/microservice-delete-contact)

Além disso, três consumidores independentes processam as mensagens da fila e realizam a persistência dos dados no banco MySQL:

- **Consumer CREATE**: Lê mensagens da fila e insere contatos no MySQL.  
  - Repositório: [consumer-create-contact](https://github.com/marlamoury/Consumer.Contact.Create)
- **Consumer UPDATE**: Lê mensagens da fila e atualiza contatos no MySQL.  
  - Repositório: [consumer-create-contact](https://github.com/marlamoury/Consumer.Contact.Update)
- **Consumer DELETE**: Lê mensagens da fila e remove contatos do MySQL.  
  - Repositório: [consumer-create-contact](https://github.com/marlamoury/Consumer.Contact.Delete)

## Tecnologias Utilizadas

- .NET 8
- Swagger
- Dapper
- MySQL
- RabbitMQ (mensageria para filas)
- Prometheus e Grafana no Azure
- GitHub Actions (para CI/CD)

## Conclusão

A fase 3 do projeto aprimorou a arquitetura, garantindo maior escalabilidade e desempenho por meio da separação dos microserviços e consumidores. Além disso, integrações com RabbitMQ, Prometheus e Grafana garantem alta disponibilidade e monitoramento eficiente.
