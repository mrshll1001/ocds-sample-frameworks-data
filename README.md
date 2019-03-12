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

### Making Direct Call-Offs (Contract)

### Running a Mini-Competition (relatedProcess)

## Framework Agreement across Multiple Publishers
There is very little difference in the OCDS representing a framework agreement handled by a single publisher, and a framework agreement with which multiple publishers interact. Since the OCID is globally unique it is used by both the publisher representing the framework setup and the publisher representing the call-offs from the framework.

### Publisher 1 sets up the Framework (Tender and Award)

### Buyers under a separate publisher make Direct Call-Offs (Contract)

### A buyer under a separate publisher runs a mini-competition (relatedProcess)

## Dynamic Purchasing Systems
A Dynamic Purchasing System (DPS) is similar to a framework agreement with the exception that new suppliers may be awarded a position on the system at any time.

TODO finish this
