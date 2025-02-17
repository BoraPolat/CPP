#include <iostream>
#include <string>
using namespace std;
class Billing {
protected:
    float water_bill;
    float electricity_bill;
public:
    virtual void set_bills(float water, float electricity) = 0;
    virtual float get_bills() = 0;
};
class business {
protected:
    string business_name;
    int personnel_number;
    double tax;
    double tip;
    double m_income;
    double all_salary;
public:
    struct personnel {
        float salary;
    };
    personnel p[100];
    virtual void monthly_income() = 0;
    virtual void input_basic_info() = 0;
    double get_income() const {
        return m_income;
    }
};
class billing_details : public Billing {
public:
    void set_bills(float water, float electricity) override {
        water_bill = water;
        electricity_bill = electricity;
    }
    float get_bills() override {
        return water_bill + electricity_bill;
    }
};
class restaurant : public business {
private:
    float menu_price;
    float menu_perday;
    billing_details billing;
public:
    void input_basic_info() override {
        cout << "What is your restaurant's name?" << endl;
        cin >> business_name;
        cout << "How many personnel do you have working with you?" << endl;
        cin >> personnel_number;
        cout << "How much is your monthly tax?" << endl;
        cin >> tax;
        cout << "How much is your monthly tip?" << endl;
        cin >> tip;
    }
    void input_personnel_salary() {
        all_salary = 0;
        for (int i = 0; i < personnel_number; i++) {
            cout << "What is the personnel " << i + 1 << "'s salary?" << endl;
            cin >> p[i].salary;
            all_salary += p[i].salary;
        }
    }
    void set_bills(float water, float electricity) {
        billing.set_bills(water, electricity);
    }
    void monthly_income() override {
        input_basic_info();
        cout << "What is the fixed menu's price?" << endl;
        cin >> menu_price;
        cout << "How many fixed menus do you sell in a day?" << endl;
        cin >> menu_perday;
        input_personnel_salary();
        m_income = ((menu_perday * menu_price * 30) + tip) - (all_salary + tax + billing.get_bills());
        cout << "Restaurant " << business_name << "'s monthly income is " << m_income << "$" << endl;
    }
};
class pub : public business {
private:
    int bucket;
    float beer_price, whiskey_price, vodka_price, tequila_price;
    billing_details billing;
public:
    void input_basic_info() override {
        cout << "What is your pub's name?" << endl;
        cin >> business_name;
        cout << "How many personnel do you have working with you?" << endl;
        cin >> personnel_number;
        cout << "How much is your monthly tax?" << endl;
        cin >> tax;
        cout << "How much is your monthly tip?" << endl;
        cin >> tip;
    }
    void input_personnel_salary() {
        all_salary = 0;
        for (int i = 0; i < personnel_number; i++) {
            cout << "What is the personnel " << i + 1 << "'s salary?" << endl;
            cin >> p[i].salary;
            all_salary += p[i].salary;
        }
    }
    void set_bills(float water, float electricity) {
        billing.set_bills(water, electricity);
    }
    void setdrinks(float beer, float whiskey, float vodka, float tequila) {
        beer_price = beer;
        whiskey_price = whiskey;
        vodka_price = vodka;
        tequila_price = tequila;
    }
    void monthly_income() override {
        input_basic_info();
        cout << "How many alcohol buckets do you sell in a day?" << endl;
        cin >> bucket;
        input_personnel_salary();
        float total_drinks = beer_price + whiskey_price + vodka_price + tequila_price;
        m_income = (((bucket * total_drinks) * 30) + tip) - (all_salary + tax + billing.get_bills());
        cout << "Pub " << business_name << "'s monthly income is " << m_income << "$" << endl;
    }
};
int main() {
    business* business_ptr;
    restaurant restaurant1;
    pub pub1;
    char ch1, ch2;
    int water1, electricity1;
    cout << endl << "-- Welcome to The Business Management Tools' Application For Los Angeles --" << endl;
    cout << "-- Electricity and Water Bills are set to 1000$ for each due to The Los Angeles Laws. If you are living in a place that doesn't follow these laws, press `1`. To just continue press any key --" << endl;
    cin >> ch1;
    cout << "Which one do you want to use for?" << endl << "1-Restaurant" << endl << "2-Pub" << endl;
    cin >> ch2;
    if (ch2 == '1') {
        if (ch1 != '1') {
            restaurant1.set_bills(1000, 1000);
        } else {
            cout << "How much is your monthly water bill?" << endl;
            cin >> water1;
            cout << "How much is your monthly electricity bill?" << endl;
            cin >> electricity1;
            restaurant1.set_bills(water1, electricity1);
        }
        business_ptr = &restaurant1;
        business_ptr->monthly_income();
    }
    else if (ch2 == '2') {
        if (ch1 != '1') {
            pub1.set_bills(1000, 1000);
        } else {
            cout << "How much is your monthly water bill?" << endl;
            cin >> water1;
            cout << "How much is your monthly electricity bill?" << endl;
            cin >> electricity1;
            pub1.set_bills(water1, electricity1);
        }
        pub1.setdrinks(2, 5, 5, 5);
        business_ptr = &pub1;
        business_ptr->monthly_income();
    } else {
        cout << "Invalid option" << endl;
    }
    return 0;
}
