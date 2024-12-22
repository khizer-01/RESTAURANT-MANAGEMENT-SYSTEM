# RESTAURANT-MANAGEMENT-SYSTEM


#include <iostream>
#include <vector>
#include <string>
#include <sstream>

using namespace std;

// Base User class
class User {
protected:
    string username;
    string password;
    string role;

public:
    User(string user, string pass, string r) : username(user), password(pass), role(r) {}

    bool login(string user, string pass) {
        if (username == user && password == pass) {
            cout << "Login successful as " << role << endl;
            return true;
        }
        cout << "Login failed!" << endl;
        return false;
    }

    void logout() {
        cout << "User  logged out!" << endl;
    }

    void displayRole() {
        cout << "Role: " << role << endl;
    }
};

// Derived Admin class
class Admin : public User {
public:
    Admin(string user, string pass) : User(user, pass, "Admin") {}

    void addMenuItem(string itemName, double price) {
        cout << "Menu item added: " << itemName << " at $" << price << endl;
    }

    void removeMenuItem(string itemName) {
        cout << "Menu item removed: " << itemName << endl;
    }

    void generateReport() {
        cout << "Generating report..." << endl;
    }
};

// Derived Staff class
class Staff : public User {
public:
    Staff(string user, string pass) : User(user, pass, "Staff") {}

    void takeOrder(int tableNumber, string orderDetails) {
        cout << "Order taken for table " << tableNumber << ": " << orderDetails << endl;
    }

    void updateOrderStatus(int orderId, string newStatus) {
        cout << "Order " << orderId << " status updated to: " << newStatus << endl;
    }

    void generateBill(int tableNumber) {
        cout << "Bill generated for table " << tableNumber << endl;
    }
};

// Restaurant class
class Restaurant {
private:
    vector<string> tables;
    vector<string> menu;
    vector<string> inventory;

public:
    Restaurant() {
        // Initialize some tables
        for (int i = 1; i <= 10; i++) {
            stringstream ss;
            ss << "Table " << i;
            tables.push_back(ss.str());
        }
    }

    void manageTables() {
        cout << "Managing tables..." << endl;
        for (size_t i = 0; i < tables.size(); i++) {
            cout << tables[i] << endl;
        }
    }

    void displayMenu() {
        cout << "Current Menu:" << endl;
        for (size_t i = 0; i < menu.size(); i++) {
            cout << menu[i] << endl;
        }
    }

    void addMenuItem(string item) {
        menu.push_back(item);
        cout << "Menu item added: " << item << endl;
    }

    void manageInventory(string item) {
        inventory.push_back(item);
        cout << "Inventory item added: " << item << endl;
    }
};

// Main function
int main() {
    Admin admin("admin", "admin123");
    Staff staff("staff", "staff123");
    Restaurant restaurant;

    string username, password;
    cout << "Enter username: ";
    getline(cin, username);
    cout << "Enter password: ";
    getline(cin, password);

    if (admin.login(username, password)) {
        int choice;
        do {
            cout << "\nAdmin Menu:\n1. Add Menu Item\n2. Remove Menu Item\n3. Generate Report\n4. Logout\nChoose an option: ";
            cin >> choice;
            cin.ignore(); // To ignore the newline character after the integer input

            switch (choice) {
                case 1: {
                    string itemName;
                    double price;
                    cout << "Enter item name: ";
                    getline(cin, itemName);
                    cout << "Enter item price: ";
                    cin >> price;
                    cin.ignore(); // Ignore newline character
                    admin.addMenuItem(itemName, price);
                    restaurant.addMenuItem(itemName);
                    break;
                }
                case 2: {
                    string itemName;
                    cout << "Enter item name to remove: ";
                    getline(cin, itemName);
                    admin.removeMenuItem(itemName);
                    break;
                }
                case 3:
                    admin.generateReport();
                    break;
                case 4:
                    admin.logout();
                    break;
                default:
                    cout << "Invalid choice!" << endl;
            }
        } while (choice != 4);
    } else if (staff.login(username, password)) {
                int choice;
        do {
            cout << "\nStaff Menu:\n1. Take Order\n2. Update Order Status\n3. Generate Bill\n4. Logout\nChoose an option: ";
            cin >> choice;
            cin.ignore(); // To ignore the newline character after the integer input

            switch (choice) {
                case 1: {
                    int tableNumber;
                    string orderDetails;
                    cout << "Enter table number: ";
                    cin >> tableNumber;
                    cin.ignore(); // Ignore newline character
                    cout << "Enter order details: ";
                    getline(cin, orderDetails);
                    staff.takeOrder(tableNumber, orderDetails);
                    break;
                }
                case 2: {
                    int orderId;
                    string newStatus;
                    cout << "Enter order ID: ";
                    cin >> orderId;
                    cin.ignore(); // Ignore newline character
                    cout << "Enter new status: ";
                    getline(cin, newStatus);
                    staff.updateOrderStatus(orderId, newStatus);
                    break;
                }
                case 3: {
                    int tableNumber;
                    cout << "Enter table number for bill: ";
                    cin >> tableNumber;
                    staff.generateBill(tableNumber);
                    break;
                }
                case 4:
                    staff.logout();
                    break;
                default:
                    cout << "Invalid choice!" << endl;
            }
        } while (choice != 4);
    } else {
        cout << "Invalid username or password!" << endl;
    }
    return 0;
}
