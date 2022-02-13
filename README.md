# GraphQL-Intro

<!-- Created by Zeb Girouard -->
<!-- Estimated Duration 1.5hrs -->

## Objectives

By the end of this lesson, you will be able to:

- Say you've met a dude named Zeb
- Define common GraphQL terms
- Find a romantic getaway for next Valentine's Day
- Perform a GraphQL query in a GraphQL Playground

<!-- 0:05 -->
## Who Is Zeb?

- [Codecademy](https://www.codecademy.com) - Fullstack Web Developer
- [All Star Code](https://www.allstarcode.org/) - Mentor
- Hackathon Planner

![Zeb](https://avatars.githubusercontent.com/u/5480464?v=4)

## What Has Zeb Done?

<!-- 2 main reasons for this list: to remind everyone that a developer can come from anywhere, and to give ideas for Q&A later -->

- [Webroot/Carbonite/OpenText](https://www.webroot.com/us/en/home/products/compare) - eCommerce Platform Developer (Denver-ish)
- General Assembly - Software Engineering Immersive Instructor (Denver)
- [Hoops by Holly*](https://doingthings.media/) - Wearer of All Hats from EC2 to UX (Remote)
- Imprivata - Build Engineer and Scrum Master (Boston-ish)
- Netbrain - Network Engineer (Boston-ish)
- Hess Education - Kindergarten Teacher (Taipei)
- Envision Prop - SAT/GRE/PSAT Tutor (Taipei)
- Carnegie Mellon - Resident Assistant (Pittsburgh)
- Convenient Mann - Pizza Delivery Boy (Boston-ish)

![Delivery Boy](https://external-preview.redd.it/FpDfEER_FmLrymffrkgWyNyiUja2AeqB8T6ATkAqIW0.png?auto=webp&s=564526b4b1fd34f2fb40c54f51d2fae3f578defe)

\* Yes, *that* Holly.

<!-- 0:15 -->

## What Is GraphQL?

![GraphQL Logo](https://graphql.org/img/og-image.png)

According to the [official site](https://graphql.org/):

> GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. GraphQL provides a complete and understandable description of the data in your API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.

<!-- 0:20 -->

## How Did We Get Here?

![before time](https://media0.giphy.com/media/3o7TKFJIIZaUV0DBnO/source.gif)

In the [before time](https://media0.giphy.com/media/3o7TKFJIIZaUV0DBnO/source.gif), developers would format their API calls in a myriad of ways:

- Query Parameters
- Comma-separated strings
- SOAP/XML
- Files
- Some weird mixture of the above options

Then, along came [RESTful APIs](https://www.redhat.com/en/topics/api/what-is-a-rest-api), and we finally had a stateless, and well-defined system of data-model reads and writes.

Coupled with CRUD, we could now identify 4 unique actions, with their expected **request** and **response** format:

| CRUD      | HTTP |
| ---       | --- |
| Create    | POST |
| Read      | GET |
| Update    | PUT |
| Delete    | DELETE |

With JSON, we could now quickly, and **mutually intelligibly** interact with an API. For example, want to get all of the `countries` from [The Countries List API](https://annexare.github.io/Countries/)? Send the following request:

`GET /countries`

and you get the response:

```
  "countries": {
    "AE": {
      "name": "United Arab Emirates",
      "native": "دولة الإمارات العربية المتحدة",
      "phone": "971",
      "continent": "AS",
      "capital": "Abu Dhabi",
      "currency": "AED",
      "languages": [
        "ar"
      ],
      "emoji": "🇦🇪",
      "emojiU": "U+1F1E6 U+1F1EA"
    },
    ...
    "UA": {
      "name": "Ukraine",
      "native": "Україна",
      "phone": "380",
      "continent": "EU",
      "capital": "Kyiv",
      "currency": "UAH",
      "languages": [
        "uk"
      ],
      "emoji": "🇺🇦",
      "emojiU": "U+1F1FA U+1F1E6"
    }
  }
```

Easy. Beautiful. But that's a LOT of countries: that ellipsis hides about 200 countries! We can filter with query parameters like `continent=EU` or `currency=in["EUR", "USD"]`, but not only is that hard to read, we'd need to keep those query parameters constant **forever** once we deploy our API.

Not only that, but let's say we just want to display a list of these countries' names. Why do we need `phone` or `capital`? That's extra data over the wire, and extra data in the front end. We're using excess memory, and taking excess time. In a world where everyone is starting to expect EVERYTHING like RIGHT NOW, every kilobyte and every second counts.

Enter...

<!-- 0:30 -->

## GraphQL

So what's so different about GraphQL?

<!-- Make sure folks do NOT follow along for this -->

For one, the queries look a little different.

```
query countries {
  countries {
    name
    currency
    continent {
      name
    }
  }
}
```

or, we can use the shorthand:

```
{
  countries {
    name
    currency
    continent {
      name
    }
  }
}
```

And here's the result:

```
{
  "data": {
    "countries": [
      {
        "name": "Andorra",
        "currency": "EUR",
        "continent": {
          "name": "Europe"
        }
      },
      ...
    ]
  }
}
```

Excellent! We're just getting the `name`, `currency`, and `continent.name` now.

Now, let's try something fancy. Nowadays, many countries are abandoning their local currency in favor of a more global currency like the `EUR` or `USD`. Imagine you are a European or American traveler, looking to spend your tourist-moneys far from home, and avoid any currency exchange. We can search for all countries that use the `EUR` or the `USD` that are neither in North America or Europe. In GraphQL, that query would be:

```
{
  countries(filter: {
    currency: {
      in: ["EUR", "USD"]
    }
    continent: {
      nin: ["EU", "NA"]
    }
  }) {
    name
    currency
    continent {
      name
    }
  }
}
```

And that query returns:

```
{
  "data": {
    "countries": [
      ...
      {
        "name": "Palau",
        "currency": "USD",
        "continent": {
          "name": "Oceania"
        }
      },
      ...
    ]
  }
}
```

Looks like I'm taking a trip to Palau!

![Palau](https://www.geograf.in/images/pic/b3098cea.jpg)

<!-- 0:45 -->

## Your Turn

You can answer all the questions below by querying [The Countries GraphQL API](https://countries.trevorblades.com/).

Try to work out the answer for at least a minute without using the **Hint**.

Only check the **Answer** if you're really stuck.

1. We can directly query 3 data types with this API. What are they?

<details>
  <summary>Hint</summary>

  ## Check the DOCS Section
</details>

<details>
  <summary>Answer</summary>

  ## Country, Continent, Language
</details>

2. How many continents does this API have information for? Which one is not really a continent (more of a geographical area)?

<details>
  <summary>Hint</summary>

  ## Check the `continents` query
</details>

<details>
  <summary>Answer</summary>

  ## 7 / Oceania
</details>

3. How many countries in South America have a currency other than the Euro or the US Dollar? Which ones **don't** have Spanish in their `languages` list?\*

<details>
  <summary>Hint</summary>

  ## Look at the documentation for `CountryFilterInput`
</details>

<details>
  <summary>Answer</summary>
  
  ## 12 / Brazil, Falkland Islands, Guyana, Suriname
  ```
  {
    countries(filter: {
      currency: {
        nin: ["EUR", "USD"]
      }
      continent: {
        eq: "SA"
      }
    }) {
      name
      currency
      continent {
        code
        name
      }
      languages {
        name
      }
    }
  }
  ```
</details>

> \* **Note:** Unfortunately, we can't filter countries by language with this API. Where do you think we might add this filter? What might it look like?

## Q & A

Some things Zeb can answer questions about:

- GraphQL
- Digital Nomad-ing
- Hackathons
- Upskilling
- Foreign Languages / Linguistics
- Backend vs Frontend
- Tech Job Hunting
## Resources

- [GraphQL Official Site](https://graphql.org/)
- [Advanced React with GraphQL Course (Spanish)](https://www.udemy.com/course/fullstack-react-graphql-y-apollo-de-principiante-a-experto) \*
- [Apollo Caching](https://www.apollographql.com/docs/react/caching/cache-configuration/)
- [Apollo Federation](https://www.apollographql.com/docs/federation/)
- [Public APIs in GraphQL](https://github.com/APIs-guru/graphql-apis)
