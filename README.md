# peptide-db

> ## ⚠️ Read this first
>
> **This is not medical advice, and nothing here is a recommendation to use any substance.**
>
> This is an **open, community-edited** dataset. Anyone can propose changes. It is **not curated, verified, or endorsed by any medical professional**, and its accuracy, completeness, and currency are **guaranteed by no one** — not the maintainers, not the contributors, and not any app that displays it. A merged pull request is not an endorsement of its content.
>
> - **Dosing schedules** in this dataset describe what sources *report*. They are not instructions, and they are not prescriptions.
> - **Pharmacokinetic values** (half-life, tMax, bioavailability) may be wrong, provisional, derived from animal data, or missing entirely. Many peptides here have **no published human data at all**. Any app drawing a concentration curve from these numbers is drawing an estimate, not a fact.
> - Peptides referenced here may be **unapproved, investigational, or regulated substances** in your jurisdiction, and some are banned in competitive sport.
>
> Always consult a licensed healthcare provider before making any health decision. **Use entirely at your own risk.**

An open, community-maintained dataset of peptides: tags, reported benefits, commonly reported protocols, compatibility notes, pharmacokinetics, and source links.

This dataset powers the "Peptide Database" browser in the [Peptide Log](https://apps.apple.com/app/id6744315346) iOS app, and anyone is welcome to use it or contribute to it.

## The data

Everything lives in [`peptides.json`](peptides.json):

```jsonc
{
  "schemaVersion": 1,        // bumped on breaking structure changes
  "updatedAt": "2026-07-11", // date of last content change
  "license": "CC0-1.0",
  "entries": [
    {
      "id": "bpc-157",       // stable slug, never reused
      "name": "BPC-157",
      "tags": ["Healing"],
      "benefits": "...",
      "protocols": "...",
      "goodWith": "...",
      "notGoodWith": "...",
      "halfLife": "...",     // free text
      "notes": "...",
      "link": "https://...", // primary source / further reading
      "sideEffects": "...",

      // optional, structured fields consumed by apps when present:
      "formFactor": "Injectable",   // Injectable | Oral | Nasal
      "halfLifeHours": 4,           // numeric convenience form of halfLife
                                    // (midpoint for simple ranges)
      "tMaxHours": 36,              // time to peak concentration
      "bioavailability": 1.0,       // 0..1
      "modelType": "longActing",    // legacy | rapidClearance | longActing | depot
      "effectDurationDays": 7
    }
  ]
}
```

Optional fields may be absent; consumers must fall back gracefully. They are additive within schema v1.

### Sourcing rule for PK values

`halfLifeHours`, `tMaxHours`, `bioavailability`, `modelType`, and `effectDurationDays` drive concentration curves in downstream apps. A wrong number there is worse than no number.

**Every PK value must carry a citation** in the entry's `sources` array:

```jsonc
"sources": [
  { "field": "halfLifeHours", "value": 168, "url": "https://...", "note": "HUMAN, FDA label, Ozempic §12.3" }
]
```

Acceptable sources: FDA/EMA labels, peer-reviewed literature, manufacturer prescribing information. Not acceptable: vendor sites, forums, blogs. If only animal or in-vitro data exists, the value may be included but the note **must** say so. If no credible published value exists, **omit the field** — most research peptides genuinely have no human PK, and a blank is the honest answer.

Consume it raw:

```
https://raw.githubusercontent.com/Johannes-ece/peptide-db/main/peptides.json
```

or via CDN (cached, for higher-traffic use):

```
https://cdn.jsdelivr.net/gh/Johannes-ece/peptide-db@main/peptides.json
```

## Contributing

Open a pull request that edits `peptides.json`:

- Keep the `id` of existing entries unchanged; new entries get a lowercase-dash slug of the name.
- Bump `updatedAt`.
- Cite a source in `link` for new entries or substantive claims.
- Plain, factual language. No marketing, no medical recommendations, no dosing advice presented as instruction.

Maintainers review PRs for formatting and obvious vandalism only. A merged PR is not an endorsement of its content.

## Disclaimer

See the notice at the top of this file. In short: community-edited, unverified, not medical advice, no warranty of any kind, use entirely at your own risk.

## License

[CC0 1.0](LICENSE): public domain dedication. Do whatever you want with the data; attribution appreciated but not required.
