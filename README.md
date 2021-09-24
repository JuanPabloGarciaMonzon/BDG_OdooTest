# Python/Odoo Programmer Test
## Odoo Implementation
I used a Docker Hub image using WSL2 with an Ubuntu 20.04 LTS virtual machine to run Odoo, using the 13th version. Docker-compose was the tool of preference to deploy the containers needed in order for Odoo to be displayed and customized.

The containers are:

* PostgreSQL:13
* Odoo:13

It's a requirement to create a file called "odoo_pg_pass" for managing the keys that PostgreSQL and Odoo ask.

The yaml file used:
```dockerfile
version: '3.1'
services:
  web:
    image: odoo:13.0 "13th version"
    depends_on:
      - db
    ports:
      - "8069:8069" "the port where it's going to be listening"
    volumes:
      - odoo-web-data:/var/lib/odoo
      - ./config:/etc/odoo
      - ./addons:/mnt/extra-addons
    environment:
      - PASSWORD_FILE=/run/secrets/postgresql_password
    secrets:
      - postgresql_password
  db:
    image: postgres:13
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_PASSWORD_FILE=/run/secrets/postgresql_password
      - POSTGRES_USER=odoo
      - PGDATA=/var/lib/postgresql/data/pgdata
    volumes:
      - odoo-db-data:/var/lib/postgresql/data/pgdata
    secrets:
      - postgresql_password
volumes:
  odoo-web-data:
  odoo-db-data:

secrets:
  postgresql_password:
    file: odoo_pg_pass "local file"
```
After Odoo is deployed
![](images/WSL1.png)
![](images/WSL2.png)

## Part 1
We can begin to add the applications needed for the requirements needed from the test, these are:

* Website
* Sales
* E-commerce
* Invoicing
* Inventory

![](images/Od1.png)

After all the applications are installed we edit our Home Page with the aid of the Website module.
![](images/Od_WPEdit.png)
![](images/Od_WP.png)

Now to the main reason of having an E-commerce module, adding the products. I'll use the template that Odoo give to the user to add in a batch, but it can be created one by one also.

![](images/Od_Products.png)
![](images/Od_ProductsIngress.png)
![](images/Od_Products_Details.png)

With products, now our Shop has something to sell, but we have to tidy some details in order to comply with the requirements of the test. These are that some information of the product needs to be displayed when the user will buy something. The data is:
* Name
* Code
* Price
* Description

By default Odoo is going to give us the name, price and description of the product. We only have to add the code, and the photo.
![](images/Od_ProductsEditedandPublished.png)
![](images/Od_ProductsPublished.png)

With the resource that Odoo has to edit the HTML/JavaScript/CSS of the product, we add this little line of HTML that will display the Code in a "h3" title. This will not only edit the current product but all that are published in the Shop

![](images/Od_HTML.png)
![](images/Od_Products_Custom.png)
![](images/Od_Shop.png)

This completes the part 1 of the test. The part 2 is to buy one product, add information like address and email to create an invoice for the user.

## Part 2
In order to send an email we have to twick a little some of the settings. First we need an email server, in my case I'll use gmail.

So first we need to have a gmail account, and enable the option to manage third party emails.
![](images/Od_Gm1.png)
![](images/Od_Gm2.png)
![](images/Od_Gm3.png)

In Odoo we have to follow these steps:


* Enable the Developer mode in Odoo "General Settings". In my case it's already enable.
![](images/Od_Sm1.png)
* Goto Settings ‣ Technical ‣ Email ‣ Templates
![](images/Od_Sm2.png)
* Open the first template, in my case "Partner Mass Mail"
![](images/Od_Sm3.png)
* Click "Edit" and choose "Advanced Settings"
![](images/Od_Sm4.png)
* Select "Outgoing Mail Server"  ‣ "Create and Edit"
![](images/Od_Sm5.png)
* Set the SMTP information
![](images/Od_Sm6.png)
* Test the connection
![](images/Od_Sm7.png)
* Save the template
![](images/Od_Sm8.png)

Now we can buy some products from our "Shop". And we'll use the page as a visitor, not as the administrator.
![](images/Od_B1.png)

The products that will be bought are:

* 2 "Mouse, Wireless"
* 1 "Laptop Customized"
* 3 "iPod"
* 1 "Apple Wireless Keyboard"
* 2 "Apple In-Ear Headphones"
* 1 "Self Build Kit"

![](images/Od_B2.png)
![](images/Od_B3.png)
![](images/Od_B4.png)
![](images/Od_B5.png)
![](images/Od_B6.png)
Now I'll confirm the Sale in the "Unpaid Orders" and the Invoice will be send.
![](images/Od_F1.png)
![](images/Od_F2.png)
![](images/Od_F3.png)
![](images/Od_F4.png)
![](images/Od_F5.png)
![](images/Od_F6.png)
![](images/Od_F7.png)
![](images/Od_F8.png)
![](images/Od_F9.png)
![](images/Od_F10.png)
![](images/Od_F11.png)
![](images/Od_F12.png)

The Invoice has been send, thanks for the chance to do this test. 