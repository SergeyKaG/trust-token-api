# PST for embedded content providers

Some websites embed media from other providers: videos, reviews/ratings, ads, etc.
Embedded content providers get IVT abuse and would like to limit the damage by rate
limiting the requests from clients. Working with anonymous users, they can't rely
on a 1p cookie alone fro rate limiting: abusers will create fresh identities by
deleting browsing data.

User actions could be limited by requiring a PST redemption every time an action
is made, and limiting PST issuance. PST issuance can be made by the same embedded
content provider based on user activity on other websites.

Here's an example use case. Embed uses SID cookie to join actions performed by
the same client on the same website.

![PST Issuance](https://raw.githubusercontent.com/SergeyKaG/trust-token-api/main/assets/iframe-PST-issuance.png)

- Embed in website sees a repeat visitor, SID cookie is present.
- Embed provider responds with content for the client.
- Embed logs the interaction with a timestamp to their PST Issuer.
- Embed javascript code requests PSTs.
- PST Issuer runs interaction analysis, and possibly returns PSTs.

Analysis can evolve to stay ahead of abusive users adaptation. It could involve
things like lengths of browsing sessions - screen time - over multiple days,
to drive up the cost of synthetic traffic.

Whan a new user visits website B for the first time, and the embed provider doesn't
get SID cookie. Embed provider doesn't know whether it's a new user (and possibly
a bot with a clean browser) or a user with a long history who's just not been on
this website before.

![PST Redemption](https://raw.githubusercontent.com/SergeyKaG/trust-token-api/main/assets/iframe-PST-redemption.png)

Embed javascript sends a request with PST if there is a PST from embed provider's Issuer.
If a PST is redeemed successfully, the new client is treated like a user with
a long history elsewhere. Client is less likely to be a malicious actor. For example,
their likes and ratings don't get quietly ignored.

Client identity creation requires a PST to be redeemed. It is rate limited by issuing PSTs
only after a substantial amount of interaction between the user and another website that
contains content from this embed provider. Client identities can still be created in
excess of the limit, but embed provider would treat them as either bots or brand new users,
and either present a CAPTCHA or assign zero value to their interaction.
