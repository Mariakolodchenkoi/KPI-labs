## 1. EARS-вимоги

- **REQ-01** (Event): WHEN Guest або Admin надсилає правильні email і пароль, система SHALL автентифікувати його й надати доступ відповідно до його ролі. - UC2
- **REQ-02** (Unwanted): IF Guest намагається оформити замовлення без вказування адреси доставки, THEN система SHALL відхилити створення замовлення й показати повідомлення про помилку. - UC5
- **REQ-03** (Event): WHEN Authenticated user додає до замовлення товар, який вже є в ньому, система SHALL збільшити quantity в існуючому OrderItem замість створення дубліката. - UC4, UC5
- **REQ-04** (Event): WHEN Authenticated user ініціює оплату замовлення, система SHALL передати Payment service суму Order.totalAmount. - UC5, UC7
- **REQ-05** (Event): WHEN Payment service повертає статус SUCCESS, система SHALL змінити стан замовлення на PAID та очистити кошик користувача. - UC5, UC7
- **REQ-06** (Ubiquitous): Система SHALL не допускати створення Order або OrderItem, що посилаються на неіснуючих User чи Product. - UC5