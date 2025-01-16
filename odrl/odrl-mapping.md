# This file contains a mapping from iSHARE licenses to ODRL

This file contains ODRL representations of iSHARE [licenses](https://framework.ishare.eu/detailed-descriptions/functional/licenses).

This file was created by TNO.

## Nextpolicy

> [nextPolicy documentatie](https://www.w3.org/TR/odrl-vocab/#term-nextPolicy)

### Enrich profile for licenses 0002, 0004-0007

> [Profile best practices](https://www.w3.org/community/reports/odrl/CG-FINAL-profile-bp-20240808.html#profdef)
> [Example ODRL profile](https://raw.githubusercontent.com/Fraunhofer-IESE/ODS/refs/heads/main/ods-profile.ttl)

In de `...` staat informatie als prefixes en verdere specificatie van het profile. `exns` is de example namespace onder welke ons profile valt.

```json
[
    ...
    {
        "@id": "exns:enrich",
        "@type": [
            ":Action",
            "skos:Concept"
        ],
        "rdfs:isDefinedBy": {
            "@id": "exns"
        },
        "includedIn": {
            "@id": "odrl:use"
        },
        "rdfs:label": {
            "@value": "Enrich",
            "@language": "en"
        },
        "skos:definition": {
            "@value": "To improve the quality of a dataset by adding other data to it.",
            "@language": "en"
        },
    },
    {
        "@id": "exns:sourceOrigin",
        "@type": [
            ":LeftOperand",
            "owl:NamedIndividual",
            "skos:Concept"
        ],
        "rdfs:isDefinedBy": {
            "@id": "exns"
        },
        "rdfs:label": {
            "@value": "Source Origin",
            "@language": "en"
        },
        "skos:definition": {
            "@value": "The origin of the data used to modify or enrich another data object.",
            "@language": "en"
        },
        "skos:note": {
            "@value": "The source of the data can be exns:internal or exns:external.",
            "@language": "en"
        }
    },
    {
        "@id": "exns:internal",
        "@type": [
            ":RightOperand",
            "owl:NamedIndividual",
            "skos:Concept"
        ],
        "rdfs:isDefinedBy": {
            "@id": "exns"
        },
        "rdfs:label": {
            "@value": "Internal",
            "@language": "en"
        },
        "skos:definition": {
            "@value": "Meaning that something is owned by or intended to be used by within the assignee.",
            "@language": "en"
        }
    },
    {
        "@id": "exns:external",
        "@type": [
            ":RightOperand",
            "owl:NamedIndividual",
            "skos:Concept"
        ],
        "rdfs:isDefinedBy": {
            "@id": "exns"
        },
        "rdfs:label": {
            "@value": "External",
            "@language": "en"
        },
        "skos:definition": {
            "@value": "Meaning that something is owned by or intended to be used within some party other than the assignee.",
            "@language": "en"
        }
    }
]
```

### License 0000: No limitations
```json
{
    "@context": "http://www.w3.org/ns/odrl.jsonld",
    "@type": "Set",
    "uid": "https://example.com/license/0000",
    "permissions": [
        {
            "target": "https://example.com/resource-id",
            "action": "use",
            "nextPolicy": "https://example.com/data-space-agreement"
        }
    ]
}
```

### License 0001: Re-sharing with Adhering Parties only
> [isPartOf documentatie](https://www.w3.org/TR/odrl-vocab/#term-isPartOf)
```json
{
    "@context": "http://w3.org/ns/odrl.jsonld",
    "@type": "Set",
    "uid": "https://example.com/license/0001",
    "permissions": [
        {
            "target": "https://example.com/resource-id",
            "action": "distribute",
            "constraints": [
                {
                    "leftOperand": "assignee",
                    "operator": "isPartOf",
                    "rightOperand": "https://example.com/adhering-parties"
                }
            ],
            "nextPolicy": "https://example.com/data-space-agreement"
        }
    ]
}
```

### License 0002: Internal use only
> [isPartOf documentatie](https://www.w3.org/TR/odrl-vocab/#term-isPartOf)

```json
{
    "@context": [
        "http://w3.org/ns/odrl.jsonld",
        { "exns": "https://example.com/profile" }
    ],
    "@type": "Set",
    "uid": "https://example.com/license/0002",
    "permissions": [
        {
            "target": "https://example.com/resource-id",
            "action": "use",
            "constraints": [
                { 
                    "leftOperand": "purpose",
                    "operator": "eq",
                    "rightOperand": "internal"
                }
            ]
            "nextPolicy": "https://example.com/data-space-agreement"
        }
    ]
}
```

### License 0003: Non-commercial use only: licensee may not use the data to generate revenue
> [purpose documentatie](https://www.w3.org/TR/odrl-vocab/#term-purpose)

```json
{
    "@context": "http://w3.org/ns/odrl.jsonld",
    "@type": "Set",
    "uid": "https://example.com/license/0003",
    "permissions": [
        {
            "target": "https://example.com/resource-id",
            "action": "use",
            "constraints": [
                {
                    "leftOperand": "purpose",
                    "operator": "eq",
                    "rightOperand": "Non-commercial use"
                }
            ],
            "nextPolicy": "https://example.com/data-space-agreement"
        }
    ]
}
```

### License 0004: Licensee may enrich received data with own data before re-sharing
```json
{
    "@context": [
        "http://w3.org/ns/odrl.jsonld",
        { "exns": "https://example.com/profile" }
    ],
    "@type": "Set",
    "uid": "https://example.com/license/0004",
    "permissions": [
        {
            "target": "https://example.com/resource-id",
            "action": "exns:enrich",
            "constraints": [
                {
                    "leftOperand": "exns:enrichmentSource",
                    "operator": "eq",
                    "rightOperand": "exns:internal"
                }
            ],
            "nextPolicy": "https://example.com/data-space-agreement"
        },
        {
            "target": "https://example.com/resource-id",
            "action": "distribute",
            "nextPolicy": "https://example.com/data-space-agreement"
        }
    ]
}
```

### License 0005: Licensee may enrich received data with data of others before re-sharing
```json
{
    "@context": [
        "http://w3.org/ns/odrl.jsonld",
        { "exns": "https://example.com/profile" }
    ],
    "@type": "Set",
    "uid": "https://example.com/license/0005",
    "permissions": [
        {
            "target": "https://example.com/resource-id",
            "action": "exns:enrich",
            "constraints": [
                {
                    "leftOperand": "exns:enrichmentSource",
                    "operator": "eq",
                    "rightOperand": "exns:external"
                }
            ],
            "nextPolicy": "https://example.com/data-space-agreement"
        },
        {
            "target": "https://example.com/resource-id",
            "action": "distribute",
            "nextPolicy": "https://example.com/data-space-agreement"
        }
    ]
}
```

### License 0006: Licensee may enrich received data with own data before re-sharing on a non-commercial basis
```json
{
    "@context": [
        "http://w3.org/ns/odrl.jsonld",
        { "exns": "https://example.com/profile" }
    ],
    "@type": "Set",
    "uid": "https://example.com/license/0006",
    "permissions": [
        {
            "target": "https://example.com/resource-id",
            "action": "exns:enrich",
            "constraints": [
                {
                    "leftOperand": "exns:enrichmentSource",
                    "operator": "eq",
                    "rightOperand": "exns:internal"
                },
                {
                    "leftOperand": "purpose",
                    "operator": "eq",
                    "rightOperand": "Non-commercial use"
                }
            ],
            "nextPolicy": "https://example.com/data-space-agreement"
        },
        {
            "target": "https://example.com/resource-id",
            "action": "distribute",
            "constraints": [
                {
                    "leftOperand": "purpose",
                    "operator": "eq",
                    "rightOperand": "Non-commercial use"
                }
            ],
            "nextPolicy": "https://example.com/data-space-agreement", 
        }
    ]
}
```

### License 0007: Licensee may enrich received data with data of others before re-sharing on a non-commercial basis
```json
{
    "@context": [
        "http://w3.org/ns/odrl.jsonld",
        { "exns": "https://example.com/profile" }
    ],
    "@type": "Set",
    "uid": "https://example.com/license/0006",
    "permissions": [
        {
            "target": "https://example.com/resource-id",
            "action": "exns:enrich",
            "constraints": [
                {
                    "leftOperand": "exns:enrichmentSource",
                    "operator": "eq",
                    "rightOperand": "exns:external"
                },
                {
                    "leftOperand": "purpose",
                    "operator": "eq",
                    "rightOperand": "Non-commercial use"
                }
            ],
            "nextPolicy": "https://example.com/data-space-agreement"
        },
        {
            "target": "https://example.com/resource-id",
            "action": "distribute",
            "constraints": [
                {
                    "leftOperand": "purpose",
                    "operator": "eq",
                    "rightOperand": "Non-commercial use"
                }
            ],
            "nextPolicy": "https://example.com/data-space-agreement", 
        }
    ]
}
```

### License 9999: As determined between parties
> [inheritFrom documentatie](https://www.w3.org/TR/odrl-model/#inheritance)

```json
{
    "@context": "http://w3.org/ns/odrl.jsonld",
    "@type": "Set",
    "uid": "https://example.com/license/9999",
    "inheritFrom": "https://example.com/custom-negotiation",
}
```
