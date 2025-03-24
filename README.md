## Apresentação

Link do vídeo: [Inserir link do vídeo]

## Descrição

Este projeto é composto por uma arquitetura de microsserviços, desenvolvida para gerenciamento de contatos. Ele segue as melhores práticas de desenvolvimento, garantindo escalabilidade, desempenho e facilidade de manutenção.

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
- Prometheus e Grafana (para monitoramento)
- GitHub Actions (para CI/CD)

## GitHub Actions Workflow

Este projeto utiliza GitHub Actions para automação do CI/CD. O workflow está definido em `.github/workflows/dotnet.yml` e inclui:

- **Build**: Compila e gera artefatos.
- **Test**: Executa testes automatizados.
- **Deploy**: Publica os microsserviços.

## Conclusão

A fase 3 do projeto aprimorou a arquitetura, garantindo maior escalabilidade e desempenho por meio da separação dos microserviços e consumidores. Além disso, integrações com RabbitMQ, Prometheus e Grafana garantem alta disponibilidade e monitoramento eficiente.
