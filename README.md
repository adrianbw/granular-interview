# Granular Frontend Interviewing

- [Hi there! 👋](#hi-there-)
  - [Before the Coding Interview](#before-the-coding-interview)
- [The API](#the-api)
  - [GET `/fields` &rarr; `{ fields: BasicField[] }`](#get-fields---fields-basicfield-)
  - [GET `/fields/:id` &rarr; `ExtendedField`](#get-fieldsid--extendedfield)
- [The API is a Bit Terrible...](#the-api-is-a-bit-terrible)
- [Your Task](#your-task)
  - [Please note!](#please-note)

## Hi there! 👋

Thank you for considering Granular! For a portion of your interview, you'll be **live-coding a small React app in JavaScript or TypeScript** based on an API we provide. We'd prefer you use Typescript, but if you've never used it before, please stick with what you know!

### Before the Coding Interview

Please make sure you have

- Installed [the Zoom client](https://zoom.us/download) _and are able to screen-share_
- Your favorite React programming setup all ready to go
- The following packages installed in your app:
  - [This library](https://www.npmjs.com/package/react-world-flags) to render some flags
  - [This library](https://www.npmjs.com/package/@mapbox/geojson-area) to compute area
  - Your favorite fetching library, as you'll be interacting with our API
  - Your UI library of choice. We use (a highly customized extension of) [Reactstrap](https://reactstrap.github.io/) for our products so if you're comfortable using it, please do.
- An understanding of the API you'll be working with, speaking of...

## The API

The API can be found at <https://us.insights.granular.ag/api/technical-interview-api/>.

It always replies with an `application/json` content-type and has only two endpoints. Please look at [`types.ts`](/types.ts) for what you'll get back. If you're using Typescript, you will want to use that file in your project.

### GET `/fields` &rarr; `{ fields: BasicField[] }`

Returns some basic information on all the fields in the backend. Here's a sample response:

```javascript
{
  "fields": [
    {
        "id": "318fcafb-a40c-425e-bb91-798f2b3e6c3e",
        "name": "Makeba",
        "type": "corporate"
    },
    {
        "id": "2b103f85-919b-4826-9858-00b0729f2fb9",
        "name": "Olathe Farms",
        "type": "individual"
    },
    // ... more `BasicField`s
  ]
}
```

### GET `/fields/:id` &rarr; `ExtendedField`

For the given field ID, returns its basic information _plus_ extended information:

- Field geometry in valid [GeoJSON](https://geojson.org/)
- The field's [two-letter country code](https://www.iban.com/country-codes), and
- The field's owner

Here's a sample request and response:

GET /fields/318fcafb-a40c-425e-bb91-798f2b3e6c3e

```javascript
{
  "id": "318fcafb-a40c-425e-bb91-798f2b3e6c3e",
  "countryCode": "IN",
  "name": "Moyanka",
  "owner": "Giuseppe Sydow",
  "type": "collective",
  "geoData": {
    "type": "Feature",
    "properties": {},
    "geometry": {
      "type": "Polygon",
      "coordinates": [
        [
          [-95.42449951171875, 44.32384807250689],
          [-95.152587890625, 44.32384807250689],
          [-95.152587890625, 44.39257961837961],
          [-95.42449951171875, 44.39257961837961],
          [-95.42449951171875, 44.32384807250689]
        ]
      ]
    }
  }
}
```

You'll get the expected `HTTP 404` if the Field ID was not found in the `/fields` collection.

## The API is a Bit Terrible

At Granular, our APIs are fast, resilient, and reliable 😎 But _this_ API isn't _any_ of these things 🙄

- For both endpoints, it will reply with a **happy `HTTP 200` around 75% of the time** and sulk with a **sad `HTTP 500` around 25% of the time**.
- You might **wait between 10ms and 3s** for all responses.

Locally, you can add these URL params to all endpoints so _you_ can develop faster:

- Add `?fail` to get nothing but HTTP 500s
- Add `?succeed` to get nothing but HTTP 200s (supersedes `fail` (so let's not waste time being cheeky with `fail&succeed` 😁))
- Add `?fast` to enjoy a super-fast API without the simulated latency

Example: `/fields?fail&fast`

## Your Task

We'll give you details of the task when your interview starts, but as you can probably guess, it will involve building a React application that interacts with the API above.

### Please note

👉 **Don't worry about how your solution looks** (we have a fantastic Design team for that.) Just focus on the _states_ of the app and the _information_ you're showing the user.

👉 Please be prepared for questions about **how you'd test your app**.

👉 Please be prepared for broad discussions about **state management and performance.**