#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <conio.h>
#include <windows.h> // For Sleep()

// Define constants
#define MAX_PRODUCTS 100
#define FILE_NAME "products.txt"

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
    char b[200]; // Additional info or description
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

    // printf("\nPress any key to continue....\n");

    // getch();
    // system("cls");
}

// Login function
void login(void)
{
    system("cls");
    int a = 0, i = 0;
    char uname[10], c = ' ';
    char pword[10];
    do

    {
        printf("\n");
        printf("**********  ADMIN LOGIN  **********\n");
        printf("\n");
        printf(" \nUSERNAME: ");
        scanf("%s", uname);
        printf(" \nPASSWORD: ");
        while (i < 10)
        {
            pword[i] = getch();
            c = pword[i];
            if (c == 13)
                break; // Enter key pressed
            else
                printf("*");
            i++;
        }
        pword[i] = '\0';
        i = 0;

        if (strcmp(uname, "ramim") == 0 && strcmp(pword, "17662") == 0)
        {
            printf("  \n\nYAH!!! LOGIN IS SUCCESSFUL\n");
            for (i = 0; i < 25; i++)
            {
                printf(".");
                Sleep(70);
            }
            printf("\n\nHEY USER (*_*). PLEASE WAIT... \n");
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
            a++;
            getch();
        }
    } while (a <= 2);

    if (a > 2)
    {
        printf("\nSorry you have entered the wrong username and password three times!!!");
        getch();
        exit(0); // Exit the program if login fails after 3 attempts
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
        printf("\n ****** Daffodil General Store - Admin Menu ****** \n");
        printf("\n\t1. Add Product\n");
        printf("\t2. View Products\n");
        printf("\t3. Update Product\n");
        printf("\t4. Delete Product\n");
        printf("\t5. Back to Main Menu\n");

        printf("\n\n\tEnter your choice [1-5]: ");
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
            printf("Returning to Main Menu...\n");
            Sleep(1000); // Pause for a moment before returning
            return;      // Exit admin menu
        default:
            printf("Invalid Choice! Please enter a number between 1 and 5.\n");
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
    printf("-------------------------------------------------\n");
    printf("NO.\tName\t\tCompany\t\tPrice\tQuantity\tID\n");
    printf("-------------------------------------------------\n");

    for (int i = 0; i < count; i++)
    {
        printf("%02d.\t%-15s\t%-15s\t%d\t%d\t\t%d\n", i + 1, products[i].productname, products[i].productcomp,
               products[i].price, products[i].Qnt, products[i].productid);
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
    int count = loadProducts(products); // Load product data into the array

    if (count == 0) {
        printf("No products available for purchase.\n");
        printf("Press any key to continue...");
        getchar();
        return;
    }

    viewProducts(products, count); // Display all available products

    int opt, qty;
    float totalCost = 0.0;
    printf("\nEnter the option to purchase: ");
    scanf("%d", &opt);

    if (opt <= 0 || opt > count) {
        printf("Invalid option.\n");
        printf("Press any key to continue...");
        getchar();
        getchar();
        return;
    }

    printf("Enter quantity to purchase: ");
    scanf("%d", &qty);

    if (qty > products[opt - 1].Qnt) {
        printf("Not enough stock available.\n");
        printf("Press any key to continue...");
        getchar();
        getchar();
        return;
    }

    // Calculate total cost and update stock
    totalCost = qty * products[opt - 1].price;
    products[opt - 1].Qnt -= qty;
    printf("Purchase added to cart. Total cost: %.2f taka\n", totalCost);

    // Proceed to payment
    printf("\n--- Payment Process ---\n");
    float paymentAmount;

    while (1) {
        printf("Total Amount Due: %.2f\n taka", totalCost);
        printf("Enter Payment Amount: ");
        scanf("%f", &paymentAmount);

        if (paymentAmount < totalCost) {
            printf("Insufficient payment! Please enter at least taka%.2f.\n", totalCost);
        } else {
            printf("\nPayment successful! Change: taka%.2f\n", paymentAmount - totalCost);
            break;
        }
    }

    // Save the updated product data back to the file
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


