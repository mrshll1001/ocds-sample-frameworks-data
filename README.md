# Sample OCDS Data for Framework Call-Offs

This repository contains sample data for how the various stages of a framework may appear in OCDS data. OCDS currently provides support for Direct Call-Offs and Mini-Competitions in Framework agreements, as well as Dynamic Purchasing Systems.

## Framework Agreement within a Single Publisher
The following examples represent the creation of a framework by Glasgow City, who will be acting as the buyer and procuring entity for the subsequent call-offs and mini-competitions. Please note that in OCDS buyer and procuring entity may be different at times.

In these examples the publisher responsible is *Scottish Government* using their registered prefix of `ocds-r6ebe6`.

### Setting up the Framework (Tender and Award)

Full examples:
+ [001_framework_set-up.json](/single_publisher/001_framework_set-up.json)
+ [002_bid-01.json](/single_publisher/002_bid-01.json)
+ [003_bid-02.json](/single_publisher/003_bid-02.json)
+ [004_bid-03.json](/single_publisher/004_bid-03.json)
+ [005_bid-04.json](/single_publisher/005_bid-04.json)
+ [006_bid-05.json](/single_publisher/006_bid-05.json)
+ [007_bid-06.json](/single_publisher/007_bid-06.json)
+ [008_framework_award.json](/single_publisher/008_framework_award.json)

The framework is set up by first publishing a tender block opening up the procurement process as any normal contracting process published under OCDS.
```json
{
  "ocid": "ocds-r6ebe6-example_framework",
  "id": "ocds-r6ebe6-example_framework-tender-2019-01-01T00:00:00Z",
  "date": "2019-01-01T00:00:00Z",
  "tag": ["tender"],
  "initiationType": "tender"

  ...
}
```

Glasgow City is the buyer in the framework, as their funds will be used to pay for any subsequent call-offs. They appear as `buyer` in the release, and have an entry in the `parties` array:
```json
{
  ...

  "buyer": {
    "name": "Glasgow City",
    "id": "GB-LAS-GLG"
  },
  "parties": [
    {
      "name": "Glasgow City",
      "id": "GB-LAS-GLG",
      "identifer": {
        "scheme": "GB-LAS",
        "id": "GLG"
      },
      "roles": [
        "buyer",
        "procuringEntity"
      ]
    }
  ],

  ...

}
```

The `tender` block is populated normally, with information about the framework tender. Glasgow City is the procuring entity as well as the buyer, so they are referenced in `procuringEntity`.
```json
{

  "tender": {
    "id": "ocds-r6ebe6-example_framework-tender",
    "title": "An Example Framework",
    "description": "An Example Framework",
    "status": "active",
    "procuringEntity": {
      "name": "Glasgow City",
      "id": "GB-LAS-GLG"
    },
    "value": {
      "amount": "1000000",
      "currency": "GBP"
    }
    ...
  }
}

```

When someone bids for a position on the framework, they are added to the `parties` array with a role of *"tenderer"* since they have not yet been awarded the position.

```json
{
  "parties": [
    {
      "name": "Supplier 1",
      "id": "GB-COH-00000001-supplier_57",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000001"
      },
      "roles": [
        "tenderer"
      ]
    }
  ]
}
```

As well as this, in the  `tender` block changes are made to add their reference to the list of tenderers, and update the total number of tenderers:

```json
{
...
  "numberOfTenderers": "1",
  "tenderers": [
    {
      "name": "Supplier 1",
      "id": "GB-COH-00000001-supplier_57"
    }
  ]
}

...

}
```

A release may be made every time a tenderer places a bid, or a single release may be published to add all tenderers to the data at once. More granularity is better, however, as some data users may be interested to know when individual tenderers enter the procurement process:

```json
{
  ...
  "numberOfTenderers": "6",
  "tenderers": [
    {
      "name": "Supplier 1",
      "id": "GB-COH-00000001-supplier_57"
    },
    {
      "name": "Supplier 2",
      "id": "GB-COH-00000002-supplier_58"
    },
    {
      "name": "Supplier 3",
      "id": "GB-COH-00000001-supplier_59"
    },
    {
      "name": "Supplier 4",
      "id": "GB-COH-00000001-supplier_60"
    },
    {
      "name": "Supplier 5",
      "id": "GB-COH-00000001-supplier_61"
    },
    {
      "name": "Supplier 6",
      "id": "GB-COH-00000001-supplier_62"
    }
  ]
}
```
When a framework is finalised, a release is made for the `award` award stage like a normal contracting process. The successful suppliers will be updated with the role of `supplier`. In this example Supplier 1, Supplier 2, and Supplier 3 have been awarded a position onto the framework.

```json
{
  "ocid": "ocds-r6ebe6-example_framework",
  "id": "ocds-r6ebe6-example_framework-award-2019-02-01T00:00:00Z",
  "date": "2019-02-01T00:00:00Z",
  "tag": [
    "award"
  ],
  "initiationType": "tender",
  "buyer": {
    "name": "Glasgow City",
    "id": "GB-LAS-GLG"
  },
  "parties": [
    {
      "name": "Glasgow City",
      "id": "GB-LAS-GLG",
      "identifer": {
        "scheme": "GB-LAS",
        "id": "GLG"
      },
      "roles": [
        "buyer",
        "procuringEntity"
      ]
    },
    {
      "name": "Supplier 1",
      "id": "GB-COH-00000001-supplier_57",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000001"
      },
      "roles": [
        "tenderer",
        "supplier"
      ]
    },
    {
      "name": "Supplier 2",
      "id": "GB-COH-00000001-supplier_58",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000002"
      },
      "roles": [
        "tenderer",
        "supplier"
      ]
    },
    {
      "name": "Supplier 3",
      "id": "GB-COH-00000001-supplier_59",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000003"
      },
      "roles": [
        "tenderer",
        "supplier"
      ]
    }
  ]
...
}
```

An `awards` entry must also be published with the relevant information about the award, and references to the Suppliers are made in the `suppliers` array. For brevity, this sample shows a single award adding multiple suppliers onto the framework. Publishers seeking extra granularity may publish individual awards for each supplier's place on the contract:

```json

  "awards": [
    {
      "id": "ocds-r6ebe6-example_framework-award-01",
      "title": "Award of suppliers on the example framework",
      "description": "Suppliers 1, 2, and 3 have been awarded a place on the framework",
      "status": "active",
      "date": "2019-02-01T00:00:00Z",
      "value": {
        "amount": 1000000,
        "currency": "GBP"
      },
      "suppliers": [
        {
          "name": "Supplier 1",
          "id": "GB-COH-00000001-supplier_57"
        },
        {
          "name": "Supplier 2",
          "id": "GB-COH-00000002-supplier_58"
        },
        {
          "name": "Supplier 3",
          "id": "GB-COH-00000001-supplier_59"
        }
      ],
      "contractPeriod": {
        "startDate": "2019-02-02",
        "endDate": "2020-01-31"
      }
    }
  ]
```

The framework is now set up, and call-offs may now be made.

### Making Direct Call-Offs (Contract)
Full examples:
+ [009_first_direct_calloff.json](/single_publisher/009_first_direct_calloff.json)
+ [010_second_direct_calloff.json](/single_publisher/010_second_direct_calloff.json)

A Direct Call-Off from a Framework agreement occurs when goods or services are procured directly from a supplier on an existing Framework agreement without any further tendering process. For example a Framework may be set up to supply an office with stationery and a Direct Call-Off may be made to purchase items from this.

In OCDS a Direct Call-Off from a Framework agreement is represented by publishing a new `contract` block for each call-off under the *same OCID as the initial Framework Agreement*. Depending on how the `award` stage of the framework was published an extension may be required. This is because the contract block does not usually contain supplier information. Usually the supplier of a contract is determined looking at the `awardID` referenced in the contract block, and using the supplier information in this award. Therefore two options are available to publishers:

+ If a single `award` was published adding multiple suppliers to the framework, include the [Contract Suppliers Extension](https://github.com/open-contracting-extensions/ocds_contract_suppliers_extension). This adds another `suppliers` array to the contract block.
+ If multiple `award` blocks are created for each supplier being added to the framework, ensure that the contract's `awardID` field is populated appropriately (ie using the correct award for the supplier).

> The following samples presumes a single award adding multiple suppliers to the framework, and the presence of the Contract Suppliers Extension

Following the previous Framework Agreement, Glasgow now make a Direct Call-Off to Supplier 1. A release is made with the appropriate release information:

```json
{
  "ocid": "ocds-r6ebe6-example_framework",
  "id": "ocds-r6ebe6-example_framework-contract-2019-03-01T00:00:00Z",
  "date": "2019-03-01T00:00:00Z",
  "tag": [
    "contract"
  ]
  ...
}
```
The `contracts` array is then added to with the details of the contract, including the supplier information since the previous award added multiple suppliers to the Framework Agreement.

```json
"contracts": [
  {
    "id": "ocds-r6ebe6-example_framework-contract-01",
    "awardID": "ocds-r6ebe6-example_framework-award-01",
    "title": "The First Direct Call-Off",
    "description": "A direct call off to buy things from Supplier 1 ",
    "status": "active",
    "period": {
      "startDate": "2019-03-01T00:00:00Z",
      "endDate": "2019-03-01T00:00:00Z"
    },
    "value": {
      "amount": 1000,
      "currency": "GBP"
    },
    "dateSigned": "2019-03-01T00:00:00Z",
    "suppliers": [
      {
        "name": "Supplier 1",
        "id": "GB-COH-00000001-supplier_57"
      }
    ]
  }
]
```

This is repeated for subsequent Direct Call-Offs.

### Running a Mini-Competition (`relatedProcess`)
Full Example:
+ [011_mini-competition_tender.json](/single_publisher/011_mini-competition_tender.json)


Another form of calling off from a Framework Agreement is the *Mini Competition*. In this form of purchas from the framework, a separate tendering process is run with participants limited to those already accepted as suppliers.

Representing a Mini-Competition in OCDS is straightforward. Broadly:
+ A *new contracting process* with a *new OCID* is created to represent the Mini Competition
+ In the new process the `relatedProcesses` array contains an entry referencing the OCID of the existing Framework Agreement
+ In the `tender` block of the new process, the `procurementMethod` is set to `limited` or `selective` to represent the fact that this was not an open tender

Following the previous sample of the Glasgow City Framework Agreement; after making their Direct Call-Offs Glasgow City hold a Mini-Competition between suppliers on the framework. A new contracting process is created with an entry in `relatedProcesses` referencing the original Framework Agreement:

```json
{
  "ocid": "ocds-r6ebe6-example_framework-competition-01",
  "id": "ocds-r6ebe6-example_framework-competition-01-tender-2019-05-01T00:00:00Z",
  "date": "2019-05-01T00:00:00Z",
  "tag": [
    "tender"
  ],
  "initiationType": "tender",
  "buyer": {
    "name": "Glasgow City",
    "id": "GB-LAS-GLG"
  },
  "parties": [
    {
      "name": "Glasgow City",
      "id": "GB-LAS-GLG",
      "identifer": {
        "scheme": "GB-LAS",
        "id": "GLG"
      },
      "roles": [
        "buyer",
        "procuringEntity"
      ]
    }
  ],
  "relatedProcesses": [
    {
      "id": "ocds-r6ebe6-example_framework-parent",
      "relationship": "framework",
      "title": "An Example Framework",
      "scheme": "ocid",
      "identifer": "ocds-r6ebe6-example_framework",
      "uri": "https://example.org/records/ocds-r6ebe6-example_framework"
    }
  ],
  ...
}
```
Since this is a `tender` release the `tender` block contains information about the tender opportunity. The `procurementMethod` is set to `selective` to indicate that this is not an open tender.

```json
"tender": {
  "id": "ocds-r6ebe6-example_framework-tender",
  "title": "An Example Mini Competition",
  "description": "A mini competition run off of the original framework",
  "status": "active",
  "procurementMethod": "selective",
  "procuringEntity": {
    "name": "Glasgow City",
    "id": "GB-LAS-GLG"
  },
  "value": {
    "amount": "2000",
    "currency": "GBP"
  }
  ...
}
```
From this point the contracting process continues as normal, with the award and contract stages being released under the new OCID created for the Mini-Competition.
## Framework Agreement across Multiple Publishers
There is very little difference in the OCDS representing a framework agreement handled by a single publisher, and a framework agreement with which multiple publishers interact. Since the OCID is globally unique it is used by both the publisher representing the framework setup and the publisher representing the call-offs from the framework.

### Publisher 1 sets up the Framework (Tender and Award)

### Buyers under a separate publisher make Direct Call-Offs (Contract)

### A buyer under a separate publisher runs a mini-competition (relatedProcess)

## Dynamic Purchasing Systems
A Dynamic Purchasing System (DPS) is similar to a framework agreement with the exception that new suppliers may be awarded a position on the system at any time.

TODO finish this
