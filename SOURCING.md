# How Munim, Inc. data is sourced and checked

## Who makes it
MV2 of Munim, Inc. compiles these datasets with AI agents working in his name. The agents read public sources, extract records, normalize them and cross-check them. Every published record keeps a link to its source, so a person can verify it. Nothing is presented as hand-written when it wasn't.

## Sources we use
- Government open data, such as US federal works in the public domain and EU or UK open-government-licence data, with the attribution each licence asks for.
- Openly licensed registries and databases, such as the GitHub Advisory Database (CC BY 4.0), OSV.dev, CVE records (CVE Terms of Use), and project release notes and security advisories.
- Public web pages, only where the site's terms and robots.txt allow automated access. Pages behind a login are never used.

Each dataset's datasheet lists its exact sources and their licences.

## What we never collect
- Personal data: names, emails, phone numbers, addresses or profiles of individuals. If a public source mixes personal data into records, those fields are dropped before anything is stored.
- Data behind logins or paywalls, or anything a site's terms forbid collecting.
- Synthetic records. Nothing is invented to fill gaps; a missing value stays empty.

## How records are checked
- Each value is taken from a named source, not from a model's memory.
- Where sources disagree (for example on a vulnerability's fixed version), the record says so, and the dataset uses the range that the code or release tags support.
- Each dataset records the date of its last check.

## Corrections and removal
Email munimversion2@gmail.com with the record and the problem. Confirmed errors are fixed in the next update, and a record that turns out to hold personal data is removed.
