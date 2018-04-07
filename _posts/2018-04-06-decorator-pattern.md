---
layout: post
title: Tìm hiểu mẫu thiết kế Decorator Pattern.
tags: [OOP, Design Patterns]
---

Đôi khi chúng ta cần mở rộng một phương thức trong đối tượng, và cách thông thường là chúng ta sẽ kế thừa đối tượng đó. Việc này không phải sai, nhưng trong một vài trường hợp sẽ làm cho mã nguồn trở lên phức tạp hơn chúng ta mong muốn. Đó là lý do chính cho việc ra đời của mẫu thiết kế Decorator, là một cách để mở rộng các phương thức một cách linh động.

## 1. Vấn đề

Khi chúng ta thực hiện chức năng soạn nội dung mail. Thường chúng ta sẽ  tạo 1 class với đầy đủ header, body, footer. Khi cần thay đổi nôi dung mail thường thi chúng ta sẽ tạo một class khác đẻ kế thừa và override lai ví dụ như Class christmasEmail Khi tới một sự kiện khác như Happy New Year chúng ta lại cần một nôi dung mail mới và ta làm điều tương tự  với newYearEmail . Vấn đề sẽ xảy ra khi ta cần gửi cả 2 nôi dung trong 1 mail.

## 2. Giải pháp

Decorator pattern (còn được gọi là Wrapper), là một design pattern cho phép các hành vi được thêm vào một đối tượng cụ thể, tĩnh hoặc động, mà không ảnh hưởng đến các hành vi của các đối tượng khác trong cùng một class.

## 3. Những thành phần trong mẫu thiết kế Decorator:

![Crepe](https://viblo.asia/uploads/77b01d67-bfb6-49bc-9c89-13315d2905c5.png)

+ **Component**: giao diện (interface) chung để các đối tượng cần thêm chức năng trong quá trình chạy thì triển khai giao diện này.
+ **ConcreteComponent** : Một cài đặt cho giao diện Component mà nó định nghĩa một đối tượng cần thêm các chức năng trong quá trình chạy.
+ **Decorator** : một lớp trừu tượng dùng để duy trì một tham chiếu của đối tượng thành phần và đồng thời cài đặt các thành phần của giao diện.
+ **ConcreteDecorator** : Một cài đặt của Decorator, nó cài đặt thêm các thành phần vào đầu của các đối tượng thành phần.

## 4. Áp dụng

![Crepe](https://viblo.asia/uploads/115fdc5b-cb03-4b1a-95a9-9dada612c7ce.png)

Giao diện IPizza là thành phần Component trong mẫu thiết kế Decorator, nó chứa phương thức doPizza, đây là phương thức dùng để tạo ra một pizza phù hợp.

```java
/**
 * This is our component interface
 * It has doPizza() method that provides a Pizza cake.
 * Created by TienDQ on 1/28/16.
 */

public interface IPizza {
    String doPizza();
}
```

TomatoPizza và ChickenPizza là những cài đặt, triển khai của IPizza. Chúng cung cấp cụ thể các thể hiện của lớp mà chúng ta cẩn mở rộng trong quá trình chương trình đang chạy.

```java
/**
 * Created by TienDQ on 1/28/16.
 */
public class TomatoPizza implements IPizza {

    @Override
    public String doPizza() {
        return "I am a Tomato Pizza";
    }
}
```

```java
/**
 * Created by TienDQ on 1/28/16.
 */
public class ChickenPizza implements IPizza {

    @Override
    public String doPizza() {
        return "I am a Chicken Pizza";
    }
}
```

PizzaDecorator là trái tim của sơ đồ thiết kế trên. Nó giữ một thể hiện đã tồn tại của pizza như TomatoPizza hoặc ChickenPizza. Thuộc tính này sẽ được cài đặt thông qua phương thức cởi tạo, và nó được mở rộng trong khi chương trình chạy.

```java
/**
 * This is our abstract Decorator class It maintains a reference to IPizza
 * instance that is object need additional functionalities at runtime.
 *
 * Created by TienDQ on 1/28/16.
 */

public abstract class PizzaDecorator implements IPizza {
    // Reference to component object
    protected IPizza mPizza;

    // We initialize a Decorator with existing pizza we need decorate
    public PizzaDecorator(IPizza pizza) {
        mPizza = pizza;
    }

    public IPizza getPizza() {
        return mPizza;
    }

    public void setPizza(IPizza mPizza) {
        this.mPizza = mPizza;
    }
}
```

PepperDecorator và CheeseDecorator cài đặt các phương thức mở rộng, trong trường hợp ví dụ này, PepperDecorator sẽ thêm hồ tiêu vào một pizza đã có. Tính năng mở rộng là được cài đặt trong phương thức addPepper().

```java
/**
 * This is class that extends an Pizza by adding cheese at runtime.
 * Created by TienDQ on 1/28/16.
 */
public class CheeseDecorator extends PizzaDecorator {

    public CheeseDecorator(IPizza pizza) {
        super(pizza);
    }

    @Override
    public String doPizza() {
        String type = mPizza.doPizza();
        return type + addCheese();
    }

    // This is additional functionality
    // It adds cheese to an existing pizza
    private String addCheese() {
        return "+ Cheese";
    }
}
```

```java
/**
 * This class extends an Pizza by adding pepper to an existing one.
 *
 * Created by TienDQ on 1/28/16.
 */
public class PepperDecorator extends PizzaDecorator {

    public PepperDecorator(IPizza pizza) {
        super(pizza);
    }

    @Override
    public String doPizza() {
        String type = mPizza.doPizza();
        return type + addPepper();
    }

    // This is our additional functionality
    // It add pepper to existing pizza at runtime
    private String addPepper() {
        return "+ Pepper";
    }
}
```

Và cuối cùng chúng ta cần viết lớp để chạy các cài đặt ở bên trên là lớp PizzaShop

```java
/**
 * Created by TienDQ on 1/28/16.
 */
public class PizzaShop {

    public static void main(String[] args) {
        IPizza tomato = new TomatoPizza();
        IPizza chicken = new ChickenPizza();

        System.out.println(tomato.doPizza());
        System.out.println(chicken.doPizza());

        // Use Decorator pattern to extend existing pizza dynamically

        // Add pepper to tomato-pizza
        PepperDecorator pepperDecorator = new PepperDecorator(tomato);
        System.out.println(pepperDecorator.doPizza());

        // Add cheese to tomato-pizza
        CheeseDecorator cheeseDecorator = new CheeseDecorator(tomato);
        System.out.println(cheeseDecorator.doPizza());

        // Add cheese and pepper to tomato-pizza
        // We combine functionalities together easily.
        CheeseDecorator cheeseDecorator2 = new CheeseDecorator(pepperDecorator);
        System.out.println(cheeseDecorator2.doPizza());
    }
}
```

Và kết quả chúng ta thu được khi chạy chương trình:

```
I am a Tomato Pizza
I am a Chicken Pizza
I am a Tomato Pizza+ Pepper
I am a Tomato Pizza+ Cheese
I am a Tomato Pizza+ Pepper+ Cheese
```
Đó là tất cả những gì cần biết về mẫu thiết kế Decorator. Trong thế giới lập trình, mẫu thiết kế này được sự dụng rất nhiều. Và đặc biệt là khi làm việc với stream trong Java.

Nguồn: [viblo.asia](https://viblo.asia/p/hieu-biet-co-ban-ve-decorator-pattern-pVYRPjbVG4ng)



