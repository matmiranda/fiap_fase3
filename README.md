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

### Configuração do Pipeline

O pipeline CI/CD segue a estrutura abaixo:

1. **Restaurar Dependências**: O comando `dotnet restore` é executado para baixar todas as dependências do projeto.
2. **Compilar o Código**: O comando `dotnet build` compila a solução na configuração `Release`.
3. **Subir o RabbitMQ para Testes**: Um container do RabbitMQ é iniciado via Docker para simular a fila de mensagens.
4. **Definir Variáveis de Ambiente**: As credenciais do RabbitMQ são configuradas como variáveis no ambiente do pipeline.
5. **Executar a API em Background**: A aplicação é iniciada para validar sua inicialização correta.
6. **Verificar Saúde da API**: São feitas chamadas para os endpoints `/health` e `/metrics` para garantir que a API está funcional.
7. **Executar Testes Unitários**: Os testes automatizados são executados para validar o comportamento da aplicação.
8. **Publicar Artefatos**: O comando `dotnet publish` gera o pacote para deploy.
9. **Deploy para o Azure**: A aplicação é enviada automaticamente para o serviço Azure Web Apps.

O deploy no Azure é realizado utilizando a action `azure/webapps-deploy@v3`, garantindo que a versão mais recente da aplicação esteja disponível.

Para que o deploy funcione corretamente, é necessário configurar o **secret** `AZURE_WEBAPP_PUBLISH_PROFILE` no repositório do GitHub.

## Conclusão

A fase 3 do projeto aprimorou a arquitetura, garantindo maior escalabilidade e desempenho por meio da separação dos microserviços e consumidores. Além disso, integrações com RabbitMQ, Prometheus e Grafana garantem alta disponibilidade e monitoramento eficiente.
