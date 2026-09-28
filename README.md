# collectionTestApiEventService

## 🧩 Mocks do EventService (WireMock)

Assim como o `collectionTestApiUserService` mantém mappings que simulam o UserService, este repositório mantém os mappings que simulam o **EventService**, que serão consumidos pela collection do [`userService`](https://github.com/LucasMCFidelis/collectionTestApiUserService) — especificamente pelo fluxo de **gerenciamento dos eventos favoritos**, de dados validos de eventos que são fornecidos pelo EventService.

| Arquivo                     | Endpoint simulado       | `X-Mock-Scenario`                      | Resposta                       |
| --------------------------- | ----------------------- | -------------------------------------- | ------------------------------ |
| `01-success-get-event.json` | `GET /events/{eventId}` | `SUCCESS_GET_EVENT`                    | `200` — Evento válido          |
| `00-fallback.json`          | `ANY /*`                | *(qualquer cenário não mapeado acima)* | `500` — "Scenario not defined" |

### Subindo o mock isoladamente

Use a imagem buildada a partir do `docker/mock.Dockerfile` deste repositório — os mappings já ficam embutidos na imagem (`COPY wiremock /home/wiremock`), sem precisar de bind-mount:

```bash
docker build -f docker/mock.Dockerfile -t event-service-mock .

docker run -d --name wiremock-event-service -p 8084:8080 event-service-mock
```

Isso expõe o mock em `http://localhost:8084` (porta padrão utilizada no projeto para o EventService). Depois, basta apontar a variável de URL do EventService do serviço/collection sendo testado para esse endereço e enviar o header `X-Mock-Scenario` desejado.