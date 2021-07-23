# Cooking App

## Contents page

1. [Presentation](#intro)
2. [Technical notes](#notes)
    1. [Structure](#structure)
    2. [Database](#db)
3. [Execution](#exe)

## Presentation of Cooking startup <a name="intro"></a>

A website offering ready-made meals. Customers order these meals through an interface (HMI) then the meals are delivered to the customers by a delivery service.

**Innovative part : users interaction**

The customers who order the food can also be the cooks who can suggest the recipes and earn credits (the **cook**) that can be used to purchase their meals.

Each customer can submit their own recipes, which can then be added to the list of recipes offered by Cooking (**CdR** = recipe creator, *créateur de recette* in French).

### PoC features

1. For the customers :
* Sign in or create an account
* Browse the list of recipes
* Choose one or more meals and pay it
* Validate the payments with *cook*
* After submitting an order and payment :
    * The order counter should increment
    * The selling price of the recipe increases by 2 *cook* if its number of orders exceeds 10
    * \+ 5 *cook* if it exceeds 50 and the CdR's remuneration increases up to 4 cook
    * Product stocks deducted from the quantities used to make the food that was ordered by the customer

2. For the recipe creators (CdR) :
* Identify themselves as a CdR and access their features, otherwise register
* Create a recipe
    * Input the ingredients of the recipe : *name*, list of *ingredients*, *quantity*, *description* and customer *selling price*
* Check the cook balance
* Overview of their recipe list and their number of orders

3. For the cooking manager :
* Dashboard of the week
    * CdR of the week
    * Top 5 recipes
    * CdR and his/her 5 most ordered recipes
* Weekly replenishment of products
    * Update of the min and max products quantities : a product that has not been used for the last 30 days will have its min and max quantity divided by 2
    * Generation of the weekly order list under an XML format : list of products with a quantity lower than the minimum quantity - quantity ordered equal to the maximum quantity - sorted per supplier then per product
* Delete a recipe
* Delete a cook and all their recipes (but they remain customers)

## Technical notes <a name="notes"></a>

### Console application structure <a name="structure"></a>

![Structure](./Rapport_et_complements/Fonctionnement-Cooking.jpg "Structure")

### Database <a name="db"></a>

#### Entity-Relationship diagram

![ER Diagram](./Rapport_et_complements/DiagrammeEA-Cooking.PNG "ER Diagram")

#### Relational diagram

**Unicity** / <ins>identifier</ins> / *#foreign key*

* Supplier (<ins>**codeSupplier**</ins>, nameF, phoneF)
* Product (<ins>**codeProduct**</ins>, nameP, category, stock, stockMax, stockMin, unit, lastUse)
* Recipe (<ins>**codeRecipe**</ins>, nameR, type, description, veg, priceR, remuneration, *#codeClient*, numberOrderWeekly, numberOrder)
* Basket (<ins>**codeOrder**</ins>, date, priceP, *#codeClient*)
* Client (<ins>**codeClient**</ins>, lastnameC, firstnameC, phoneC, **usernameC**, passwordC, creator, cook, numberOrderCdR)
* Supply (<ins>***#codeSupplier & #codeProduct***</ins>)
* RecipeConstitution (<ins>***#codeRecipe & #codeProduct***</ins>, quantityProduct)
* BasketConstitution (<ins>***#codeOrder & #codeRecipe***</ins>, quantityRecipe)

## Execution <a name="exe"></a>

No need to enter any credentials in the C# code in order to access the database.
A user with all rights is created at the end of the MySQL script.
The credentials of this user are used to log in.

The first two lines of the MySQL script are for dropping the database and deleting all created users.

Here are two credentials to use to login as a client, anyone can create his own :
* id : Senku | password : keurScience (--> this client is a CdR)
* id : Rick  | password : WeAreTheWD  (--> this client is not a CdR)