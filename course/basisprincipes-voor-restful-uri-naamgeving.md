# Basisprincipes voor RESTful URI-naamgeving

# Regels voor de naamgeving van URI's in RESTful APIs

1.  **Gebruik zelfstandige naamwoorden, geen werkwoorden**
    
    -   Goed: `/products`, `/users`
    -   Fout: `/getProducts`, `/createUser`
2.  **Gebruik meervoudsvorm voor collecties**
    
    -   Goed: `/customers`, `/orders`
    -   Vermijd: `/customer`, `/order` voor collecties
3.  **Gebruik van ID voor specifieke resources**
    
    -   Goed: `/customers/1234`, `/users/john`
4.  **Gebruik kleine letters**
    
    -   Goed: `/product`
    -   Vermijd: `/Product`
5.  **Gebruik koppeltekens (-) voor woorden in resource namen**
    
    -   Goed: `/shipping-addresses`
    -   Vermijd: `/shipping_addresses` of `/shippingAddresses`
6.  **Gebruik hiërarchie voor het weergeven van relaties**
    
    -   Goed: `/users/123/orders` (alle bestellingen van gebruiker 123)
    -   Goed: `/orders/456/items` (alle items in bestelling 456)
7.  **Gebruik query parameters voor filtering, sortering en paginering**
    
    -   Goed: `/products?category=electronics&sort=price&page=2`
    -   Niet: `/getElectronicsProductsSortedByPricePageTwo`
8.  **Consistentie in de gehele API**
    
    -   Gebruik dezelfde naamgevingsconventies voor alle endpoints
    -   Wees consequent in je hoofdlettergebruik, meervoudsvormen, etc.