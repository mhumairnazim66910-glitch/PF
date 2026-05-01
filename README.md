# PF
calculates totals with 17% GST, and generates a customer receipt using arrays and functions.
#include <iostream>
#include <string>
using namespace std;
 
// ─── CONSTANTS ───────────────────────────────────
const float TAX_RATE = 0.17;
 
// ─── GLOBAL ARRAYS ───────────────────────────────
string itemName[50];
int itemQty[50];
float itemPrice[50];
int totalItems = 0;
 
// ─── FUNCTION DECLARATIONS ───────────────────────
void showMenu();
void addItem();
void viewBill();
void generateReceipt();
void clearBill();
void aboutProject();
 
// ─── HELPER: print a line ────────────────────────
void line(char ch = '-', int len = 50) {
for (int i = 0; i < len; i++) cout << ch;
cout << endl;
}
 
// ─── HELPER: print float to 2 decimal places ─────
// (replaces setprecision(2) from iomanip)
void printFloat(float val) {
int whole = (int)val;
int decimal = (int)((val - whole) * 100 + 0.5); // round to 2 decimals
 
cout << whole << ".";
if (decimal < 10) cout << "0"; // e.g. 5 becomes 05
cout << decimal;
}
 
// ─── HELPER: pad string with spaces ──────────────
// (replaces setw() from iomanip)
void padString(string text, int width) {
cout << text;
int spaces = width - text.length();
for (int i = 0; i < spaces; i++) cout << " ";
}
 
void padInt(int num, int width) {
// count digits
int temp = num, digits = 1;
while (temp >= 10) { temp /= 10; digits++; }
 
cout << num;
int spaces = width - digits;
for (int i = 0; i < spaces; i++) cout << " ";
}
 
// ─────────────────────────────────────────────────
// MAIN
// ─────────────────────────────────────────────────
int main() {
int choice;
 
cout << endl;
line('=');
cout << " WELCOME TO BILLING SYSTEM" << endl;
cout << " PF Project | C++ | Easy" << endl;
line('=');
 
do {
showMenu();
cout << "Enter your choice: ";
cin >> choice;
cin.ignore();
 
switch (choice) {
case 1: addItem(); break;
case 2: viewBill(); break;
case 3: generateReceipt(); break;
case 4: clearBill(); break;
case 5: aboutProject(); break;
case 0:
cout << "\nThank you! Goodbye.\n";
break;
default:
cout << "\n[!] Invalid choice. Try again.\n";
}
 
} while (choice != 0);
 
return 0;
}
 
// ─────────────────────────────────────────────────
// SHOW MENU
// ─────────────────────────────────────────────────
void showMenu() {
cout << endl;
line();
cout << " MAIN MENU" << endl;
line();
cout << " 1. Add Item to Bill" << endl;
cout << " 2. View Current Bill" << endl;
cout << " 3. Generate Final Receipt" << endl;
cout << " 4. Clear / New Bill" << endl;
cout << " 5. About This Project" << endl;
cout << " 0. Exit" << endl;
line();
}
 
// ─────────────────────────────────────────────────
// ADD ITEM
// ─────────────────────────────────────────────────
void addItem() {
if (totalItems >= 50) {
cout << "\n[!] Bill is full! Max 50 items.\n";
return;
}
 
cout << endl;
line();
cout << " ADD ITEM" << endl;
line();
 
cout << "Item Name : ";
getline(cin, itemName[totalItems]);
 
cout << "Quantity : ";
cin >> itemQty[totalItems];
 
cout << "Unit Price : Rs. ";
cin >> itemPrice[totalItems];
cin.ignore();
 
float subtotal = itemQty[totalItems] * itemPrice[totalItems];
cout << "\n[+] Item added! Subtotal = Rs. ";
printFloat(subtotal);
cout << endl;
 
totalItems++;
}
 
// ─────────────────────────────────────────────────
// VIEW CURRENT BILL
// ─────────────────────────────────────────────────
void viewBill() {
cout << endl;
line();
cout << " CURRENT BILL" << endl;
line();
 
if (totalItems == 0) {
cout << " [!] No items added yet.\n";
return;
}
 
// header row (manual spacing)
cout << "No ";
cout << "Item ";
cout << "Qty ";
cout << "Price ";
cout << "Subtotal" << endl;
line();
 
float grandTotal = 0;
 
for (int i = 0; i < totalItems; i++) {
float sub = itemQty[i] * itemPrice[i];
grandTotal += sub;
 
padInt(i + 1, 4);
padString(itemName[i], 19);
padInt(itemQty[i], 6);
cout << "Rs."; printFloat(itemPrice[i]); cout << " ";
cout << "Rs."; printFloat(sub);
cout << endl;
}
 
float tax = grandTotal * TAX_RATE;
float total = grandTotal + tax;
 
line();
cout << " Subtotal : Rs. "; printFloat(grandTotal); cout << endl;
cout << " Tax(17%) : Rs. "; printFloat(tax); cout << endl;
line();
cout << " TOTAL : Rs. "; printFloat(total); cout << endl;
line();
}
 
// ─────────────────────────────────────────────────
// GENERATE FINAL RECEIPT
// ─────────────────────────────────────────────────
void generateReceipt() {
if (totalItems == 0) {
cout << "\n[!] No items to generate receipt.\n";
return;
}
 
string customerName;
cout << "\nEnter Customer Name: ";
getline(cin, customerName);
 
cout << endl;
line('*');
cout << " SMART MART - BILLING RECEIPT" << endl;
cout << " Lahore, Pakistan | Ph: 0300-0000000" << endl;
line('*');
cout << " Customer : " << customerName << endl;
cout << " Items : " << totalItems << endl;
line();
 
// header
cout << "No ";
cout << "Item ";
cout << "Qty ";
cout << "Unit Price ";
cout << "Amount" << endl;
line();
 
float grandTotal = 0;
 
for (int i = 0; i < totalItems; i++) {
float sub = itemQty[i] * itemPrice[i];
grandTotal += sub;
 
padInt(i + 1, 4);
padString(itemName[i], 19);
padInt(itemQty[i], 6);
cout << "Rs."; printFloat(itemPrice[i]); cout << " ";
cout << "Rs."; printFloat(sub);
cout << endl;
}
 
float tax = grandTotal * TAX_RATE;
float total = grandTotal + tax;
 
line();
cout << " Subtotal : Rs. "; printFloat(grandTotal); cout << endl;
cout << " GST(17%) : Rs. "; printFloat(tax); cout << endl;
line('*');
cout << " NET TOTAL: Rs. "; printFloat(total); cout << endl;
line('*');
cout << " ** Thank You for Shopping! **" << endl;
line('*');
}
 
// ─────────────────────────────────────────────────
// CLEAR BILL
// ─────────────────────────────────────────────────
void clearBill() {
char confirm;
cout << "\nAre you sure you want to clear the bill? (y/n): ";
cin >> confirm;
cin.ignore();
 
if (confirm == 'y' || confirm == 'Y') {
totalItems = 0;
cout << "[+] Bill cleared. Ready for new customer.\n";
} else {
cout << "[-] Cancelled.\n";
}
}
 
// ─────────────────────────────────────────────────
// ABOUT PROJECT
// ─────────────────────────────────────────────────
void aboutProject() {
cout << endl;
line('=');
cout << " ABOUT THIS PROJECT" << endl;
line('=');
cout << " Project : Billing System" << endl;
cout << " Subject : Programming Fundamentals (PF)" << endl;
cout << " IDE : Dev-C++ / Code::Blocks" << endl;
cout << " Lang : C++ (Basic - No OOP)" << endl;
cout << endl;
cout << " FEATURES USED:" << endl;
cout << " - Functions" << endl;
cout << " - Arrays (parallel arrays)" << endl;
cout << " - Loops & Conditions" << endl;
cout << " - Input / Output (cin, cout)" << endl;
cout << " - String handling" << endl;
cout << " - Manual formatting (no iomanip)" << endl;
line('=');
}
