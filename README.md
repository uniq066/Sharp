# Sharp
#include <iostream>
#include <vector>
#include <cmath>
#include <iomanip> // Для std::setprecision

// Абстрактный базовый класс фигуры
class Shape {
protected:
    static int idCounter; // Статический счетчик ID
    int id; // Уникальный ID для каждой фигуры

public:
    Shape() {
        id = ++idCounter; // Генерация уникального ID
    }

    virtual ~Shape() {
        // Деструктор
    }

    virtual double area() const = 0; // Чисто виртуальный метод для вычисления площади
    int getId() const { return id; } // Метод для получения ID
};

int Shape::idCounter = 0; // Инициализация статического члена класса

// Класс круга
class Circle : public Shape {
private:
    double radius; // Радиус круга

public:
    Circle(double r) : radius(r) {}

    double area() const override {
        return M_PI * radius * radius; // Площадь круга
    }

    void display() const {
        std::cout << "Circle ID: " << getId() 
                  << ", Radius: " << radius 
                  << ", Area: " << std::fixed << std::setprecision(2) << area() << std::endl;
    }
};

// Класс сферы
class Sphere : public Circle {
public:
    Sphere(double r) : Circle(r) {}

    double volume() const {
        return (4.0 / 3.0) * M_PI * std::pow(radius, 3); // Объем сферы
    }

    void display() const {
        std::cout << "Sphere ID: " << getId() 
                  << ", Radius: " << radius 
                  << ", Volume: " << std::fixed << std::setprecision(2) << volume() << std::endl;
    }
};

// Класс прямоугольника
class Rectangle : public Shape {
private:
    double width;  // Ширина
    double height; // Высота

public:
    Rectangle(double w, double h) : width(w), height(h) {}

    double area() const override {
        return width * height; // Площадь прямоугольника
    }

    void display() const {
        std::cout << "Rectangle ID: " << getId() 
                  << ", Width: " << width 
                  << ", Height: " << height 
                  << ", Area: " << std::fixed << std::setprecision(2) << area() << std::endl;
    }
};

// Класс кубоида
class Cuboid : public Rectangle {
public:
    Cuboid(double w, double h) : Rectangle(w, h) {}

    double volume() const {
        return area() * height; // Объем кубоида
    }

    void display() const {
        std::cout << "Cuboid ID: " << getId() 
                  << ", Width: " << width 
                  << ", Height: " << height 
                  << ", Volume: " << std::fixed << std::setprecision(2) << volume() << std::endl;
    }
};

// Основная функция
int main() {
    std::vector<Shape*> shapes; // Динамический массив фигур
    int choice;

    do {
        std::cout << "\n1. Добавить круг\n";
        std::cout << "2. Добавить прямоугольник\n";
        std::cout << "3. Добавить сферу\n";
        std::cout << "4. Добавить кубоид\n";
        std::cout << "5. Показать характеристики фигур\n";
        std::cout << "0. Выход\n";
        std::cout << "Выберите действие: ";
        std::cin >> choice;

        if (choice == 1) {
            double radius;
            std::cout << "Введите радиус круга: ";
            std::cin >> radius;
            shapes.push_back(new Circle(radius));
        } else if (choice == 2) {
            double width, height;
            std::cout << "Введите ширину и высоту прямоугольника: ";
            std::cin >> width >> height;
            shapes.push_back(new Rectangle(width, height));
        } else if (choice == 3) {
            double radius;
            std::cout << "Введите радиус сферы: ";
            std::cin >> radius;
            shapes.push_back(new Sphere(radius));
        } else if (choice == 4) {
            double width, height;
            std::cout << "Введите ширину и высоту кубоида: ";
            std::cin >> width >> height;
            shapes.push_back(new Cuboid(width, height));
        } else if (choice == 5) {
            for (const auto& shape : shapes) {
                if (auto* circle = dynamic_cast<Circle*>(shape)) {
                    circle->display();
                } else if (auto* sphere = dynamic_cast<Sphere*>(shape)) {
                    sphere->display();
                } else if (auto* rectangle = dynamic_cast<Rectangle*>(shape)) {
                    rectangle->display();
                } else if (auto* cuboid = dynamic_cast<Cuboid*>(shape)) {
                    cuboid->display();
                }
            }
        }
    } while (choice != 0);

    // Освобождение памяти
    for (auto shape : shapes) {
        delete shape;
    }

    return 0;
}
