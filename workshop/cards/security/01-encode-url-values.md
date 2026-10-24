---
category: security
bcquality_domain: security
tag: bites
seed: workshop/AmuseYou/RentalOperations/src/WeatherApiClient.Codeunit.al
---

# Must every value concatenated into an outbound URL be encoded or validated, even for a read-only call?

**Side A:** Yes. Location is text a user typed. Unencoded values can change the query, add parameters or break the request.

**Side B:** No. The value comes from our own master data, the call is read-only, and the endpoint ignores unknown parameters. Encoding everything is cargo cult.
