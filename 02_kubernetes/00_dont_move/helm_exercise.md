# Exercise 7 - Hackathon
Migrate the entire existing sunny bikes solution to a Helm chart
- Decide for yourself which variables should be configurable (e.g. the number of replicas)
- Make sure there are no hardcoded values (i.e. the PG_HOST); these should be calculated from variables

Bonus:
- Make persistence optional
- Add horizontal auto-scaling for sunnybikes
- Generate a random password for postgres on install (hint, checkout “randAlphaNum” and this fix for it.