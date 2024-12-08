#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <conio.h>
#include <windows.h> // For Sleep()


// Define constants
#define MAX_PRODUCTS 100
#define FILE_NAME "products.txt"
#define CREDENTIALS_FILE "admin_credentials.txt" // File for storing admin credentials


// Define structure
struct item
{
    char productname[40];
    char productcomp[40]; // Product company
    char c;
    int productid;
    int price;
    int Qnt; // Quantity
    float a;
    char b[200];   // Additional info or description
    char unit[20]; // Unit of measurement (e.g., kg, liter, box)
};


// Function declarations
void wel_come(void);
void login(void);
void adminMenu();
void customerMenu();
void addProduct();
void viewProducts();
void updateProduct();
void deleteProduct();
void searchProduct();
void purchaseProduct();
void saveProducts(struct item products[], int count);
int loadProducts(struct item products[]);
void saveCredentials(const char *username, const char *password);
char *loadCredentials(char *username, char *password);
void changePassword(const char *username);
void changeUsername(const char *oldUsername);


// Welcome function
void wel_come(void)
{
    int i;
    time_t t;
    time(&t);
    printf("                                                                                                         \n");
    printf("\033[0;35mWelcome to\033[0m\n");
    for (i = 0; i < 10; i++)
    {
        printf(".");
        Sleep(70);
    }
    printf("\n");
    printf("\033[0;36mDaffodil General Store\033[0m\n");
    for (i = 0; i < 31; i++)
    {
        printf(".");
        Sleep(70);
    }
    printf("\n");
    printf("\033[0;32mHERE YOU FIND BEST PRODUCT IN REASONABLE PRICE!!\n\033[0m");
    for (i = 0; i < 48; i++)
    {
        printf(".");
        Sleep(70);
    }
    printf("\n");
    printf("\n");


    printf("Current Time : %s", ctime(&t));
    for (i = 0; i < 40; i++)
    {
        printf(".");
        Sleep(70);
    }
    printf("\n");
    printf("\n");
    printf("\nPress any key to continue....\n");
    getch();
    system("cls");
}


// Login function
void login(void)
{
    system("cls");
    char uname[50], pword[50];


    // Load saved credentials
    loadCredentials(uname, pword);


    int attempts = 0;
    char inputUsername[50], inputPassword[50];


    do
    {
        printf("\n");
        printf("****  ADMIN LOGIN  ****\n");
        printf("\n");
        printf(" \nUSERNAME: ");
        scanf("%s", inputUsername);
        printf(" \nPASSWORD: ");
        int i = 0;
        char c = ' ';
        while (i < 50)
        {
            inputPassword[i] = getch();
            c = inputPassword[i];
            if (c == 13) // Enter key pressed
                break;
            else
                printf("*");
            i++;
        }
        inputPassword[i] = '\0';


        if (strcmp(inputUsername, uname) == 0 && strcmp(inputPassword, pword) == 0)
        {
            printf("  \n\nYAH!!! LOGIN IS SUCCESSFUL\n");
            for (i = 0; i < 25; i++)
            {
                printf(".");
                Sleep(70);
            }
            printf("\n\nHEY USER (_). PLEASE WAIT... \n");
            for (i = 0; i < 30; i++)
            {
                printf(".");
                Sleep(70);
            }
            printf("\n\n\nPress any key to continue...");
            getch();
            system("cls");
            return; // Successful login
        }
        else
        {
            printf("\n  SORRY!!!!  LOGIN IS UNSUCCESSFUL\n");
            attempts++;
            getch();
        }
    } while (attempts < 3);


    if (attempts >= 3)
    {
        printf("\nSorry you have entered the wrong username and password three times!!!");
        getch();
        exit(0); // Exit the program if login fails after 3 attempts
    }
}


// Save admin credentials
void saveCredentials(const char *username, const char *password)
{
    FILE *fp = fopen(CREDENTIALS_FILE, "w");
    if (fp)
    {
        fprintf(fp, "%s\n%s\n", username, password);
        fclose(fp);
    }
    else
    {
        perror("Error saving credentials");
    }
}


// Load admin credentials
char *loadCredentials(char *username, char *password)
{
    FILE *fp = fopen(CREDENTIALS_FILE, "r");
    if (fp)
    {
        fgets(username, 50, fp);
        fgets(password, 50, fp);
        username[strcspn(username, "\n")] = 0; // Remove newline
        password[strcspn(password, "\n")] = 0; // Remove newline
        fclose(fp);
        return username; // Returns the username for informational purpose
    }
    else
    {
        // If the credentials file does not exist, ask for new credentials
        printf("Admin credentials not found! Please create new ones.\n");
        char newUsername[50], newPassword[50];
        printf("Enter new username: ");
        scanf("%s", newUsername);
        printf("Enter new password: ");
        int i = 0;
        char c = ' ';
        while (i < 50)
        {
            newPassword[i] = getch();
            c = newPassword[i];
            if (c == 13) // Enter key pressed
                break;
            else
                printf("*");
            i++;
        }
        newPassword[i] = '\0';


        // Save new credentials
        saveCredentials(newUsername, newPassword);
        strcpy(username, newUsername);
        strcpy(password, newPassword);
        return username; // Returns the username
    }
}


// Change admin password
void changePassword(const char *username)
{
    char currentPassword[50], newPassword[50];
    printf("Enter current password: ");
    int i = 0;
    char c = ' ';
    while (i < 50)
    {
        currentPassword[i] = getch();
        c = currentPassword[i];
        if (c == 13) // Enter key pressed
            break;
        else
            printf("*");
        i++;
    }
    currentPassword[i] = '\0';


    char storedCredentials[50], storedPassword[50];
    loadCredentials(storedCredentials, storedPassword);


    if (strcmp(currentPassword, storedPassword) == 0)
    {
        printf("\nEnter new password: ");
        i = 0;
        while (i < 50)
        {
            newPassword[i] = getch();
            c = newPassword[i];
            if (c == 13) // Enter key pressed
                break;
            else
                printf("*");
            i++;
        }
        newPassword[i] = '\0';


        // Save the new credentials
        saveCredentials(username, newPassword);
        printf("\nPassword changed successfully!\n");
    }
    else
    {
        printf("\nCurrent password is incorrect!\n");
    }
}


// Change admin username
void changeUsername(const char *oldUsername)
{
    char currentPassword[50], newUsername[50];
    printf("Enter current password: ");
    int i = 0;
    char c = ' ';
    while (i < 50)
    {
        currentPassword[i] = getch();
        c = currentPassword[i];
        if (c == 13) // Enter key pressed
            break;
        else
            printf("*");
        i++;
    }
    currentPassword[i] = '\0';


    char storedCredentials[50], storedPassword[50];
    loadCredentials(storedCredentials, storedPassword);


    if (strcmp(currentPassword, storedPassword) == 0)
    {
        printf("\nEnter new username: ");
        scanf("%s", newUsername);


        // Save the new credentials
        saveCredentials(newUsername, storedPassword); // Keep the old password
        printf("\nUsername changed successfully!\n");
    }
    else
    {
        printf("\nCurrent password is incorrect!\n");
    }
}


// Admin Menu function
void adminMenu(void)
{
    // Require login before accessing the admin menu
    login();


    int choice;
    do
    {
        system("cls");
        printf("\n ** Daffodil General Store - Admin Menu ** \n");
        printf("\n\t1. Add Product\n");
        printf("\t2. View Products\n");
        printf("\t3. Update Product\n");
        printf("\t4. Delete Product\n");
        printf("\t5. Change Password\n");
        printf("\t6. Change Username\n");
        printf("\t7. Back to Main Menu\n");


        printf("\n\n\tEnter your choice [1-7]: ");
        scanf("%d", &choice);


        system("cls");


        switch (choice)
        {
        case 1:
            addProduct();
            break;
        case 2:
            viewProducts();
            printf("\nPress any key to return to the menu...");
            getchar();
            break;
        case 3:
            updateProduct();
            break;
        case 4:
            deleteProduct();
            break;
        case 5:
            changePassword("admin"); // Assuming "admin" is the username for the logged-in admin
            break;
        case 6:
            changeUsername("admin"); // Change username
            break;
        case 7:
            printf("Returning to Main Menu...\n");
            Sleep(1000); // Pause for a moment before returning
            return;      // Exit admin menu
        default:
            printf("Invalid Choice! Please enter a number between 1 and 7.\n");
            Sleep(1000); // Pause to display error before clearing screen
        }
    } while (1);
}


// Main function
int main()
{
    wel_come(); // Call the welcome screen function


    int choice;
    do
    {
        system("cls");
        printf("\n--- Super Shop Management System ---\n");
        printf("1. Admin Menu\n");
        printf("2. Customer Menu\n");
        printf("3. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        getchar(); // Consume newline character
        switch (choice)
        {
        case 1:
            adminMenu();
            break;
        case 2:
            customerMenu();
            break;
        case 3:
            printf("Exiting the system. Goodbye!\n");
            break;
        default:
            printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 3);
    return 0;
}


void customerMenu()
{
    int choice;
    do
    {
        system("cls");
        printf("\n--- Customer Menu ---\n");
        printf("1. View Products\n");
        printf("2. Search Product\n");
        printf("3. Purchase Product\n");
        printf("4. Back to Main Menu\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        getchar();
        switch (choice)
        {
        case 1:
            viewProducts();
            printf("\nPress any key to return to the menu...");
            getchar();
            break;
        case 2:
            searchProduct();
            break;
        case 3:
            purchaseProduct();
            break;
        case 4:
            printf("Returning to main menu...\n");
            break;
        default:
            printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 4);
}


// Add a new product
void addProduct()
{
    struct item products[MAX_PRODUCTS];
    int count = loadProducts(products);


    if (count >= MAX_PRODUCTS)
    {
        printf("Error: Maximum product limit reached.\n");
        return;
    }


    struct item newProduct;
    printf("Enter product ID: ");
    scanf("%d", &newProduct.productid);
    getchar();
    printf("Enter product name: ");
    fgets(newProduct.productname, sizeof(newProduct.productname), stdin);
    newProduct.productname[strcspn(newProduct.productname, "\n")] = '\0';
    printf("Enter product company: ");
    fgets(newProduct.productcomp, sizeof(newProduct.productcomp), stdin);
    newProduct.productcomp[strcspn(newProduct.productcomp, "\n")] = '\0';
    printf("Enter product price: ");
    scanf("%d", &newProduct.price);
    printf("Enter product quantity: ");
    scanf("%d", &newProduct.Qnt);
    getchar();
    printf("Enter unit of measurement (e.g., kg, liter, box): ");
    fgets(newProduct.unit, sizeof(newProduct.unit), stdin);
    newProduct.unit[strcspn(newProduct.unit, "\n")] = '\0';
    printf("Enter additional description: ");
    fgets(newProduct.b, sizeof(newProduct.b), stdin);
    newProduct.b[strcspn(newProduct.b, "\n")] = '\0';


    products[count++] = newProduct;
    saveProducts(products, count);
    printf("Product added successfully!\n");
}


// View all products
void viewProducts()
{
    struct item products[MAX_PRODUCTS];
    int count = loadProducts(products);


    if (count == 0)
    {
        printf("No products available.\n");
        Sleep(1000); // Pause to let the message display
        return;
    }


    printf("\n--- Product List ---\n");
    printf("---------------------------------------------------------------\n");
    printf("NO.\tName\t\tCompany\t\tPrice\tQuantity\tUnit\tID\n");
    printf("---------------------------------------------------------------\n");


    for (int i = 0; i < count; i++)
    {
        printf("%02d.\t%-15s\t%-15s\t%d\t%d\t\t%-5s\t%d\n",
               i + 1, products[i].productname, products[i].productcomp,
               products[i].price, products[i].Qnt, products[i].unit, products[i].productid);
        getchar();
    }
}


// Update a product
void updateProduct()
{
    struct item products[MAX_PRODUCTS];
    int count = loadProducts(products);


    int id, found = 0;
    printf("Enter the product ID to update: ");
    scanf("%d", &id);


    for (int i = 0; i < count; i++)
    {
        if (products[i].productid == id)
        {
            found = 1;
            printf("Enter new product name: ");
            getchar();
            fgets(products[i].productname, sizeof(products[i].productname), stdin);
            products[i].productname[strcspn(products[i].productname, "\n")] = '\0';
            printf("Enter new product company: ");
            fgets(products[i].productcomp, sizeof(products[i].productcomp), stdin);
            products[i].productcomp[strcspn(products[i].productcomp, "\n")] = '\0';
            printf("Enter new price: ");
            scanf("%d", &products[i].price);
            printf("Enter new quantity: ");
            scanf("%d", &products[i].Qnt);
            getchar();
            printf("Enter new unit of measurement: ");
            fgets(products[i].unit, sizeof(products[i].unit), stdin);
            products[i].unit[strcspn(products[i].unit, "\n")] = '\0';
            printf("Enter new description: ");
            fgets(products[i].b, sizeof(products[i].b), stdin);
            products[i].b[strcspn(products[i].b, "\n")] = '\0';


            saveProducts(products, count);
            printf("Product updated successfully!\n");
            break;
        }
    }


    if (!found)
    {
        printf("Product not found.\n");
    }
}


// Delete a product
void deleteProduct()
{
    viewProducts();
    struct item products[MAX_PRODUCTS];
    int count = loadProducts(products);


    int opt, found = 0;
    printf("Enter option to delete: ");
    scanf("%d", &opt);
    getchar();
    if (opt < 1 || opt > count)
    {
        printf("Invalid option.");
        getchar();
        return;
    }


    for (int j = opt - 1; j < count - 1; j++)
    {
        products[j] = products[j + 1];
    }
    count--;
    saveProducts(products, count);
    printf("\033[32mProduct deleted successfully!\033[0m\n");
    getchar();
}


// Search for a product
void searchProduct()
{
    struct item products[MAX_PRODUCTS];
    int count = loadProducts(products);


    char query[40];
    printf("Enter product name to search: ");
    getchar();
    fgets(query, sizeof(query), stdin);
    query[strcspn(query, "\n")] = '\0';


    printf("\n--- Search Results ---\n");
    int found = 0;
    for (int i = 0; i < count; i++)
    {
        if (strstr(products[i].productname, query))
        {
            found = 1;
            printf(" ID: %d\n Name: %s\n Company: %s\n Price: %d\n Quantity: %d\n Description: %s\n",
                   products[i].productid, products[i].productname, products[i].productcomp,
                   products[i].price, products[i].Qnt, products[i].b);
            getchar();
            getchar();
        }
    }


    if (!found)
    {
        printf("No products found.\n");
        getchar();
    }
}


void purchaseProduct()
{
    struct item products[MAX_PRODUCTS];
    int count = loadProducts(products);


    if (count == 0)
    {
        printf("No products available for purchase.\n");
        printf("Press any key to continue...");
        getchar();
        return;
    }


    viewProducts(); // Display all available products


    int opt, qty;
    float totalCost = 0.0;
    printf("\nEnter the option to purchase: ");
    scanf("%d", &opt);


    if (opt <= 0 || opt > count)
    {
        printf("Invalid option.\n");
        printf("Press any key to continue...");
        getchar();
        getchar();
        return;
    }


    printf("Enter quantity to purchase (in %s): ", products[opt - 1].unit);
    scanf("%d", &qty);


    if (qty > products[opt - 1].Qnt)
    {
        printf("Not enough stock available.\n");
        printf("Press any key to continue...");
        getchar();
        getchar();
        return;
    }


    totalCost = qty * products[opt - 1].price;
    products[opt - 1].Qnt -= qty;
    printf("Purchase added to cart. Total cost: %.2f\n taka", totalCost);


    printf("\n--- Payment Process ---\n");
    float paymentAmount;


    while (1)
    {
        printf("Total Amount Due: %.2f\n taka", totalCost);
        printf("Enter Payment Amount: ");
        scanf("%f", &paymentAmount);


        if (paymentAmount < totalCost)
        {
            printf("Insufficient payment! Please enter at least %.2f.\n", totalCost);
        }
        else
        {
            printf("\nPayment successful! Change: %.2f\n taka", paymentAmount - totalCost);
            break;
        }
    }


    saveProducts(products, count);
    printf("\nThank you for your purchase!\n");
    printf("\nPress any key to continue...");
    getchar();
    getchar();
}


// Save products to file
void saveProducts(struct item products[], int count)
{
    FILE *fp = fopen(FILE_NAME, "w");
    if (!fp)
    {
        perror("Error opening file");
        return;
    }


    fwrite(products, sizeof(struct item), count, fp);
    fclose(fp);
}


// Load products from file
int loadProducts(struct item products[])
{
    FILE *fp = fopen(FILE_NAME, "r");
    if (!fp)
    {
        return 0; // No products saved yet
    }


    int count = fread(products, sizeof(struct item), MAX_PRODUCTS, fp);
    fclose(fp);
    return count;
}
