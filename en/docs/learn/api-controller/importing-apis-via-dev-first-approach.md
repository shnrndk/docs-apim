# Importing APIs Via Dev First Approach

WSO2 API Controller (**apictl**) allows you to create and deploy APIs without using WSO2 API Publisher Portal. You can use this feature to create an API **from scratch** or **using an existing Swagger or Open API specification** and then deploy it to the desired API Manager environment.

!!! info
    **Before you begin...** 

    -   Make sure that the WSO2 API Manager CTL Tool is downloaded and initialized, if not, follow the steps in [Download and Initialize the CTL Tool]({{base_path}}/learn/api-controller/getting-started-with-wso2-api-controller/#download-and-initialize-the-ctl-tool).

    -   Make sure you already have added an environment using the CTL tool for the API Manager environment you plan to import the API to. 

        If not, follow the steps in [Add an Environment]({{base_path}}/learn/api-controller/getting-started-with-wso2-api-controller#add-an-environment).

## Initialize an API Project

1. Open a terminal window and navigate to the path you need to create the project.

2. You can follow either of the following ways to generate the project.

    1.   **From Scratch**
        -   Command
            ```bash
            apictl init <Project Path>   
            ```
            ```bash
            apictl init <Project Path> --definition <API definition template file>  --force=<force create project>
            ```

            !!! example
                ```bash
                apictl init SampleAPI                
                ```
                ```bash 
                apictl init SampleAPI --definition definition.yaml --force=true                
                ```
             
            The following is a sample API definition that can be used to generate an API Project.
             
            !!! example
                ```yaml
                id:
                  providerName: admin
                  apiName: PizzaShackAPI
                  version: 1.0.0
                uuid: febac429-0387-42d3-aec0-fb4ba9511260
                description: This is a simple API for Pizza Shack online pizza delivery store.
                type: HTTP
                context: /pizzashack/1.0.0
                contextTemplate: /pizzashack/{version}
                tags:
                 - pizza
                documents: []
                lastUpdated: Dec 21, 2020 11:44:29 AM
                availableTiers:
                 -
                  name: Unlimited
                  displayName: Unlimited
                  description: Allows unlimited requests
                  requestsPerMin: 2147483647
                  requestCount: 2147483647
                  unitTime: 0
                  timeUnit: ms
                  tierPlan: FREE
                  stopOnQuotaReached: true
                availableSubscriptionLevelPolicies: []
                apiLevelPolicy: Unlimited
                uriTemplates: []
                apiHeaderChanged: false
                apiResourcePatternsChanged: false
                status: PUBLISHED
                technicalOwner: John Doe
                technicalOwnerEmail: architecture@pizzashack.com
                businessOwner: Jane Roe
                businessOwnerEmail: marketing@pizzashack.com
                visibility: public
                gatewayLabels: []
                endpointSecured: false
                endpointAuthDigest: false
                transports: http,https
                advertiseOnly: false
                apiOwner: admin
                subscriptionAvailability: all_tenants
                corsConfiguration:
                  corsConfigurationEnabled: false
                  accessControlAllowOrigins:
                   - '*'
                  accessControlAllowCredentials: false
                  accessControlAllowHeaders:
                   - authorization
                   - Access-Control-Allow-Origin
                   - Content-Type
                   - SOAPAction
                  accessControlAllowMethods:
                   - GET
                   - PUT
                   - POST
                   - DELETE
                   - PATCH
                   - OPTIONS
                endpointConfig: '{"endpoint_type":"http","sandbox_endpoints":{"url":"https://localhost:9443/am/sample/pizzashack/v1/api/"},"production_endpoints":{"url":"https://localhost:9443/am/sample/pizzashack/v1/api/"}}'
                responseCache: Disabled
                cacheTimeout: 300
                implementation: ENDPOINT
                authorizationHeader: Authorization
                scopes: []
                isDefaultVersion: false
                isPublishedDefaultVersion: false
                environments:
                 - Production and Sandbox
                createdTime: "1608531265115"
                additionalProperties: {}
                monetizationProperties: {}
                isMonetizationEnabled: false
                environmentList:
                 - SANDBOX
                 - PRODUCTION
                apiSecurity: oauth2,oauth_basic_auth_api_key_mandatory
                endpoints: []
                enableSchemaValidation: false
                accessControl: all
                rating: 0.0
                isLatest: true
                ```

        -   Response    
            ```go
            Initializing a new WSO2 API Manager project in /home/user/work/SampleAPI
            Project initialized
            Open README file to learn more
            ```
        
            !!! info
                In this case, the project artifacts are generated according to a set of predefined templates. Therefore, after initializing the project, you need to go and edit the project artifacts manually.     


    2.   **From OpenAPI/Swagger Specification**  
        You can use a Swagger2 and OpenAPI3 specification to generate an API. The file format should be YAML or JSON.

        -   Command
            ```bash
            apictl init <Project Path> --oas <Path to API specification>
            ```
            ```bash
            apictl init <Project Path> --oas <Path to API specification> --definition <API definition template file> --force=<force create project>
            ```

            !!! example
                ```bash
                apictl init Petstore --oas petstore.yaml
                ```
                ```bash
                apictl init Petstore --oas https://petstore.swagger.io/v2/swagger.json
                ```
                ```go
                apictl init Petstore --oas petstore.yaml --definition definition.yaml --force=true
                ```

        -   Response

            ```go
            Initializing a new WSO2 API Manager project in /home/user/work/PetstoreAPI
            Project initialized
            Open README file to learn more
            ```

            !!! info
                When you initialize an API project using an OpenAPI specification, the CTL Tool will automatically read the OpenAPI definition and populate a certain set of configs in the API definition file, `api.yaml`.     

            !!! info
                **Flags:**  
                    
                -   Optional :  
                        `--definition` or `-d` : Provide a YAML definition of API  
                        `--oas` : Provide an OpenAPI specification file/URL for the API   
                        `--force` or `-f` : To overwrite the directory and create project 


     A project folder with the following default structure will be created in the given directory.

    ``` java
    ├── api_params.yaml
    ├── Docs
    │    └── FileContents
    ├── Image
    ├── Meta-information
    │    ├── api.yaml
    │    └── swagger.yaml
    ├── README.md
    └── Sequences
        ├── fault-sequence
        ├── in-sequence
        └── out-sequence
    ```

    <table>
        <thead>
            <tr class="header">
                <th>Sub Directory/File</th>
                <th>Description</th>
            </tr>
        </thead>
        <tbody>
            <tr class="odd">
                <td><code>api.yaml</code></td>
                <td>The specification of the created API.</td>
            </tr>
            <tr class="even">
                <td><code>swagger.yaml</code></td>
                <td>The Swagger file that is generated when the API is created.</td>
            </tr>
            <tr class="odd">
                <td><code>api_params.yaml</code></td>
                <td>Contains environment-specific details.</td>
            </tr>
        <tr class="even">
            <td><pre><code>Sequences
        ├── fault-sequence
        ├── in-sequence
        └── out-sequence</code></pre>
            </td>
            <td>Contains the documents. To add a new document, add the document in the <code>Docs/FileContents/</code>directory.</td>
            </tr>
        <tr class="even">
        <td><pre><code>Docs
      |── FileContents</code></pre>
        </td>
        <td>Contains the documents. To add a documentation, add them to the <code>Docs/FileContents/</code>directory.</td>
        </tr>
        </tbody>
    </table>

    !!! note 
        **Changing the Default API Template**

        When you create an API Project, APIs are generated using the default template specified in `<USER_HOME>/.wso2apictl/default_api.yaml` file. Following is the default template used to generate API Projects.  

        ```bash
        id:
            providerName: admin
            version: 1.0.0
            apiName:
        context:
        type: HTTP
        availableTiers:
            - name: Unlimited
        status: CREATED
        visibility: public
        transports: http,https
        productionUrl: http://localhost:8080
        sandboxUrl: http://localhost:8081
        ```

        This file contains the same notation as the `<API-Project>/Meta-information/api.yaml`. Organization-specific common API related details can be put into this template file and shared across developers.

        To further finetune API creation, a custom API Definition file can be used. If you need to use a specific definition file when generating a certain API project, use the `--definition` or `-d` flag along with `apictl init` command. The custom definition file should be in YAML format only.

        **Generate APIs with Dynamic Data**

        When initializing an API Project, the CTL is capable of detecting environment variables in the default definition file or in the provided custom definition file and create the `api.yaml` with the dynamic data. A sample custom definition file is shown below.

        ```bash
        id:
            providerName: admin
            apiName: $APINAME
            version: $APIVERSION
        ```

        When executing the `apictl init` command, the CLI tool automatically injects the values to the API definition. If an environment variable is not set, the tool will fail and request a set of required environment variables on the system.

        In runtime, the CTL tool will automatically inject the environment variable values to the artifacts in the API project.

        !!! tip
            To make this work you will need to set up required environment variables according to your OS. In a Linux/Unix environment, it can be done using
            ```bash
            export APINAME=MyAPI
            export APIVERSION=1.0.0
            ```      

4. Open the `<API Project>/Meta-information/api.yaml` file. You can edit the mandatory configurations listed below.

    | Field                                        | Description                                             |
    |----------------------------------------------|---------------------------------------------------------|
    | `apiName`| The name of API without spaces.                         |
    | `context`| Context of the API in API Manager with a leading slash. |
    | `productionUrl` | Production endpoint for API.                            |
    | `sandboxUrl`| Sandbox endpoint for API.                               |

    For more information about the configurations, see the [api.yaml](https://gist.github.com/kasvith/01e704611b6c301f470ab0e3b5cb0607).

    **api.yaml**

    ``` bash
    id:
        providerName: admin
        apiName: "SampleAPI"
        version: 1.0.0
    type: HTTP
    context: "/samplecontext"
    availableTiers:
        - name: Unlimited
    status: PUBLISHED
    visibility: public
    transports: http,https
    productionUrl: http://localhost:8080
    sandboxUrl: http://localhost:8081
    ```


## Import an API Project

!!! info
    **Before you begin...** 

    -   Make sure you have already created an environment to which you are planning to import the API. If not, follow steps in [Add an Environment]({{base_path}}/learn/api-controller/getting-started-with-wso2-api-controller#add-an-environment).
    
    -   Make sure you have logged-in to the importing environment. If not, follow steps in [Login to an Environment]({{base_path}}/learn/api-controller/getting-started-with-wso2-api-controller#login-to-an-environment). 


!!! tip
    A user with `admin` role is allowed to import APIs. To create a custom user who can import APIs, refer [Steps to Create a Custom User who can Perform API Controller Operations]({{base_path}}/learn/api-controller/advanced-topics/creating-custom-users-to-perform-api-controller-operations/#steps-to-create-a-custom-user-who-can-perform-api-controller-operations).

After editing the mandatory fields in the API Project, you can import the API to an environment using any of the following commands.  

-   **Command**
    ``` bash
    apictl import-api -f <path to API Project> -e <environment> -k
    ```
    ``` bash
    apictl import-api --file <path to API Project> --environment <environment> -k
    ```
    ``` bash
    apictl import-api --file <path to API Project> --environment <environment> --params=<environment params file> -k
    ```

    !!! info
        **Flags:**  
           
        -   Required :  
            `--file` or `-f` : The file path of the API project to import.  
            `--environment` or `-e` : Environment to which the API should be imported.   
        -   Optional :  
            `--preserve-provider` : Preserve the existing provider of API after importing. The default value is `true`. 
            `--update` : Update an existing API or create a new API in the importing environment.  
            `--params` : Provide a API Manager environment params file (The default file is `api_params.yaml`.).   
                        For more information, see [Configuring Environment Specific Parameters]({{base_path}}/learn/api-controller/advanced-topics/configuring-environment-specific-parameters). 
            `--skipCleanup` : Leave all temporary files created in the CTL during import process. The default value is `false`.  
            
    !!! example
        ```bash
        apictl import-api -f ~/myapi -e production -k
        ```
        ```bash
        apictl import-api --file ~/myapi --environment production -k
        ```    
        ``` go
        apictl import-api --file ~/myapi --environment production --params prod/custom_api_params.yaml -k 
        ```
        
    !!! tip
        When using the `--update` flag with the `import-api` command, the CTL tool will check if the given API exists in the targeted environment. If the API exists, it will update the existing API. If not, it will create a new API in the imported environment. 

       
-   **Response**
    ``` bash
    Successfully imported API!
    ```  
