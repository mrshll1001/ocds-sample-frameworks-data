# Sample OCDS data for framework call-offs

This repository contains sample data for how the various [stages of a framework agreement](http://standard.open-contracting.org/latest/en/implementation/related_processes) may appear in OCDS data. OCDS currently provides support for Direct Call-Offs and Mini-Competitions in Framework Agreements, as well as Dynamic Purchasing Systems.

## Required extensions
Framework Agreements represent many-to-many relationships between a variety of Buyers and Suppliers. This has implications for the structure of the OCDS data as `buyer` is declared at release level and `suppliers` are listed in `award/suppliers`; whereas each call-off from a Framework Agreement might involve a different buyer procuring from a different supplier. There are two extensions available to publishers to allow them to publish accurate Framework data as valid OCDS:

+ [Multiple Buyers - Contract Level Extension](https://extensions.open-contracting.org/en/extensions/contract_buyer/master/) adds a `buyer` reference field to the `Contract` block; allowing the buyer for each Direct Call-Off to be associated with the purchase.
+ [Contract Suppliers Extension](https://extensions.open-contracting.org/en/extensions/contract_suppliers/master/) adds a `suppliers` array to the `Contract` block; allowing the supplier for each Direct Call-Off to be associated with the purchase.

These should be declared appropriately in the package metadata:

```json
{
  "extensions": [
    "https://github.com/open-contracting/ocds_multiple_buyers_extension",
    "https://github.com/open-contracting-extensions/ocds_contract_suppliers_extension"
  ],
  "releases": []
}
```

## Framework agreement within a single publisher
The following examples represent the creation of a framework by Glasgow City, who will be acting as the buyer and procuring entity. It also spans multiple [framework types](http://standard.open-contracting.org/latest/en/implementation/related_processes/) and  will be used for multiple types of call-off; Direct Call-off and Mini-Competition. for the subsequent call-offs and mini-competitions. Please note that in OCDS buyer and procuring entity may be different at times.

In these examples the publisher responsible is *Scottish Government* using their registered prefix of `ocds-r6ebe6`.

### Establishing the framework (Tender and Award)

Full examples:
+ [001_framework_set-up.json](/single_publisher/001_framework_set-up.json)
+ [002_bid-01.json](/single_publisher/002_bid-01.json)
+ [003_bid-02.json](/single_publisher/003_bid-02.json)
+ [004_bid-03.json](/single_publisher/004_bid-03.json)
+ [005_bid-04.json](/single_publisher/005_bid-04.json)
+ [006_bid-05.json](/single_publisher/006_bid-05.json)
+ [007_bid-06.json](/single_publisher/007_bid-06.json)
+ [008_framework_award.json](/single_publisher/008_framework_award.json)

The framework is established by first publishing a tender release opening up the procurement process as any normal contracting process published under OCDS.
```json
{
  "ocid": "ocds-r6ebe6-example_framework",
  "id": "ocds-r6ebe6-example_framework-tender-2019-01-01T00:00:00Z",
  "date": "2019-01-01T00:00:00Z",
  "tag": ["tender"],
  "initiationType": "tender"


}
```

Glasgow City is the one Establishing the framework, so they have an entry in the `parties` array:
```json
{
  "parties": [
    {
      "name": "Glasgow City",
      "id": "GB-LAS-GLG",
      "identifer": {
        "scheme": "GB-LAS",
        "id": "GLG"
      },
      "roles": [
        "procuringEntity"
      ]
    }
  ]
}
```

The `tender` block is populated normally, with information about the framework tender. Glasgow City is the procuring entity so they are referenced in `procuringEntity`.
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

As well as this, in the `tender` block changes are made to add their reference to the list of tenderers, and update the total number of tenderers:

```json
{

  "numberOfTenderers": "1",
  "tenderers": [
    {
      "name": "Supplier 1",
      "id": "GB-COH-00000001-supplier_57"
    }
  ]
}



}
```

A release may be made every time a tenderer places a bid, or a single release may be published to add all tenderers to the data at once. More granularity is better, however, as some data users may be interested to know when individual tenderers enter the procurement process:

```json
{
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
  "parties": [
    {
      "name": "Glasgow City",
      "id": "GB-LAS-GLG",
      "identifer": {
        "scheme": "GB-LAS",
        "id": "GLG"
      },
      "roles": [
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

The framework is now established, and call-offs may now be made.

### Making direct call-offs (Contract)
Full examples:
+ [009_first_direct_calloff.json](/single_publisher/009_first_direct_calloff.json)
+ [010_second_direct_calloff.json](/single_publisher/010_second_direct_calloff.json)

A Direct Call-Off from a Framework Agreement occurs when goods or services are procured directly from a supplier on an existing Framework Agreement without any further tendering process. For example a Framework may be established to supply an office with stationery and a Direct Call-Off may be made to purchase items from this.

Following the previous Framework Agreement, Glasgow now make a Direct Call-Off to Supplier 1. A release is made with the appropriate release information:

```json
{
  "ocid": "ocds-r6ebe6-example_framework",
  "id": "ocds-r6ebe6-example_framework-contract-2019-03-01T00:00:00Z",
  "date": "2019-03-01T00:00:00Z",
  "tag": [
    "contract"
  ]
}
```
The `contracts` array is then added to with the details of the contract, including the supplier and buyer information:

```json
{
  "contracts": [
    {
      "id": "ocds-r6ebe6-example_framework-contract-01",
      "awardID": "ocds-r6ebe6-example_framework-award-01",
      "title": "The First Direct Call-Off",
      "description": "A direct call off to buy things from Supplier 1 ",
      "buyer": {
        "name": "Glasgow City",
        "id": "GB-LAS-GLG"
      },
      "suppliers": [
        {
          "name": "Supplier 1",
          "id": "GB-COH-00000001-supplier_57"
        }
      ]
    }
  ]
}
```

Glasgow City is now a buyer, so their entry in the `parties` array is updated with the role of `"buyer"`:

```json
{
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
  ]
}
```

This is repeated for subsequent Direct Call-Offs.

### Running a mini-competition (`relatedProcess`)
Full Example:
+ [011_mini-competition_tender.json](/single_publisher/011_mini-competition_tender.json)


Another form of calling off from a Framework Agreement is the *Mini Competition*. In this form of purchas from the framework, a separate tendering process is run with participants limited to those already accepted as suppliers.

Representing a Mini-Competition in OCDS is straightforward. Broadly:
+ A *new contracting process* with a *new OCID* is created to represent the Mini Competition
+ In the new process the `relatedProcesses` array contains an entry referencing the OCID of the existing Framework Agreement
+ In the `tender` block of the new process, the `procurementMethod` is set to `limited` or `selective` to represent the fact that this was not an open tender.

> Note: This is a new contracting process where the buyer is known and the suppliers will be determined by the award block. Therefore the schema changes made by [Multiple Buyers - Contract Level Extension](https://extensions.open-contracting.org/en/extensions/contract_buyer/master/) and [Contract Suppliers Extension](https://extensions.open-contracting.org/en/extensions/contract_suppliers/master/) that apply to the Contract block may not be necessary for Mini-Competitions.

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

}
```
From this point the contracting process continues as normal, with the award and contract stages being released under the new OCID created for the Mini-Competition.

## Framework agreement across multiple publishers
Framework Agreements may sometimes span data published by two or more different publishers. For example a Framework Agreement established and published by the UK National Government may be called off by buyers that are published by the Scottish Government or a regional publisher.

 There is very little difference in the OCDS representing a Framework Agreement handled by a single publisher, and a Framework Agreement with which multiple publishers interact. Since the OCID is globally unique it is used by both the publisher representing the framework setup and the publisher representing the call-offs from the framework.

In the following samples, the Framework Agreement is published by *Crown Commercial Services* using their registered prefix of `ocds-b5fd17`. The purchases from the framework are made by entities published by *Scottish Government* using their registered prefix of `ocds-r6ebe6`.

### Publisher 1 sets up the framework (Tender and Award)
Full Examples:
+ [012_two_publishers_framework_setup.json](/multi_publisher/012_two_publishers_framework_setup.json)
+ [013_two_publishers_framework_bids_added.json](/multi_publisher/013_two_publishers_framework_bids_added.json)
+ [014_two_publishers_framework_award.json](/multi_publisher/014_two_publishers_framework_award.json)

The first stages of the Framework Agreement are very similar to that when it only concerns a single publisher. In this sample, Crown Commercial Services commissions a Framework Agreement:

```json
{
  "ocid": "ocds-b5fd17-second_example_framework",
  "id": "ocds-b5fd17-second_example_framework-tender-2019-03-01T00:00:00Z",
  "date": "2019-03-01T00:00:00Z",
  "tag": [
    "tender"
  ],
  "initiationType": "tender",
  "parties": [
    {
      "name": "Crown Commercial Services",
      "id": "GB-GOR-EA1015",
      "identifer": {
        "scheme": "GB-GOR",
        "id": "EA1015"
      },
      "roles": [
        "procuringEntity"
      ]
    }
  ]
}
```
They are the procuring entity of the framework so they are referenced under `tender/procuringEntity`:

```json
{
  "tender": {
    "id": "ocds-b5fd17-second_example_framework-tender",
    "title": "An Example National Framework",
    "description": "An Example National Framework",
    "status": "active",
    "procuringEntity": {
      "name": "Crown Commercial Services",
      "id": "GB-LAS-GLG"
    },
    "value": {
      "amount": "1000000",
      "currency": "GBP"
    }
  }
}
```
Bidders are added to the framework as per the previous example. For brevity this has been condensed to a single release:
```json
{
  "ocid": "ocds-b5fd17-second_example_framework",
  "id": "ocds-b5fd17-second_example_framework-tenderUpdate-2019-03-31T00:00:00Z",
  "date": "2019-03-31T00:00:00Z",
  "tag": [
    "tenderUpdate"
  ],
  "initiationType": "tender",
  "parties": [
    {
      "name": "Crown Commercial Services",
      "id": "GB-GOR-EA1015",
      "identifer": {
        "scheme": "GB-GOR",
        "id": "EA1015"
      },
      "roles": [
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
        "tenderer"
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
        "tenderer"
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
        "tenderer"
      ]
    },
    {
      "name": "Supplier 4",
      "id": "GB-COH-00000001-supplier_60",
      "identifer": {
        "scheme": "GB-COH",
        "id": "00000004"
      },
      "roles": [
        "tenderer"
      ]
    }
  ]
}
```
Remember to update the relevant fields in the `tender` block:

```json
{
  "numberOfTenderers": "4",
  "tenderers": [
    {
      "name": "Supplier 1",
      "id": "GB-COH-00000001-supplier_57"
    },
    {
      "name": "Supplier 2",
      "id": "GB-COH-00000001-supplier_58"
    },
    {
      "name": "Supplier 3",
      "id": "GB-COH-00000001-supplier_59"
    },
    {
      "name": "Supplier 4",
      "id": "GB-COH-00000001-supplier_60"
    }
  ]
}
```

Next the Framework Agreement is finalised. Supplier 1, Supplier 2, and Supplier 3 have made it into the framework. Poor Supplier 4 was the only one excluded:
> Remember to update the entries under `release/parties` as well!
```json
{
  "awards": [
    {
      "id": "ocds-b5fd17-second_example_framework-award",
      "title": "Award of suppliers on the example framework",
      "description": "Suppliers 1, 2, and 3 have been awarded a place on the framework",
      "status": "active",
      "date": "2019-04-01T00:00:00Z",
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
        "startDate": "2019-04-01",
        "endDate": "2020-03-31"
      }
    }
  ]
}
```

The framework is now established, and call-offs may be made from it.

### Buyers under a separate publisher make direct call-offs (Contract)
Full Examples:
+ [015_two_publishers_framework_first_direct-calloff.json](/multi_publisher/015_two_publishers_framework_first_direct-calloff.json)
+ [016_two_publishers_framework_second_direct-calloff.json](/multi_publisher/016_two_publishers_framework_second_direct-calloff.json)

With the Framework Agreement in place and published by *Crown Commercial Services*, it becomes straightforward to represent Direct Call-offs made by another publisher.

A Direct Call-Off is represented by a `contract` block; so an OCDS release is published by *Scottish Government* containing the details of the Call-Off. Since it is published by *Scottish Government*, they use their registered prefix for the release id:
 ```json
 {
   "ocid": "ocds-b5fd17-second_example_framework",
   "id": "ocds-r6ebe6-second_example_framework-contract-2019-04-20 T00:00:00Z",
   "date": "2019-04-20T00:00:00Z",
   "tag": [
     "contract"
   ]
 }
 ```

 The buyer is also added to the `parties` array with the appropriate role, in this case *East Ayrshire*:
 ```json
{
  "parties": [
    {
      "name": "East Ayrshire",
      "id": "GB-LAS-EAY",
      "identifer": {
        "scheme": "GB-LAS",
        "id": "EAY"
      },
      "roles": [
        "buyer",
        "procuringEntity"
      ]
    }
  ]
}
 ```

 As before the contract section refers back to the `awardID` of the Framework Agreement published by *Crown Commercial Services*. This will require access to the Award ID and the OCID of the Framework Agreement:
```json
{
  "contracts": [
    {
      "id": "ocds-r6ebe6-second_example_framework-contract-02",
      "awardID": "ocds-b5fd17-second_example_framework-award"
    }
  ]
}
```
Remember to include the `buyer` and `suppliers` in this section, added by the extensions used:
```json
{
  "buyer": {
    "name": "East Ayrshire",
    "id": "GB-LAS-EAY"
  },
  "suppliers": [
    {
      "name": "Supplier 2",
      "id": "GB-COH-00000001-supplier_58"
    }
  ],
}
```

### A buyer under a separate publisher runs a mini-competition (`relatedProcess`)
Full Example:
+ [017_two_publishers_framework_minicompetition_tender.json](/multi_publisher/017_two_publishers_framework_minicompetition_tender.json)

It is even more straightforward to run a mini-competition calling off of the framework. Since this is a new contracting process, a reference to the original Framework OCID should be included and the contracting process should proceed as a normal process under OCDS.

In this example, Edinburgh are running a mini-competition on the Framework Agreement established previously by *Crown Commercial Services*:

```json
{
  "ocid": "ocds-r6ebe6-minicompetiion_from_other_publisher_framework",
  "id": "ocds-r6ebe6-minicompetiion_from_other_publisher_framework-tender-2019-05-01T00:00:00Z",
  "date": "2019-05-01T00:00:00Z",
  "tag": [
    "tender"
  ],
  "initiationType": "tender",
  "buyer": {
    "name": "Edinburgh",
    "id": "GB-LAS-EDH"
  },
  "parties": [
    {
      "name": "Edinburgh",
      "id": "GB-LAS-EDH",
      "identifer": {
        "scheme": "GB-LAS",
        "id": "EDH"
      },
      "roles": [
        "buyer",
        "procuringEntity"
      ]
    }
  ],
  "relatedProcesses": [
    {
      "id": "ocds-b5fd17-second_example_framework-parent",
      "relationship": "framework",
      "title": "An Example National Framework",
      "scheme": "ocid",
      "identifer": "ocds-b5fd17-second_example_framework",
      "uri": "https://example.org/records/ocds-r6ebe6-example_framework"
    }
  ]
}
```


## Dynamic Purchasing Systems
A Dynamic Purchasing System (DPS) is similar to a Framework Agreement with the exception that new suppliers may be awarded a position on the system at any time.

+ What implications does this have for our award model?
