# Creating a Simple Asset Management API with Azure Functions

This guide walks you through creating a simple mock API for an asset management application using Azure Functions. The API will expose a single operation to return a list of assets. You will also learn how to create a custom connector in Power Platform using API Management.

> [!IMPORTANT] This guide assumes you have access to the Azure Portal and a Power Apps environment with Code App capabilities enabled.

## Prerequisites

- Access to the [Azure Portal](https://portal.azure.com)
- Power Platform environment with Code Apps enabled

## Create a New Azure Function in the Azure Portal 

1. Go to the [Azure Portal](https://portal.azure.com/).

1. Click **Create a resource** > **Web** > **Function App**.

1. Select **Consumption**.

1. Fill in the required details:
   - **Subscription**: Select your subscription.
   - **Resource Group**: Create a new one or use an existing one.
   - **Function App name**: Choose a unique name.
   - **Runtime stack**: Node.js
   - **Region**: Choose a region close to you.

1. Click **Review + create** and then **Create**.
![Create Function in Azure](../contentMedia/Custom-Connector-create-azure-function.png)

1. Once deployment is complete, go to your new Function App.

1. Select **Create function**.

1. If asked, select: **Development environment: Develop in portal**.

1. Select **HTTP trigger** as the template, give it the name `GetAssets`, and set **Authorization level** to **Anonymous** (no authentication required).
![Create HTTP trigger](../contentMedia/Custom-Connector-create-GetAssets.png)

1. Click **Create** to create the function.

1. In the **Code + Test** tab, replace the function code with the mock API code from the next section.

1. Click **Save**.

## Implement the Mock API

Edit `GetAssets/index.js` to return a mock list of assets:

```js
module.exports = async function (context, req) {
    context.res = {
        // status: 200, /* Defaults to 200 */
        body: [
            { id: 1, name: "Laptop", type: "Electronics", status: "Available" },
            { id: 2, name: "Projector", type: "Electronics", status: "In Use" },
            { id: 3, name: "Desk", type: "Furniture", status: "Available" },
            { id: 4, name: "Office Chair", type: "Furniture", status: "In Use" },
            { id: 5, name: "Monitor", type: "Electronics", status: "Available" },
            { id: 6, name: "Whiteboard", type: "Office Supply", status: "Available" },
            { id: 7, name: "Phone", type: "Electronics", status: "In Use" },
            { id: 8, name: "Tablet", type: "Electronics", status: "Available" },
            { id: 9, name: "Printer", type: "Electronics", status: "Maintenance" },
            { id: 10, name: "Filing Cabinet", type: "Furniture", status: "Available" }
        ]
    };
};
```

## Expose Your Function App via API Management

1. In the Azure Portal, search for and select **API Management services**.

1. Click **+ Create** to create a new API Management instance (the Developer tier is free for development/testing).

1. Fill in the required details and deploy the instance.

1. Once deployed, open your API Management instance.

1. In the left menu, select **APIs** > **+ Add API** > **Function App**.
![Add API](../contentMedia/Custom-Connector-APIM-create-api.png)

1. Select your Function App and the `GetAssets` function. 
![Import API](../contentMedia/Custom-Connector-APIM-import-api.png)

1. Complete the wizard to import your function as an API operation.

1. After import, go to your API in API Management.

1. In the top menu, select **Settings** for your API.

1. Under **Security**, set **Subscription required** to **Off**. This will remove the need for a subscription key (API key) when calling the API.
![Update security settings](../contentMedia/Custom-Connector-APIM-security-subscription.png)

1. Save your changes.

## Create a Custom Connector in Power Platform Using API Management (from Azure Portal)

1. In **API Management Services** In the left menu within APIs, select **Power Platform**.

1. Select **Create a connector**.

1. Select Your API
![Create custom connector](../contentMedia/Custom-Connector-APIM-create-connector.png)

1. Select your environment, display name and select **Create**. The custom connector will be created in your selected environment.

1. In the [Power Apps maker portal](https://make.powerapps.com/), go to **Custom connectors** to review, edit, and test your new connector.
![Test custom connector](../contentMedia/Custom-Connector-test-connector.png)

## Next Steps
- Create a new Code App using this custom connector with **Power Apps SDK** 
- Expand the API with more operations as needed. Don't forget to update the connector with the new operations when you do.
- Secure your API if you move beyond development/testing.

---
This simple API provides a mock asset list and is ready for integration with Power Platform using a custom connector via API Management.
