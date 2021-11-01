# 20211101 Today I Learned

# ☀️ 11월의 시작!

## 🎯 오늘의 목표

- [x]  Strategy 패턴 공부
- [ ]  slf4j 공부 후 정리
- [ ]  로그인 기능 개발

# 📖 오늘 무슨 일을 했나?

Strategy 패턴을 공부했다. 요즘 들어 늘어지고있다. 열심히 하자!!!!

# 🚀 오늘의 산출물

## 📖Strategy Pattern이란?

행동 패턴 중 하나로 **프로그램 실행 중에** 필요한 알고리즘을 선택 할 수 있게 해주는 디자인 패턴이다. 전략 패턴을 사용함으로써 사용하는 객체와 알고리즘을 분리 할 수 있게 된다. 

## 📗UML Diagram

![UML1.jpg](20211101%20Today%20I%20Learned%204c524c1f3cc348c4a4d022757811a92f/UML1.jpg)

UML DIAGRAM  from - [https://en.wikipedia.org/wiki/Strategy_pattern](https://en.wikipedia.org/wiki/Strategy_pattern)

 위의 UML을 보면 구현 클래스인 Context가 Strategy를 구현(implements)가 아니라 단순히 의존 화살표로 연결되어 있는 것을 볼 수 있다.  Context가 사용하는 Strategy를 바로 구현하는게 아니라 Strategy Interface를 '**참조'**만 함으로써, 알고리즘의 구현과 알고리즘을 사용하는 Context를 분리하는 것이다.

![11.png](20211101%20Today%20I%20Learned%204c524c1f3cc348c4a4d022757811a92f/11.png)

from - [https://en.wikipedia.org/wiki/Strategy_pattern](https://en.wikipedia.org/wiki/Strategy_pattern) class diagram

## 🚀 구현

```java
import java.util.ArrayList;
import java.util.List;

interface BillingStrategy {
    // use a price in cents to avoid floating point round-off error
    int getActPrice(int rawPrice);

    // Normal billing strategy (unchanged price)
    static BillingStrategy normalStrategy() {
        return rawPrice -> rawPrice;
    }

    // strategy for happy hour (50% discount)
    static BillingStrategy happyHourStrategy() {
        return rawPrice -> rawPrice / 2;
    }
}

class CustomerBill {
    private final List<Integer> drinks = new ArrayList<>();
    private BillingStrategy strategy;

    public CustomerBill(BillingStrategy strategy) {
        this.strategy = strategy;
    }

    public void add(int price, int quantity) {
        this.drinks.add(this.strategy.getActPrice(price*quantity));
    }

    // Payment of bill
    public void print() {
        int sum = this.drinks.stream().mapToInt(v -> v).sum();
        System.out.println("total due: " + sum);
        this.drinks.clear();
    }

    public void setStrategy(BillingStrategy strategy) {
        this.strategy = strategy;
    }
}

public class strategy {

    public static void main(String[] args) {
        // prepare strategy
        BillingStrategy normalStrategy = BillingStrategy.normalStrategy();
        BillingStrategy happyHourStrategy = BillingStrategy.happyHourStrategy();

        CustomerBill firstCustomer = new CustomerBill(normalStrategy);

        // Normal billing
        firstCustomer.add(100, 1);

        // Start happy hour
        firstCustomer.setStrategy(happyHourStrategy);
        firstCustomer.add(100, 2);

        // New Customer
        CustomerBill secondCustomer = new CustomerBill(happyHourStrategy);
        secondCustomer.add(80, 1);

        // the customer pays
        firstCustomer.print();

        // end Happy hour
        secondCustomer.setStrategy(normalStrategy);
        secondCustomer.add(130, 2);
        secondCustomer.add(25, 1);
        secondCustomer.print();
    }
}
```

![output.PNG](20211101%20Today%20I%20Learned%204c524c1f3cc348c4a4d022757811a92f/output.png)

output

아직 좀 긴가민가하다 좀 더 정리해서 써야할듯

[Strategy pattern - Wikipedia](https://en.wikipedia.org/wiki/Strategy_pattern)