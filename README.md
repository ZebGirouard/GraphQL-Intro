# GraphQL-Intro

<!-- Created by Zeb Girouard -->
<!-- Estimated Duration 1.5hrs -->

## Objectives

By the end of this lesson, you will be able to:

- Name some cool things about Zeb
- Define common GraphQL terms
- Perform a GraphQL query in a GraphQL Playground
- Perform a GraphQL mutation in a GraphQL Playground

<!-- 0:05 -->
## Who Is Zeb?

- [Codecademy](https://www.codecademy.com) - Fullstack Web Developer
- [All Star Code](https://www.allstarcode.org/) - Mentor
- Hackathon Planner

![Zeb](https://avatars.githubusercontent.com/u/5480464?v=4)

## What Is Zeb's Purpose?

### To ascertain what I *really* want to do, and help others find that truth, too.

## What Has Zeb Done?

- [Webroot/Carbonite/OpenText](https://www.webroot.com/us/en/home/products/compare) - eCommerce Platform Developer (Denver)
- General Assembly - Software Engineering Immersive Instructor (Denver)
- [Hoops by Holly*](https://doingthings.media/) - Wearer of All Hats from EC2 to UX (Remote)
- Imprivata - Build Engineer and Scrum Master (Boston)
- Netbrain - Network Engineer (Boston)
- Hess Education - Kindergarten Teacher (Taipei)
- Envision Prop - SAT/GRE/PSAT Tutor (Taipei)
- Carnegie Mellon - Resident Assistant (Pittsburgh)
- Convenient Mann - Pizza Delivery Boy (MA)

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

## Q & A

Some things Zeb knows something about:

- GraphQL
- Traveling
- Digital Nomad-ing
- Hackathons
- Upskilling
- Foreign Languages / Linguistics
## Resources

- [GraphQL Official Site](https://graphql.org/)
- [Advanced React with GraphQL Course](https://www.udemy.com/course/fullstack-react-graphql-y-apollo-de-principiante-a-experto) \*
- [Apollo Caching](https://www.apollographql.com/docs/react/caching/cache-configuration/)
- [Apollo Federation](https://www.apollographql.com/docs/federation/)
- [Public APIs in GraphQL](https://github.com/APIs-guru/graphql-apis)

> **Note:** \* That course is in Spanish, so I recommend at least an Intermediate level of Spanish fluency before starting. There are other options in different languages on Udemy, I just didn't use them personally.