
# Deploy Azure Container Instances

## Exercise — Task 1: Create a container instance

- [ ] Sign in to the Azure portal:

    ```
    https://portal.azure.com
    ```

- [ ] From **All services**, search for:

    ```
    Container instances
    ```

    then select it.

- [ ] Select:

    ```
    + Add / + Create / + New
    ```

---

### Configure the Basics tab

- [ ] Subscription:

    ```
    Use default supplied
    ```

- [ ] Resource group:

    ```
    Use default supplied
    ```

- [ ] Container name:

    ```
    mycontainer
    ```

- [ ] Region:

    ```
    (US) East US
    ```

- [ ] Image source:

    ```
    Docker Hub or other registry
    ```

- [ ] Image type:

    ```
    Public
    ```

- [ ] Image:

    ```
    mcr.microsoft.com/azuredocs/aci-helloworld
    ```

- [ ] OS type:

    ```
    Linux
    ```

- [ ] Size → **Leave default**

---

### Configure the Networking tab

- [ ] DNS name label (must be globally unique):

    ```
    mycontainerdnsxxxxx
    ```

    *(Replace **xxxxx** with letters/digits until Azure accepts it)*

- [ ] Do **not** change any other settings.

---

### Configure the Monitoring tab

- [ ] Uncheck:

    ```
    Enable container instance logs
    ```

---

### Finish deployment

- [ ] Select **Review + Create**.

- [ ] Wait for validation.

- [ ] Select **Create**.

- [ ] Monitor deployment through:

    - Deployment page  
    - Notifications (bell icon)

---

## Exercise — Task 2: Verify deployment of the container instance

- [ ] After deployment finishes, select:

    ```
    Go to resource
    ```

- [ ] On the **Overview** blade, verify:

    ```
    Status = Running
    ```

- [ ] Locate the container FQDN.

- [ ] Copy the FQDN into a new browser tab.

- [ ] Press **Enter** and confirm the **Welcome to ACI** page displays.

---

### Optional Cleanup

- [ ] Search for **Resource groups**.

- [ ] Open your resource group.

- [ ] Select:

    ```
    Delete resource group
    ```

- [ ] Type the resource group name to confirm.

- [ ] Monitor deletion in Notifications.
