# Working with data through the HTTP interface

In this section, you'll learn how to create a table, add data to it, and then read it using the Document API HTTP interface.

For HTTP queries, the examples in the section use the [cURL](https://curl.se/) utility.

## Before you start {#before-you-begin}

1. If you don't have the {{ yandex-cloud }} CLI, [install and initialize it](../../../cli/quickstart.md#install).
1. Prepare the Document API endpoint of the [created database](../create-db.md).

    {% cut "How to get the Document API endpoint of the database" %}

    1. Go to the [management console]({{ link-console-main }}).
    1. Select the folder and go to **{{ ydb-full-name }}**.
    1. Select the database you want to query.
    1. In the menu on the left, go to the **Overview** section.
    1. The endpoint value is in the **Document API endpoint** line.

        Sample endpoint value: `https://docapi.serverless.yandexcloud.net/ru-central1/b1g4ej5ju4rf5kelpk4b/etn01lrprvnlnhv8v5kj`.

    {% endcut %}

    {% note info %}

    For the Amazon DynamoDB-compatible mode, use a serverless database configuration.

    {% endnote %}

1. Set the `ENDPOINT` environment variable to the prepared value:

    ```bash
    export ENDPOINT=<Document API endpoint>
    ```

## Creating a table {#create-table}

{% list tabs %}

- HTTP interface

  Set the table configuration in the `create.json` file:

    ```json
    {
      "TableName": "test/pets",
      "AttributeDefinitions":
      [
        {
          "AttributeName": "species",
          "AttributeType": "S"
        },
        {
          "AttributeName": "name",
          "AttributeType": "S"
        }
      ],
      "KeySchema":
      [
        {
          "AttributeName": "species",
          "KeyType": "HASH"
        },
        {
          "AttributeName": "name",
          "KeyType": "RANGE"
        }
      ]
    }
    ```

  Create a document table in the database using the command:

    ```bash
    curl \
      -H 'X-Amz-Target: DynamoDB_20120810.CreateTable' \
      -H "Authorization: Bearer $(yc iam create-token)" \
      -H "Content-Type: application.json" \
      -d @create.json $ENDPOINT
    ```

    For more information about the `CreateTable` method, see the [Document API reference](../../docapi/api-ref/actions/createTable.md).

{% endlist %}

## Adding data {#add}

{% list tabs %}

- HTTP interface

  Prepare data to save to the document table by creating a file named `put.json`:

    ```json
    {
      "TableName": "test/pets",
      "Item":
      {
        "species": {"S": "cat"},
        "name":    {"S": "Tom"},
        "color":   {"S": "black"},
        "price":   {"N": "10.5"}
      }
    }
    ```

  Add the data to the table using the command:

    ```bash
    curl \
      -H 'X-Amz-Target: DynamoDB_20120810.PutItem' \
      -H "Authorization: Bearer $(yc iam create-token)" \
      -H "Content-Type: application.json" \
      -d @put.json $ENDPOINT
    ```

    For more information about the `PutItem` method, see the [Document API reference](../../docapi/api-ref/actions/putItem.md).

{% endlist %}

## Reading a record {#read}

{% list tabs %}

- HTTP interface

  To read the data from the document table, run the command:

    ```bash
    curl \
      -H 'X-Amz-Target: DynamoDB_20120810.GetItem' \
      -H "Authorization: Bearer $(yc iam create-token)" \
      -H "Content-Type: application.json" \
      -d '{"TableName": "test/pets", "Key": {"species":{"S":"cat"}, "name":{"S":"Tom"}}}' \
      $ENDPOINT
    ```

    Result:

    ```text
    {"Item":{"name":{"S":"Tom"},"species":{"S":"cat"},"color":{"S":"black"},"price":{"N":"10.5"}}}
    ```

    For more information about the `GetItem` method, see the [Document API reference](../../docapi/api-ref/actions/getItem.md).

{% endlist %}
