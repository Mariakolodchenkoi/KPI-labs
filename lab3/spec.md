# UC5: Оформлювати та оплачувати замовлення

## Учасники
- **Customer (`Authenticated user`)** — авторизований покупець, який оформлює та оплачує замовлення
- **CheckoutController** — обробляє запит на оформлення та оплату замовлення
- **OrderService** — керує сутностями `Order` (`id`, `customerId`, `totalAmount`, `status`, `createdAt`) та `OrderItem` (`id`, `orderId`, `productId`, `quantity`, `unitPrice`)
- **ProductService** — перевіряє існування й актуальні дані товару (`Product`: `id`, `categoryId`, `title`, `description`, `price`)
- **PaymentService** — взаємодіє з платіжним сервісом і фіксує спробу оплати (`Payment`: `id`, `orderId`, `amount`, `status`, `createdAt`)


## Сценарії UC5 (із 2 лаби)

### S1: Успішне оформлення та оплата замовлення
1. `Authenticated user` перебуває на сторінці кошика з товарами
2. `Authenticated user` натискає «Оформити замовлення», вводить адресу доставки та підтверджує оплату
3. Система перевіряє коректність адреси доставки та наявність позицій.
4. Система звіряє позиції з даними `Product` і формує записи `OrderItem` (без створення дублікатів)
5. Система розраховує `Order.totalAmount` та передає суму в `Payment service`
6. `Payment service` повертає статус `SUCCESS`
7. Система фіксує запис у `Payment`, створює/оновлює замовлення в `Order` зі статусом `PAID`, застосовує безкоштовну доставку та вилучає товари з кошика `Authenticated user`

### S2: Оформлення замовлення після зміни кількості товарів
1. `Authenticated user` перебуває на сторінці кошика
2. `Authenticated user` змінює `quantity` обраного товару та натискає «Оформити замовлення»
3. Система оновлює `quantity` у відповідному `OrderItem` та перераховує підсумкову суму `Order.totalAmount`
4. `Authenticated user` підтверджує оплату через `Payment service`
5. Система передає оновлену суму `Order.totalAmount` у `Payment service`
6. Після успішної транзакції (`SUCCESS`) фіксується `Payment` і створюється замовлення зі статусом `PAID`

### S3: Неуспішна спроба оплати замовлення
1. `Authenticated user` перебуває на сторінці оформлення замовлення
2. `Authenticated user` вводить дані картки та підтверджує платіж
3. Система передає `Order.totalAmount` у `Payment service`
4. `Payment service` повертає статус `DECLINED` (недостатньо коштів або помилка банку)
5. Система фіксує спробу оплати в `Payment` і зберігає замовлення в таблиці `Order` зі статусом `PENDING_PAYMENT`
6. Система відображає повідомлення «Оплату відхилено», а товари залишаються у кошику `Authenticated user` для повторної спроби