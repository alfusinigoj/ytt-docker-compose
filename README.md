This repo shows a simple demonstration of using [ytt](https://carvel.dev/ytt/) and [yq](https://mikefarah.gitbook.io/yq) to generate docker compose file for multiple services and for multiple target environments.

### What is [ytt](https://carvel.dev/ytt/)?

It is a simple tool helps in shaping and templating YAML content, including, Kubernetes configuration, Concourse pipelines, Docker Compose files, etc. This is maintained by [CNCF](https://www.cncf.io/sandbox-projects/)

For documentation, refer [here](https://carvel.dev/ytt/docs/v0.46.x/), there are lot of samples with playground, which can be found [here](https://carvel.dev/ytt/#playground)

> Note: This demonstration just uses very few of the features of [ytt](https://carvel.dev/ytt/), like data values, overlay, if, etc in combination with [yq eval - command](https://mikefarah.gitbook.io/yq/commands/evaluate). There are lot of powerful feature that [ytt](https://carvel.dev/ytt/) tool contributes, to make use of. 

### Getting Started

- Install [ytt](https://carvel.dev/ytt/) command line tool from [here](https://carvel.dev/ytt/docs/v0.46.x/install/)

- Install [yq](https://mikefarah.gitbook.io/yq/) command line tool from [here](https://github.com/mikefarah/yq/#install)

- The `template` folder here contains the template for the docker-compose file and the defaults. In this case, there is a default file for docker-compose itself. The defaults include each services too, here as an example, there is redis and db for demonstration purpose.

- The other folders here `dev`, `prod`, etc. represents the target environments where it contains overlays as needed.

- To demonstrate the example, to create `docker-compose.yml` for `dev` environment, execute below code from the root.

    ```sh
    ytt -f ./template/ -f ./dev/ | yq eval '.services |= with_entries(.key = .value.container_name)'
    ```

- To demonstrate the example, to create `docker-compose.yml` for `prod` environment, execute below code from the root.

    ```sh
    ytt -f ./template/ -f ./prod/ | yq eval '.services |= with_entries(.key = .value.container_name)'
    ```

- You can pipe the output to create a file or use it on the fly as below.

    ```sh
    ytt -f ./template/ -f ./dev/ | yq eval '.services |= with_entries(.key = .value.container_name)' > docker-compose.yml
    ```
    OR
    
    ```sh
    ytt -f ./template/ -f ./dev/ | yq eval '.services |= with_entries(.key = .value.container_name)' | docker compose -f- up
    ```

### Additional References

- Documentation from Tanzu Development Center, [Getting Started with ytt](https://tanzu.vmware.com/developer/guides/ytt-gs/)
- Refer this [docker-compose](https://github.com/UKP-SQuARE/square-core/blob/master/docker-compose.ytt.yaml) file for usage of [ytt](https://carvel.dev/ytt/) extensively.