# ai-agent-runs-pool

Пул ранов GitHub Actions для [ai-agent-runner](https://github.com/trained-assist/ai-agent-runner) (экспериментальный аккаунт).

## Модель

```
триггер (issue label / repository_dispatch / ручной)
   → endpoint-приёмник (хвост CF-воркера, только JSON ≤ КБ)
      → dispatch СЮДА (этот репозиторий)
         → джоба: submit в Serverless API → SSE events в лог → result
            → большие данные НИКОГДА не идут через endpoint/API:
              прямая загрузка клиент⇄object storage, в API — только
              ссылка с TTL-токеном и sha256 (share-by-link, slice D1)
```

## Правила

- **Никаких гигабайтов через API/endpoint** — только метаданные и подписанные ссылки (GCS presigned / token-URL).
- Секреты — только в GitHub Secrets / GCP Secret Manager; в код и логи — никогда.
- Продуктовый код живёт в trained-assist/ai-agent-runner; здесь — только оркестрация джоб.

## Статус

Бутстрап. Легаси-эксперимент этого аккаунта (`gha-cluster-*`, `gha-worker-*`) — воркфлоу отключены 01.10.2026.
