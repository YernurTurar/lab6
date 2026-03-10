import java.util.*;

interface PaymentStrategy {
    void pay(double amount);
}

class CreditCardPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Оплата " + amount + " через банковскую карту");
    }
}

class PayPalPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Оплата " + amount + " через PayPal");
    }
}

class CryptoPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Оплата " + amount + " криптовалютой");
    }
}

class PaymentContext {
    private PaymentStrategy strategy;

    public void setStrategy(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void executePayment(double amount) {
        strategy.pay(amount);
    }
}

interface Observer {
    void update(String message);
}

interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers();
}

class CurrencyExchange implements Subject {

    private List<Observer> observers = new ArrayList<>();
    private String rate;

    public void attach(Observer observer) {
        observers.add(observer);
    }

    public void detach(Observer observer) {
        observers.remove(observer);
    }

    public void setRate(String rate) {
        this.rate = rate;
        notifyObservers();
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(rate);
        }
    }
}

class BankObserver implements Observer {
    public void update(String message) {
        System.out.println("Банк получил курс: " + message);
    }
}

class TraderObserver implements Observer {
    public void update(String message) {
        System.out.println("Трейдер реагирует на курс: " + message);
    }
}

class MobileAppObserver implements Observer {
    public void update(String message) {
        System.out.println("Приложение обновило курс: " + message);
    }
}

public class Main {

    public static void main(String[] args) {

        PaymentContext payment = new PaymentContext();

        payment.setStrategy(new CreditCardPayment());
        payment.executePayment(100);

        payment.setStrategy(new PayPalPayment());
        payment.executePayment(200);

        payment.setStrategy(new CryptoPayment());
        payment.executePayment(300);

        System.out.println("-----");

        CurrencyExchange exchange = new CurrencyExchange();

        Observer bank = new BankObserver();
        Observer trader = new TraderObserver();
        Observer app = new MobileAppObserver();

        exchange.attach(bank);
        exchange.attach(trader);
        exchange.attach(app);

        exchange.setRate("USD = 500 KZT");

        exchange.detach(trader);

        exchange.setRate("USD = 510 KZT");
    }
}
