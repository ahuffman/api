# ahuffman.api
An Ansible role to avoid having to fill in repetitive parameters when writing a playbook with multiple API calls.  This role wraps around the Ansible [`uri`](https://docs.ansible.com/ansible/latest/modules/uri_module.html#uri-module) module.  This helps reduce tedious playbook writing and makes the overall playbook more readable while reducing repetitive lines of code.

## Role Variables
Where the default value "omit" is noted, refer to the Ansible [`uri`](https://docs.ansible.com/ansible/latest/modules/uri_module.html#uri-module) module documentation for the inherited defaults.

|Variable Name|Required|Description|Type|Default|
|---|:---:|---|:---:|:---:|
|api_task_name|no|Name of the task to run the uri/api task with.|string|"API Task"|
|api_body|yes|Body of the request.|string|omit|
|api_body_format|no|The serialization format of the body.|string|"json"|
|api_client_cert|no|PEM formatted certificate chain file to be used for SSL client authentication.|string|omit|
|api_client_key|no|PEM formatted file that contains your private key to be used for SSL client authentication.|string|omit|
|api_creates|no|A filename, when it already exists, this step will not be run.|string|omit|
|api_dest|no|A path of where to download the file to (if desired).|string|omit|
|api_follow_redirects|no|Whether or not the URI module should follow redirects.|string|omit|
|api_force_basic_auth|no|The library used by the uri module only sends authentication information when a webservice responds to an initial request with a 401 status.|boolean|True|
|api_headers|no|Add custom HTTP headers to a request in the format of a YAML hash.|list|omit|
|api_method|no|The HTTP method of the request or response. It MUST be uppercase.|string|"GET"|
|api_others|no|All arguments accepted by the file module also work here|list|omit|
|api_pass|no|Password for your API request user|string|""|
|api_removes|no|A filename, when it does not exist, this step will not be run.|string|omit|
|api_return_content|no|Whether or not to return the body of the response as a "content" key in the dictionary result.|boolean|omit|
|api_status_code|no|Comma separated list of valid, numeric, HTTP status codes that signifies success of the request.|string|"200"|
|api_timeout|no|The socket level timeout in seconds|string|omit|
|api_base_url|yes|The base URL that you would like to perform requests against|string|""|
|api_url_path|no|The extended path of the URL that you would like to perform requests against|string|""|
|api_user|no|User for your API request|string|omit|
|api_validate_certs|no|Whether or not to validate SSL certificates|boolean|omit|
|api_register_var|no|Variable to register task results.|string|"api_results"|

## Example Playbook
```yaml
    ---
    - hosts: "all"
      vars:
        api_base_url: "https://myserver.mydomain.com"
        api_user: "someuser"
        api_pass: "strongp@$$" #you should store this in a ansible-vault
      tasks:
        - include_role:
            name: "api"
          vars:
            api_task_name: "Get some data from webservice"
            api_url_path: "api/hosts"
            api_body: "{\"search\": \"myhostname.mydomain.com\"}"  #for large bodies, use a template lookup here
            api_register_var: "host_query"

        - name: "Output results"
          debug:
            var: "host_query"
```
## License
[MIT](LICENSE)

## Author Information
[Andrew J. Huffman](https://github.com/ahuffman)
