This repo shows a simple demonstration of using [ytt](https://carvel.dev/ytt/) and [yq](https://mikefarah.gitbook.io/yq) to generate docker compose file for multiple services targeting multiple environments.

### What is [ytt](https://carvel.dev/ytt/)?

It is a simple tool helps in shaping and templating YAML content, including, Kubernetes configuration, Concourse pipelines, Docker Compose files, etc. This is maintained by [CNCF](https://www.cncf.io/sandbox-projects/)

For documentation, refer [here](https://carvel.dev/ytt/docs/v0.46.x/), there are lot of samples with playground, which can be found [here](https://carvel.dev/ytt/#playground)

### Getting Started

> Note: This demonstration just uses very few of the features of [ytt](https://carvel.dev/ytt/), like data values, overlay, etc. in combination with [yq eval - command](https://mikefarah.gitbook.io/yq/commands/evaluate). There are lot of powerful feature that [ytt](https://carvel.dev/ytt/) tool contributes, to make use of. 

- Install [ytt](https://carvel.dev/ytt/) command line tool from [here](https://carvel.dev/ytt/docs/v0.46.x/install/)

- Install [yq](https://mikefarah.gitbook.io/yq/) command line tool from [here](https://github.com/mikefarah/yq/#install)

- The `default_templates` folder here contains the template for the docker-compose file and the defaults. In this case, there is a default file for docker-compose itself. The defaults include each services too, here as an example, there is redis and db for demonstration purpose.

- The other folders here `dev`, `prod`, etc. represents the target environments where it contains overlays as needed.

- To demonstrate the example, to create `docker-compose.yml` for `dev` environment, execute below code from the root. The key here is that you are using the `dev` folder along with the `default_templates`, where `dev` folder contains the overlay values corresponding to dev environment.

    ```sh
    ytt -f ./default_templates/ -f ./dev/ | yq eval '.services |= with_entries(.key = .value.container_name)'
    ```

- To demonstrate the example, to create `docker-compose.yml` for `prod` environment, execute below code from the root. . The key here is that you are using the `dev` folder along with the `default_templates`, where `dev` folder contains the overlay values corresponding to dev environment.

    ```sh
    ytt -f ./default_templates/ -f ./prod/ | yq eval '.services |= with_entries(.key = .value.container_name)'
    ```

- To create a `docker-compose` file from the output, execute the below command. This command targets the dev environment.

    ```sh
    ytt -f ./default_templates/ -f ./dev/ | yq eval '.services |= with_entries(.key = .value.container_name)' > docker-compose.yml
    ```
- To execute `docker compose up` from the produced output inline, execute the below command. This command targets the prod environment.
    
    ```sh
    ytt -f ./default_templates/ -f ./prod/ | yq eval '.services |= with_entries(.key = .value.container_name)' | docker compose -f- up
    ```

### Additional References

- Documentation from **Tanzu Development Center**, [Getting Started with ytt](https://tanzu.vmware.com/developer/guides/ytt-gs/)
- Refer this [docker-compose](https://github.com/UKP-SQuARE/square-core/blob/master/docker-compose.ytt.yaml) file for usage of [ytt](https://carvel.dev/ytt/) extensively.