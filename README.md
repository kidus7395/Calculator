#include <iostream>
#include <stack>
#include <cctype>
#include <sstream>
#include <cmath>
using namespace std;

int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/' || op == '%') return 2;
    return 0;
}

double applyOp(double a, double b, char op) {
    switch (op) {
        case '+': return a + b;
        case '-': return a - b;
        case '*': return a * b;
        case '/': return b != 0 ? a / b : NAN;
        case '%': return (int)a % (int)b;
        default: return 0;
    }
}

double evaluate(const string& expr) {
    stack<double> values;
    stack<char> ops;
    istringstream ss(expr);
    char ch;

    while (ss >> ch) {
        if (isdigit(ch) || ch == '.') {
            ss.putback(ch);
            double val;
            ss >> val;
            values.push(val);
        }
        else if (ch == '(') {
            ops.push(ch);
        }
        else if (ch == ')') {
            while (!ops.empty() && ops.top() != '(') {
                double b = values.top(); values.pop();
                double a = values.top(); values.pop();
                char op = ops.top(); ops.pop();
                values.push(applyOp(a, b, op));
            }
            if (!ops.empty()) ops.pop(); // remove '('
        }
        else if (ch == '+' || ch == '-' || ch == '*' || ch == '/' || ch == '%') {
            while (!ops.empty() && precedence(ops.top()) >= precedence(ch)) {
                double b = values.top(); values.pop();
                double a = values.top(); values.pop();
                char op = ops.top(); ops.pop();
                values.push(applyOp(a, b, op));
            }
            ops.push(ch);
        }
    }

    while (!ops.empty()) {
        double b = values.top(); values.pop();
        double a = values.top(); values.pop();
        char op = ops.top(); ops.pop();
        values.push(applyOp(a, b, op));
    }

    return values.top();
}

int main() {
    string expression;
    cout << "Enter expression (e.g., 10 + 5 * 2 - 3): ";
    getline(cin, expression);

    try {
        double result = evaluate(expression);
        cout << "Result: " << result << endl;
    } catch (...) {
        cout << "Invalid expression or division by zero." << endl;
    }

    return 0;
}
