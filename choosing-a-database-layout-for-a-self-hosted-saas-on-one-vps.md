# Choosing a database layout for a self-hosted SaaS on one VPS

A single VPS can run app and Postgres together for years. The decision is about failure modes and growth, not fashion.

## Colocated Postgres

Pros: low latency, one bill, simple private networking. Cons: a noisy neighbor app can starve IO; restoring the DB may require stopping the app; vertical limits arrive together.

Use colocated when traffic is modest, you have tested dumps, and you can tolerate a maintenance window.

## Managed database

Pros: automated backups, easier major version upgrades, disk and compute scaled separately. Cons: network latency, extra cost, and provider-specific auth networking.

Move sooner if sleep matters, if you need replicas, or if compliance wants snapshots you did not invent.

## Extensions and extras

Need `pgvector`, PostGIS or logical decoding? Confirm the managed plan allows them before you rewrite the app. On self-hosted Postgres you install extensions yourself and own the upgrade path.

Whatever you pick, rehearse a restore into an empty database at least once; a dump you have never restored is only a hopeful file.

Peon's approach to provisioning and connecting data stores is summarized under [databases on Peon](https://peon.sh/solutions/databases).

Disclosure: maintained alongside work on [Peon](https://peon.sh/).
