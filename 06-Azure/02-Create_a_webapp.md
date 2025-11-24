
# Create a Web App

## Exercise — Task 1: Create a Web App

- [ ] Sign in to the Azure portal:

    ```
    https://portal.azure.com
    ```

- [ ] From **All services**, search for:

    ```
    App Services
    ```

    then select it.

- [ ] Select:

    ```
    + Create → + Web App
    ```

---

### Configure the Basics tab

- [ ] Subscription → **Use default supplied**
- [ ] Resource Group → **Use default supplied**

- [ ] Web App name (must be globally unique):

    ```
    myDockerWebAppxxxx
    ```

    *(Replace **xxxx** with unique letters/numbers)*

- [ ] Publish:

    ```
    Container
    ```

- [ ] Operating System:

    ```
    Linux
    ```

- [ ] Region:

    ```
    East US
    ```

- [ ] Select **Next → Database**.

- [ ] Select **Next → Container**.

---

### Configure Container Settings

- [ ] Image Source:

    ```
    Other container registries
    ```

- [ ] Access Type:

    ```
    Public
    ```

- [ ] Registry server URL:

    ```
    https://mcr.microsoft.com
    ```

- [ ] Image and tag:

    ```
    azuredocs/aci-helloworld
    ```

*(No startup command required.)*

---

### Finish Creation

- [ ] Select **Review + create**.

- [ ] Select **Create**.

---

## Exercise — Task 2: Test the Web App

- [ ] Wait for deployment to complete.

- [ ] In **Notifications**, select:

    ```
    Go to resource
    ```

- [ ] On the **Overview** blade, locate **Default Domain**.

- [ ] Copy the URL.

- [ ] Open a **new browser window**, paste the URL, press **Enter**.

- [ ] Verify the page displays:

    ```
    Welcome to Azure Container Instances!
    ```

---

### Review telemetry

- [ ] Return to the Web App **Overview** blade.

- [ ] Scroll to the monitoring charts.

- [ ] Refresh or reload the Web App URL a few times.

- [ ] Verify charts show updated:

    - Requests
    - Data In/Out
    - Average Response Time

---

### Optional Cleanup

- [ ] Search for **Resource groups**.
- [ ] Open your resource group.
- [ ] Select **Delete resource group**.
- [ ] Confirm the name and delete.