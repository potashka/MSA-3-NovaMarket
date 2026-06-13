# MSA-3-NovaMarket

NovaMarket — e-commerce marketplace, проектируется микросервисная событийная архитектура с Kubernetes, NGINX API Gateway, внешним платежным шлюзом и логистическим провайдером.

## Цель проекта

Подготовить проектную работу по микросервисной архитектуре NovaMarket: описать предметную область, спроектировать архитектуру, подготовить инфраструктурные артефакты и собрать подтверждения работы отдельных компонентов.

## Содержимое

- `Task1` — event-driven architecture, C4 C2 diagram, Saga events registry, sequence diagrams.
- `Task2` — updated architecture for order history, ADR with CQRS/API Composition/Event Sourcing comparison.
- `Task3` — Kubernetes manifests for Deployment, Service, HPA, Locust test and verification commands.
- `Task4` — NGINX rate limiter config and NGINX circuit breaker config.

## Диаграммы

Диаграммы проекта представлены в формате PlantUML.

## Evidence

Скриншоты и логи HPA нужно положить в `Task3/evidence`.
