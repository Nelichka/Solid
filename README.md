#### ВАЖНЫЕ ОШИБКИ С ТОЧКИ ЗРЕНИЯ ШАБЛОНОВ ПРОЕКТИРОВАНИЯ:
1. Нарушен принцип единой ответственности, т.е 
каждый класс должен отвечать только за 1 часть 
функциональности программы. 
Следовательно, выделяем отдельный класс Basket, тем самым разделяем функции и для корзины, 
и для покупки одного товара, то есть 
в классе Purchase будет одна покупка, а в классе Basket - набор покупок.
А в классе Main заменили Purchase purchase = new Purchase(); на
   Basket purchase = new Basket(products);, т.к. нужно создавать корзину, 
а не одну покупку.
2. Размер массива Purchases объявлен через число, что ограничивает количество товара
до 4-х, в данном случае, и это не хорошо, потому что всегда работали бы с 4-мя товарами.
Поэтому, должно было быть объявление через размер МАПы ассортимента товаров.
А в классе Main передаем Map не в sum(), а в Basket(), то есть вот сюда:
Basket purchase = new Basket(products);

# Задача №1. (обязательная)

Перед вами код программы, напоминающий по функционалу домашнее задание на одномерные массивы про магазин.

```
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        HashMap<String, Integer> products = new HashMap<>();
        products.put("Хлеб", 56);
        products.put("Масло", 153);
        products.put("Колбаса", 211);
        products.put("Пирожок", 45);

        System.out.println("В МАГАЗИНЕ В НАЛИЧИИ");
        for (Map.Entry<String, Integer> productAndPrice : products.entrySet()) {
            System.out.println(productAndPrice.getKey() + " за " + productAndPrice.getValue() + " руб./шт.");
        }

        System.out.println("Введите два слова: название товара и количество. Или end");
        Scanner scanner = new Scanner(System.in);
        Purchase purchase = new Purchase();
        while (true) {
            String line = scanner.nextLine();
            if ("end".equals(line)) break;
            String[] parts = line.split(" ");
            String product = parts[0];
            int count = Integer.parseInt(parts[1]);
            purchase.addPurchase(product, count);
        }
        long sum = purchase.sum(products);
        System.out.println("ИТОГО: " + sum);
    }
}
```
и вспомогательный класс:

```
import java.util.Map;

public class Purchase {
protected String title;
protected int count;
protected Purchase[] purchases = new Purchase[4];

    public Purchase(String title, int count) {
        this.title = title;
        this.count = count;
    }

    public Purchase() {
    }

    public void addPurchase(String title, int count) {
        for (int i = 0; i < purchases.length; i++) {
            if (purchases[i] == null) {
                purchases[i] = new Purchase(title, count);
                return;
            }
            if (purchases[i].title.equals(title)) {
                purchases[i].count += count;
                return;
            }
        }
    }

    public long sum(Map<String, Integer> prices) {
        long sum = 0;
        System.out.println("КОРЗИНА:");
        for (int i = 0; i < purchases.length; i++) {
            Purchase purchase = purchases[i];
            if (purchase == null) continue;
            System.out.println("\t" + purchase.title + " " + purchase.count + " шт. в сумме " + (purchase.count * prices.get(purchase.title)) + " руб.");
            sum += purchase.count * prices.get(purchase.title);
        }
        return sum;
    }
}
```
Как можно заметить, отличие прежде всего в том, что пользователь теперь вводит не номер товара, а его название, а также что в реализации используется хешмапа. Этот код специально напиан с нарушением некоторых базовых принципов, рассмотренных на занятии.

Ваша задача - понять чужой код без подробных комментариев, найти эти нарушения и исправить. Это то, что часто встречается в задачах в коммерческой разработке.

В качестве решения должна быть ссылка на репозиторий с двумя (или более) классами в исправленном варианте.

ВАЖНО: Кроме этого в комментарии к решению следует описать, какие конкретно были нарушения, где находились и в чём состояло ваше исправление. Переписать всё с нуля или полностью изменить логику реализации программы считаться исправлением недочётов существующей программы не будет.