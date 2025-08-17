# Домашнее задание «Основные конструкции языка GO»  

## Инструкция к выполнению
Изучите предоставленный шаблон кода
· Ознакомьтесь с структурой кода и комментариями для понимания различных способов объявления переменных и констант: go.dev...6enuFc9esr

## Дополните программу своими функциями
· Создайте функцию addNewItem, которая будет добавлять новый товар и возвращать его ID.
· Создайте функцию calculateDiscount, которая будет рассчитывать скидку на товар.

Сделайте копию шаблона для сдачи домашнего задания и сохраните в нёё получившийся код. Предоставьте доступ на комментирование всем, у кого есть ссылка.

Рекомендации:
· Обратите особое внимание на различные способы объявления переменных.
· Помните о разнице между var и оператором :=.
· Константы в Go объявляются с помощью const.
· Обращайте внимание на области видимости переменных и констант.
· Используйте говорящие имена для переменных и функций.
· Используйте пакет fmt для:
o отображения информации о товарах в удобочитаемом формате;
o форматирования числовых значений (цены, веса) с нужной точностью;
o вывода предупреждений и уведомлений;
o отображения дат в удобном для пользователя формате.


 
[Cсылка на песочницу](https://go.dev/play/p/nbetTVMlV3J)

 ```go

/*
=======================================================
ДОМАШНЕЕ ЗАДАНИЕ ПО GO
=======================================================
Автор: Бызгаев Александр
Курс: Нетология - Основы языка Go
Тема: Типы данных в Go
Дата: Август 2025

Описание: Программа для управления небольшим складом товаров
Демонстрирует работу с различными типами данных, константами,
переменными и функциями в языке Go.
=======================================================
*/

// Программа для управления небольшим складом товаров
package main

import (
	"fmt"
	"strings"
	"time"
)

// Глобальные константы для категорий товаров
const (
	CategoryElectronics = "Электроника"
	CategoryFood        = "Продукты"
	CategoryClothes     = "Одежда"
	MaxItems            = 100 // Максимальное количество товаров на складе
)

// Глобальная переменная для подсчета товаров
var totalItems int
var nextItemID int64 = 1000 // Счетчик для генерации ID товаров

func main() {
	// Вывод информации об авторе
	fmt.Println(strings.Repeat("=", 60))
	fmt.Println("СИСТЕМА УПРАВЛЕНИЯ СКЛАДОМ ТОВАРОВ")
	fmt.Println("Домашнее задание по Go")
	fmt.Println("Автор: Бызгаев Александр")
	fmt.Println("Курс: Нетология - Основы языка Go")
	fmt.Println(strings.Repeat("=", 60))
	fmt.Println()

	// Объявление и инициализация переменных разными способами

	// Способ 1: Объявление переменной с явным указанием типа и последующим присваиванием
	var itemName string
	itemName = "Смартфон"

	// Способ 2: Объявление переменной с явным указанием типа и инициализацией
	var itemPrice float64 = 999.99

	// Способ 3: Объявление нескольких переменных одного типа
	var minQuantity, maxQuantity int = 5, 20

	// Способ 4: Объявление переменной без явного указания типа (тип определяется компилятором)
	var isAvailable = true

	// Способ 5: Короткое объявление переменной (только внутри функций)
	quantity := 15

	// Способ 6: Объявление нескольких переменных разных типов одновременно
	var (
		itemID     int64     = 12345
		itemColor  string    = "Черный"
		itemWeight float32
		dateAdded  time.Time = time.Now()
	)

	// Присваивание значения ранее объявленной переменной
	itemWeight = 0.3

	// Приведение типов
	percentInStock := float64(quantity) / float64(MaxItems) * 100

	// Использование констант
	category := CategoryElectronics

	// Вызов функции для вывода информации о товаре
	displayItemInfo(itemID, itemName, quantity, itemPrice, isAvailable, category)

	// Демонстрация работы с базовыми типами
	fmt.Println("\nДополнительная информация:")
	fmt.Printf("Цвет товара: %s\n", itemColor)
	fmt.Printf("Вес товара: %.2f кг\n", itemWeight)
	fmt.Printf("Минимальное количество: %d, Максимальное количество: %d\n", minQuantity, maxQuantity)
	fmt.Printf("Процент от максимального количества на складе: %.1f%%\n", percentInStock)
	fmt.Printf("Дата добавления: %s\n", dateAdded.Format("02-01-2006"))

	// Работа с целочисленными константами и их типами
	const (
		Small  = 1
		Medium = 5
		Large  = 10
	)

	// Демонстрация работы с различными числовыми типами
	var (
		shortValue   int8      = 127 // -128 до 127
		intValue     int       = 1000000
		uintValue    uint      = 10000 // только положительные
		floatValue   float32   = 123.456
		complexValue complex64 = 1 + 2i
	)

	fmt.Println("\nРазные числовые типы:")
	fmt.Printf("int8: %d\n", shortValue)
	fmt.Printf("int: %d\n", intValue)
	fmt.Printf("uint: %d\n", uintValue)
	fmt.Printf("float32: %f\n", floatValue)
	fmt.Printf("complex64: %v\n", complexValue)

	// Работа со строками и байтами
	productCode := "ЯЩИК-12345"
	fmt.Printf("\nКод товара: %s, длина: %d символов, длина: %d байт\n", productCode, len([]rune(productCode)), len(productCode))

	// Обновление общего количества товаров
	updateTotalItems(quantity)
	fmt.Printf("\nОбщее количество товаров на складе: %d\n", totalItems)

	// ДОПОЛНИТЕЛЬНЫЙ ФУНКЦИОНАЛ
	fmt.Println("\n" + strings.Repeat("=", 50))
	fmt.Println("ДЕМОНСТРАЦИЯ ДОПОЛНИТЕЛЬНЫХ ФУНКЦИЙ")
	fmt.Println(strings.Repeat("=", 50))

	// Добавление новых товаров
	fmt.Println("\n--- Добавление новых товаров ---")
	newItemID1 := addNewItem("Ноутбук", CategoryElectronics, 1299.99, 8)
	fmt.Printf("Добавлен товар с ID: %d\n", newItemID1)

	newItemID2 := addNewItem("Хлеб", CategoryFood, 45.50, 25)
	fmt.Printf("Добавлен товар с ID: %d\n", newItemID2)

	newItemID3 := addNewItem("Джинсы", CategoryClothes, 2500.00, 12)
	fmt.Printf("Добавлен товар с ID: %d\n", newItemID3)

	// Расчет скидок
	fmt.Println("\n--- Расчет скидок ---")
	originalPrice := 1000.0
	discountPercent := 15.0
	discountedPrice := calculateDiscount(originalPrice, discountPercent)
	fmt.Printf("Цена товара: %.2f руб.\n", originalPrice)
	fmt.Printf("Скидка: %.1f%%\n", discountPercent)
	fmt.Printf("Цена со скидкой: %.2f руб.\n", discountedPrice)
	fmt.Printf("Экономия: %.2f руб.\n", originalPrice-discountedPrice)

	// Расчет скидок для разных товаров
	fmt.Println("\n--- Скидки для разных товаров ---")
	prices := []float64{500.00, 1200.00, 75.00, 3000.00}
	discounts := []float64{10.0, 20.0, 5.0, 25.0}

	for i, price := range prices {
		discounted := calculateDiscount(price, discounts[i])
		fmt.Printf("Товар %d: %.2f руб. -> %.2f руб. (скидка %.1f%%)\n", 
			i+1, price, discounted, discounts[i])
	}

	// Финальная статистика
	fmt.Printf("\n--- Итоговая статистика склада ---\n")
	fmt.Printf("Общее количество товаров: %d из %d максимальных\n", totalItems, MaxItems)
	fmt.Printf("Заполненность склада: %.1f%%\n", float64(totalItems)/float64(MaxItems)*100)
	
	if totalItems >= MaxItems {
		fmt.Println("ВНИМАНИЕ: Склад заполнен!")
	} else {
		fmt.Printf("Можно добавить еще %d товаров\n", MaxItems-totalItems)
	}

	// Завершающая информация
	fmt.Println("\n" + strings.Repeat("=", 60))
	fmt.Println("ПРОГРАММА ЗАВЕРШЕНА УСПЕШНО")
	fmt.Printf("Время выполнения: %s\n", time.Now().Format("02-01-2006 15:04:05"))
	fmt.Println("Автор: Бызгаев Александр")
	fmt.Println(strings.Repeat("=", 60))
}

// Функция для отображения информации о товаре
func displayItemInfo(id int64, name string, qty int, price float64, isAvailable bool, category string) {
	fmt.Println("=== Информация о товаре ===")
	fmt.Printf("ID: %d\n", id)
	fmt.Printf("Название: %s\n", name)
	fmt.Printf("Категория: %s\n", category)
	fmt.Printf("Количество: %d\n", qty)
	fmt.Printf("Цена: %.2f руб.\n", price)

	// Использование условного оператора с логическим типом
	if isAvailable {
		fmt.Println("Статус: В наличии")
	} else {
		fmt.Println("Статус: Нет в наличии")
	}
}

// Функция для обновления общего количества товаров
func updateTotalItems(qty int) {
	totalItems += qty

	// Проверка на превышение максимального количества
	if totalItems > MaxItems {
		fmt.Println("Предупреждение: Превышено максимальное количество товаров на складе!")
		totalItems = MaxItems
	}
}

// ДОПОЛНИТЕЛЬНАЯ ФУНКЦИЯ 1: Добавление нового товара
func addNewItem(name, category string, price float64, quantity int) int64 {
	// Генерируем уникальный ID для товара
	currentID := nextItemID
	nextItemID++

	// Проверяем, не превысим ли лимит склада
	if totalItems+quantity > MaxItems {
		availableSpace := MaxItems - totalItems
		fmt.Printf("Внимание: Можно добавить только %d единиц товара '%s' (ограничение склада)\n", 
			availableSpace, name)
		quantity = availableSpace
	}

	// Обновляем общее количество товаров
	totalItems += quantity

	// Выводим информацию о добавленном товаре
	fmt.Printf("Товар добавлен на склад:\n")
	fmt.Printf("   ID: %d\n", currentID)
	fmt.Printf("   Название: %s\n", name)
	fmt.Printf("   Категория: %s\n", category)
	fmt.Printf("   Цена: %.2f руб.\n", price)
	fmt.Printf("   Количество: %d\n", quantity)
	fmt.Printf("   Дата добавления: %s\n", time.Now().Format("02-01-2006 15:04"))

	return currentID
}

// ДОПОЛНИТЕЛЬНАЯ ФУНКЦИЯ 2: Расчет скидки на товар
func calculateDiscount(originalPrice, discountPercent float64) float64 {
	// Проверяем корректность входных данных
	if originalPrice < 0 {
		fmt.Println("Ошибка: Цена не может быть отрицательной")
		return originalPrice
	}

	if discountPercent < 0 || discountPercent > 100 {
		fmt.Println("Ошибка: Скидка должна быть от 0 до 100%")
		return originalPrice
	}

	// Рассчитываем цену со скидкой
	discountAmount := originalPrice * (discountPercent / 100)
	finalPrice := originalPrice - discountAmount

	// Дополнительная информация о скидке
	if discountPercent >= 50 {
		fmt.Printf("Супер скидка %.1f%%! ", discountPercent)
	} else if discountPercent >= 20 {
		fmt.Printf("Отличная скидка %.1f%%! ", discountPercent)
	} else if discountPercent > 0 {
		fmt.Printf("Скидка %.1f%% ", discountPercent)
	}

	return finalPrice
}


```
