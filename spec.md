# spec for data model: Online clothing store

## 1. Intention
Розробити модель даних для онлайн магазину одягу, яка підтримує управління замовленням та зберіганням історії

## 2. Entities and attributes

1. **Customer**
- `id` *[PK, string]*: ідентифікатор
- `email` *[string]*: електронна пошта
- `fullName` *[string]*: ПІБ клієнта
- `phoneNumber` *[string]*: номер телефону
- `createdAt` *[date]*: дата реєстрації

2. **Category**
- `id` *[PK, string]*: ідентифікатор
- `name` *[string]*: назва каьегорії (штани, футболки, верхній одяг...)
- `description` *[string]*: опис категорії

3. **Product**
- `id` *[PK, string]*: ідентифікатор
- `categoryId` *[FK -> Category.id, string]*: посилання на категорію
- `title` *[string]*: назва товару
- `description` *[string]*: текстовий опис товару
- `price` *[number]*: вартість

4. **Order**
- `id` *[PK, string]*: ідентифікатор
- `customerId` *[FK -> Customer.id, string]*: покупець
- `totalAmount` *[number]*: вся сума
- `status` *[string]*: статус товару (`Expected` `Cancelled` `Delivered`)
- `createdAt` *[date]*: дата оформлення

5. **OrderItem**
- `id` *[PK, string]*: ідентифікатор
- `orderId` *[FK -> Order.id, string]*: замовлення
- `productId` *[FK -> Product.id, string]*: продукт
- `quantity` *[number]*: кількість одиниць товару
- `unitPrice` *[number]*: ціна


## 3. Зв'язки

- `Category` 1 to N `Product`: В категор11 може міститися багато товарів, але товар належить лише одній категорії
- `Customer` 1 to N `Order`: Один покупець може зробити багато замовлень
- `Order` 1 to N `OrderTtem`: Одне замовлення містить багато позицій у чеку
- `Product` 1 to N `OrderItem`: Один товар може фігурувати в багатьох позиціях замовлень

## 4. Критерії прийняття
- Товари прив'язуються до чека через `OrderItem`, без прямого зв'язку "many-to-many"
- Ціна товару фіксується в `OrderItem.unitPrice` на момент оплати, щоб стара ціна замовлень не змінювалоась після оновлення каталогу
- Усі зв'язки між сутностями підтримують цілісність через зовнішні ключі (РК)
