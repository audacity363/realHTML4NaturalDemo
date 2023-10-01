# realHTML4NaturalDemo

This repo contains a Natural One Project which showcases the differend capabilities of the framework. For every available connector there is a natural library which shows the functionality.

The natural library "RH4N" is defined as a steplib and contains some common helpers.

## TOMCON
This library contains example programs for the [TomcatConnector](https://github.com/audacity363/realHTML4NaturalTomcatConnector). Merge the following config with yours and make the necessary changes if your environment has some specialities:

```
<realHTML4Natural loglevel="ERROR">
    <environment authHeaderField="" authServer="" name="rh4ndemo" natbinpath="/opt/softwareag/Natural/bin" natparms="" natsrcpath="/opt/softwareag/Natural/fuser">
        <route path="/echo">
            <natLibrary>TOMCON</natLibrary>
            <natProgram>SECHO</natProgram>
            <login>false</login>
            <loglevel>ERROR</loglevel>
            <active>true</active>
        </route>
        <route path="/shop/:cmd=articledetail|order|article-list">
            <natLibrary>TOMCON</natLibrary>
            <natProgram>SSHOP</natProgram>
            <login>false</login>
            <loglevel>ERROR</loglevel>
            <active>true</active>
        </route>
    </environment>
</realHTML4Natural>
```

### Examples:
In the following examples the program `jq` is just used to generate a beautified view of the returned JSON.

Returns a simple JSON object from a simple, single level natural structure:

```
curl -s "http://localhost:8083/realHTML4Natural/nat/rh4ndemo/shop/articledetail" | jq
{
  "article-id": 4711,
  "article-name": "Article 4711",
  "article-description": "I am just a description",
  "price": "73.63"
}
```

Return a nested natural structure:

```
curl -s "http://localhost:8083/realHTML4Natural/nat/rh4ndemo/shop/order" | jq
{
  "order-id": 936,
  "customer-id": 376,
  "customer-name": "Customer 376",
  "article": [
    {
      "article-id": 4711,
      "article-name": "Article 4711",
      "article-quantity": 2
    },
    {
      "article-id": 815,
      "article-name": "Article 0815",
      "article-quantity": 73
    }
  ]
}
```

Returns the value which is given in the URL parameter `msg`

```
curl -s "http://localhost:8083/realHTML4Natural/nat/rh4ndemo/echo?msg=Hello%20World" | jq
{
  "message": "Hello World"
}
```

Returns a JSON from nested Natural structures

```
curl -s "http://localhost:8083/realHTML4Natural/nat/rh4ndemo/shop/order" | jq
{
  "order-id": 936,
  "customer-id": 376,
  "customer-name": "Customer 376",
  "article": [
    {
      "article-id": 4711,
      "article-name": "Article 4711",
      "article-quantity": 2
    },
    {
      "article-id": 815,
      "article-name": "Article 0815",
      "article-quantity": 73
    }
  ]
}
```