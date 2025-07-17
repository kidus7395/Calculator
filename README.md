#include <iostream>
#include <fstream>
#include <vector>
#include <string>
using namespace std;

struct Driver {
    string name;
    string licenseNumber;
    string address;
};

struct Vehicle {
    string registrationNumber;
    string type;
    string ownerLicense;
};

struct Violation {
    string licenseNumber;
    string violationType;
    double fine;
    string date;
};

vector<Driver> drivers;
vector<Vehicle> vehicles;
vector<Violation> violations;

void addDriver() {
    Driver d;
    cout << "\nEnter Driver Name: ";
    cin.ignore();
    getline(cin, d.name);
    cout << "Enter License Number: ";
    getline(cin, d.licenseNumber);
    cout << "Enter Address: ";
    getline(cin, d.address);
    drivers.push_back(d);
    cout << "Driver added successfully.\n";
}

void addVehicle() {
    Vehicle v;
    cout << "\nEnter Vehicle Registration Number: ";
    cin.ignore();
    getline(cin, v.registrationNumber);
    cout << "Enter Vehicle Type (Car/Bike/Truck): ";
    getline(cin, v.type);
    cout << "Enter Owner License Number: ";
    getline(cin, v.ownerLicense);
    vehicles.push_back(v);
    cout << "Vehicle added successfully.\n";
}

void addViolation() {
    Violation v;
    cout << "\nEnter Driver License Number: ";
    cin.ignore();
    getline(cin, v.licenseNumber);
    cout << "Enter Violation Type (Speeding, Red Light, etc): ";
    getline(cin, v.violationType);
    cout << "Enter Fine Amount: ";
    cin >> v.fine;
    cin.ignore();
    cout << "Enter Date (dd/mm/yyyy): ";
    getline(cin, v.date);
    violations.push_back(v);
    cout << "Violation recorded.\n";
}

void displayDrivers() {
    cout << "\n--- Driver Records ---\n";
    for (const auto& d : drivers) {
        cout << "Name: " << d.name << ", License: " << d.licenseNumber << ", Address: " << d.address << endl;
    }
}

void displayVehicles() {
    cout << "\n--- Vehicle Records ---\n";
    for (const auto& v : vehicles) {
        cout << "Reg No: " << v.registrationNumber << ", Type: " << v.type << ", Owner License: " << v.ownerLicense << endl;
    }
}

void displayViolations() {
    cout << "\n--- Violation Records ---\n";
    for (const auto& v : violations) {
        cout << "License: " << v.licenseNumber << ", Violation: " << v.violationType
             << ", Fine: " << v.fine << ", Date: " << v.date << endl;
    }
}

void searchViolationsByLicense() {
    string license;
    cout << "\nEnter License Number to Search Violations: ";
    cin.ignore();
    getline(cin, license);
    bool found = false;
    for (const auto& v : violations) {
        if (v.licenseNumber == license) {
            found = true;
            cout << "Violation: " << v.violationType << ", Fine: " << v.fine << ", Date: " << v.date << endl;
        }
    }
    if (!found) {
        cout << "No violations found for this license.\n";
    }
}

void saveData() {
    ofstream out("traffic_data.txt");
    for (auto& d : drivers)
        out << "D," << d.name << "," << d.licenseNumber << "," << d.address << "\n";
    for (auto& v : vehicles)
        out << "V," << v.registrationNumber << "," << v.type << "," << v.ownerLicense << "\n";
    for (auto& v : violations)
        out << "T," << v.licenseNumber << "," << v.violationType << "," << v.fine << "," << v.date << "\n";
    out.close();
    cout << "Data saved to traffic_data.txt\n";
}

int main() {
    int choice;
    do {
        cout << "\n==== Traffic Management System ====\n";
        cout << "1. Add Driver\n";
        cout << "2. Add Vehicle\n";
        cout << "3. Record Violation\n";
        cout << "4. Display All Drivers\n";
        cout << "5. Display All Vehicles\n";
        cout << "6. Display All Violations\n";
        cout << "7. Search Violations by License\n";
        cout << "8. Save & Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
            case 1: addDriver(); break;
            case 2: addVehicle(); break;
            case 3: addViolation(); break;
            case 4: displayDrivers(); break;
            case 5: displayVehicles(); break;
            case 6: displayViolations(); break;
            case 7: searchViolationsByLicense(); break;
            case 8: saveData(); break;
            default: cout << "Invalid choice.\n";
        }
    } while (choice != 8);

    return 0;
}