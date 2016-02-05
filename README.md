##A.P.I.

###Application Programming Interface

##Wait Wat?

#####Ok, that's not a good start.

An API is a service that offers a couple of functions or methods trought protocols in order to communicate with another system.


Do you remeber about SOAP?

![thenam.gif](https://github.com/zenbakiak/rails_api_guide/blob/master/images/thenam.gif)


# RESTful services

#####_"REST stands for REpresentational State Transfer. REST is web standards based architecture and uses HTTP Protocol for data communication. It revolves around resource where every component is a resource and a resource is accessed by a common interface using HTTP standard methods."_

###Client–server
_A uniform interface separates clients from servers. This means that clients are not concerned with data storage, which remains internal to each server._

![contentNegotiation.png](https://github.com/zenbakiak/rails_api_guide/blob/master/images/contentNegotiation.png)

###Stateless
_The client–server communication is further constrained by no client context being stored on the server between requests.   ._

![webservice.png](https://github.com/zenbakiak/rails_api_guide/blob/master/images/webservice.png)

###Cacheable
_As on the World Wide Web, clients and intermediaries can cache responses._

###Layered system
_Intermediary servers may improve system scalability by enabling load balancing and by providing shared caches._

###HTTP Methods

######Commonly used HTTP methods in REST based architecture.

-__GET__ _Provides a read only access to a resource._

-__PUT__ _Used to create a new resource._

-__DELETE__ _Used to remove a resource._

-__POST__ _Used to update a existing resource or create a new resource._


#Let's talk about JSON

![json.jpg](https://github.com/zenbakiak/rails_api_guide/blob/master/images/json.jpg)

#####Nope! nope! nope!

##Not this one!

###The JavaScript Object Notation.

according to www.json.org

__JSON__ (**J**ava**S**cript **O**bject **N**otation) is a lightweight data-interchange format. It is easy for humans to read and write. It is easy for machines to parse and generate. It is based on a subset of the JavaScript Programming Language, Standard ECMA-262 3rd Edition - December 1999. JSON is a text format that is completely language independent but uses conventions that are familiar to programmers of the C-family of languages, including C, C++, C#, Java, JavaScript, Perl, Python, and many others. These properties make JSON an ideal data-interchange language.

- It is just a collection of name/value pairs.

```json
{
  "name": "Juan Pérez",
  "email": "jc@iclialabs.com"
}
```

- It could be an ordered list of values a.k.a Arrays

```json
[
  { "name": "Andrea Legarreta", "email": "legarreta@gmail.com" },
  { "name": "Pedro Páramo", "email": "pedro@gmail.com" },
  { "name": "Lorem Pérez", "email": "lorem@gmail.com" }
]
```

- Both.

```json
[
  {
    "name": "Andrea Legarreta",
    "email": "legarreta@gmail.com",
    "quotes": [
      "El precio del dólar no afecta a los mexicanos",
      "Hay amenazas de muerte, por decir lo del dólar (yeah, sure!)"
    ]
  },
  {
    "name": "Peña Nieto",
    "email": "pena.nieto@mexico.gob",
    "quotes": [
      "Infraschuc, Infraschuc, Infraschuchur",
      "No soy la señora de la casa",
      "I guant to bi very cliar de economic policis shud not mak… shudnot meikus forged"
    ]
  }
]
```


###Yeah yeah, whatever...
![yeahyeah.jpg](https://github.com/zenbakiak/rails_api_guide/blob/master/images/yeahyeah.jpg)

![hgames.jpg](https://github.com/zenbakiak/rails_api_guide/blob/master/images/hgames.jpg)

# Quick quiz

Rules: You can navigate through nodes with ```.``` for every key, if the value is an array navigate with ```[i]``` where i represent the index of the array element.


```js
# Example:

# if you want to get the email of Lorem Ipsum:
result.users[0].email

# Get the mobile number of Lorem Ipsum
results.users[0].phones[0].phone_number
```

- How can you get the home phone number of Antonio?
- How can you get the zip_code of javier@mailinator.com who live in Insurgentes.

```json
result = {
  "users": [
    {
      "name": "Lorem Ipsum",
      "email": "lorem@mailinator.com",
      "address": {
        "street": "Paras 850",
        "zip_code": 64000,
        "state": "Nuevo León",
        "country": "México"
      },
      "phones": [
        {
          "phone_type": "mobile",
          "phone_number": 818181818181
        },
        {
          "phone_type": "home",
          "phone_number": 555555555555
        }
      ]
    },
    {
      "name": "Javiér Pérez",
      "email": "javier@mailinator.com",
      "address": {
        "street": "Insurgentes 300",
        "zip_code": 08500,
        "state": "Ciudad de México",
        "country": "México"
      },
      "phones": [
        {
          "phone_type": "mobile",
          "phone_number": 53456798
        },
        {
          "phone_type": "home",
          "phone_number": 1234567
        }
      ]
    },
    {
      "name": "Antonio",
      "email": "antonio@mailinator.com",
      "address": {
        "street": "Escobedo 200",
        "zip_code": 33456,
        "state": "Guadalajara",
        "country": "México"
      },
      "phones": [
        {
          "phone_type": "mobile",
          "phone_number": 818181818181
        },
        {
          "phone_type": "home",
          "phone_number": 90901909
        }
      ]
    }
  ]
}
```

