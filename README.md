# Artsy (artsy)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Artsy is the world's largest online art marketplace, connecting collectors with artists and galleries worldwide. The platform features over 1 million artworks from 100,000+ artists and provides access to galleries, art fairs, and auction houses globally. Artsy offers a Public API providing access to images of historic artwork and related information for educational and non-commercial purposes, with access limited to public domain works. The API provides resources for artists, artworks, galleries, shows, sales, and gene (classification) data. Note that the public API may be retired; partner integrations are handled through a separate partner API program.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/artsy/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - Art, Marketplace, Artists, Collectors, Galleries

## Timestamps

- **Created:** 2025-02-24
- **Modified:** 2026-04-19

## APIs

### Artsy Public API
The Artsy Public API provides access to images of historic artwork and related information on artsy.net for educational and non-commercial purposes. Resources include artists, artworks, editions, fairs, genes (art classification taxonomy), images, shows, collections, partners, profiles, search, sales, bids, and bidder positions.

**Human URL:** [https://developers.artsy.net/](https://developers.artsy.net/)

#### Tags:

 - Art, Artists, Artwork, Galleries, Search

#### Properties

- [Documentation](https://developers.artsy.net/v2)
- [GettingStarted](https://developers.artsy.net/v2/docs/authentication)
- [Authentication](https://developers.artsy.net/v2/docs/authentication)

## Common Properties

- [Artsy Website](https://www.artsy.net/)
- [Developer Documentation](https://developers.artsy.net/)
- [Engineering Blog](https://artsy.github.io/)
- [Artsy GitHub Organization](https://github.com/artsy)
- [Sign Up](https://www.artsy.net/signup)
- [Login](https://www.artsy.net/login)

## Features

| Name | Description |
|------|-------------|
| Artist and Artwork Data | Comprehensive database of artist biographies, artwork metadata, images, dimensions, medium, and provenance for over 1 million artworks. |
| Gallery and Show Data | Access to gallery profiles, exhibition shows, art fairs, and partner information from Artsy's global gallery network. |
| Art Classification Genes | Artsy's proprietary gene taxonomy for classifying artworks by style, period, subject matter, and medium, enabling sophisticated art discovery and recommendation. |
| Auction and Sales Data | Sale, bidder, and bid information for auction and buy-now listings on the Artsy platform. |
| Full-Text Search | Search across artists, artworks, galleries, and other art world entities with faceted filtering and relevance ranking. |

## Use Cases

| Name | Description |
|------|-------------|
| Art Education Applications | Educational platforms use the Artsy API to access public domain artwork images and artist information for art history curriculum and museum education tools. |
| Gallery Integration | Partner galleries integrate with the Artsy Partner API to manage artwork listings, track collector inquiries, and access sales analytics. |
| Art Discovery Tools | Developers build art recommendation and discovery applications using Artsy's gene taxonomy and artist relationship data. |
| Research and Analysis | Art market researchers access Artsy data to analyze trends, auction results, and artist career trajectories. |

## Integrations

| Name | Description |
|------|-------------|
| Artsy Partner Program | Gallery and auction house partners integrate directly with Artsy through the Partner API for full marketplace integration including artwork listings and collector communications. |

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
