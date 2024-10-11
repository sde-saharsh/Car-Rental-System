import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Car {
    private String carId;
    private String brand;
    private String model;
    private double basePricePerDay;
    private boolean isAvailable;

    public Car(String carId, String brand, String model, double basePricePerDay) {
        this.carId = carId;
        this.brand = brand;
        this.model = model;
        this.basePricePerDay = basePricePerDay;
        this.isAvailable = true;
    }

    public String getCarId() {
        return carId;
    }

    public String getBrand() {
        return brand;
    }

    public String getModel() {
        return model;
    }

    public double calculatePrice(int rentalDays) {
        return basePricePerDay * rentalDays;
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void rent() {
        isAvailable = false;
    }

    public void returnCar() {
        isAvailable = true; // Fixing the availability when car is returned
    }
}

class Customer {
    private String name;
    private int customerId;

    public Customer(int customerId, String name) {
        this.customerId = customerId;
        this.name = name;
    }

    public int getCustomerId() {
        return customerId;
    }

    public String getName() {
        return name;
    }
}

class Rental {
    private Car car;
    private Customer customer;
    private int days;

    public Rental(Car car, int days, Customer customer) {
        this.car = car;
        this.days = days;
        this.customer = customer;
    }

    public Car getCar() {
        return car;
    }

    public Customer getCustomer() {
        return customer;
    }

    public int getDays() {
        return days;
    }
}

class CarRentalSystem {
    private List<Car> cars;
    private List<Customer> customers;
    private List<Rental> rentals;

    public CarRentalSystem() {
        cars = new ArrayList<>();
        customers = new ArrayList<>();
        rentals = new ArrayList<>();
    }

    public void addCar(Car car) {
        cars.add(car);
    }

    public void addCustomer(Customer customer) {
        customers.add(customer);
    }

    public void rentCar(Car car, Customer customer, int days) {
        if (car.isAvailable()) {
            rentals.add(new Rental(car, days, customer));
            car.rent();
        } else {
            System.out.println("Car is not available");
        }
    }

    public void returnCar(Car car) {
        car.returnCar();
        Rental rentalToRemove = null;
        for (Rental rental : rentals) {
            if (rental.getCar() == car) {
                rentalToRemove = rental;
                break;
            }
        }
        if (rentalToRemove != null) {
            rentals.remove(rentalToRemove);
            System.out.println("Car returned");
        } else {
            System.out.println("Car not found in rentals");
        }
    }

    public void menu() {
        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("==== Welcome To Saharsh's Project ===");
            System.out.println("1. Rent car");
            System.out.println("2. Return Car");
            System.out.println("3. Exit");
            System.out.println("Enter your choice: ");
            int choice = sc.nextInt();
            sc.nextLine();

            if (choice == 1) {
                System.out.println("== Rent a car ==");
                System.out.print("Enter your name: ");
                String customerName = sc.nextLine();

                System.out.println("== Available Cars ==");
                for (Car car : cars) {
                    if (car.isAvailable()) {
                        System.out.println(car.getCarId() + "  " + car.getModel() + "  " + car.getBrand());
                    }
                }

                System.out.print("Enter the car Id you want to rent: ");
                String carId = sc.nextLine();
                System.out.println("Enter the number of days you want to rent: ");
                int rentalDays = sc.nextInt();
                sc.nextLine();

                Customer newCustomer = new Customer(customers.size() + 1, customerName);
                addCustomer(newCustomer);

                Car selectedCar = null;
                for (Car car : cars) {
                    if (car.isAvailable() && car.getCarId().equals(carId)) {
                        selectedCar = car;
                        break;
                    }
                }

                if (selectedCar != null) {
                    double totalPrice = selectedCar.calculatePrice(rentalDays);
                    System.out.println("== Rental Information ==");
                    System.out.println("Customer Id: " + "CUS0"+newCustomer.getCustomerId());
                    System.out.println("Customer Name: " + newCustomer.getName());
                    System.out.println("Car: " + selectedCar.getBrand() + " " + selectedCar.getModel());
                    System.out.println("Rental Days: " + rentalDays);
                    System.out.println("Total Price: " + totalPrice);
                    System.out.println("Confirm rental (yes/no)?");
                    String confirm = sc.next();
                    if (confirm.equalsIgnoreCase("yes")) {
                        rentCar(selectedCar, newCustomer, rentalDays);
                        System.out.println("Car Rented");
                    } else {
                        System.out.println("Car Not Rented");
                    }
                } else {
                    System.out.println("Invalid car selection");
                }
            } else if (choice == 2) {
                System.out.println("== Return Car ==");
                System.out.print("Enter the car Id you want to return: ");
                String carId = sc.nextLine();
                Car carToReturn = null;
                for (Car car : cars) {
                    if (!car.isAvailable() && car.getCarId().equals(carId)) {
                        carToReturn = car;
                        break;
                    }
                }
                if (carToReturn != null) {
                    returnCar(carToReturn);
                    System.out.println("Car Returned");
                } else {
                    System.out.println("Car was not rented");
                }
            } else if (choice == 3) {
                break;
            } else {
                System.out.println("Invalid choice");
            }
        }
        System.out.println("== Thanks for Visiting ==");
    }
}

public class Main {
    public static void main(String[] args) {
        CarRentalSystem rentalSystem = new CarRentalSystem();

        // Adding some cars to the system
        Car car1 = new Car("C001", "Toyota", "Camry", 60.0);
        Car car2 = new Car("C002", "Honda", "Accord", 50.0);
        Car car3 = new Car("C003", "Tesla", "Model 3", 100.0);

        rentalSystem.addCar(car1);
        rentalSystem.addCar(car2);
        rentalSystem.addCar(car3);

        rentalSystem.menu();
    }
}
