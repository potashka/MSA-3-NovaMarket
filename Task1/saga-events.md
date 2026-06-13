# Saga Events Registry

Реестр событий описывает хореографию оформления заказа в NovaMarket. Сервисы взаимодействуют через Apache Kafka: каждый сервис публикует результат своего шага, а остальные сервисы подписываются на события, необходимые для продолжения процесса или компенсации.

| Этап | Тип события: domain / failure / compensation | Название события | Источник события | Потребители события | Краткое описание |
| --- | --- | --- | --- | --- | --- |
| 1. Создание заказа | domain | `OrderCreated` | Order Service | Inventory Service, Notification Service | Заказ создан после подтверждения покупателем. Событие запускает резервирование товаров. |
| 2. Резервирование товаров | domain | `InventoryReserved` | Inventory Service | Payment Service, Order Service | Товары успешно зарезервированы для заказа. Событие разрешает переход к оплате. |
| 2. Ошибка резервирования | failure | `InventoryReservationFailed` | Inventory Service | Order Service, Notification Service | Товары недоступны или резервирование не выполнено. Заказ должен быть отменён. |
| 3. Оплата заказа | domain | `PaymentSucceeded` | Payment Service | Delivery Service, Order Service, Seller Service, Notification Service | Деньги успешно списаны через внешний платёжный шлюз. Событие запускает создание доставки и уведомление продавца. |
| 3. Ошибка оплаты | failure | `PaymentFailed` | Payment Service | Inventory Service, Order Service, Notification Service | Оплата не прошла. Требуется снять резерв товаров и отменить заказ. |
| 3. Снятие резерва после ошибки | compensation | `InventoryReservationReleased` | Inventory Service | Order Service, Notification Service | Резерв товаров снят после ошибки оплаты, отмены заказа или ошибки создания доставки. |
| 4. Создание доставки | domain | `DeliveryRequestCreated` | Delivery Service | Order Service, Notification Service | Заявка на доставку успешно создана у внешнего логистического провайдера. |
| 4. Ошибка создания доставки | failure | `DeliveryCreationFailed` | Delivery Service | Payment Service, Inventory Service, Order Service, Notification Service | Доставка не создана после успешной оплаты. Требуется возврат средств, снятие резерва и отмена заказа. |
| 5. Возврат средств | compensation | `RefundSucceeded` | Payment Service | Inventory Service, Order Service, Notification Service | Возврат средств успешно выполнен после ошибки создания доставки. |
| 5. Ошибка возврата средств | failure | `RefundFailed` | Payment Service | Order Service, Notification Service | Возврат средств не выполнен. Требуется ручная проверка или повторная компенсация. |
| 6. Подтверждение заказа | domain | `OrderConfirmed` | Order Service | Notification Service, Seller Service, Review Service | Заказ подтверждён после успешной оплаты и создания доставки. |
| 6. Отмена заказа | compensation | `OrderCancelled` | Order Service | Notification Service, Seller Service, Inventory Service | Заказ отменён из-за недоступности товаров, ошибки оплаты или ошибки доставки после компенсации. |
| 7. Уведомление продавца | domain | `SellerNotificationSent` | Notification Service | Seller Service, Order Service | Продавцу отправлено уведомление о новом оплаченном или отменённом заказе. |
| 7. Уведомление покупателя | domain | `CustomerNotificationSent` | Notification Service | Order Service | Покупателю отправлено уведомление о статусе заказа, ошибке или компенсации. |
