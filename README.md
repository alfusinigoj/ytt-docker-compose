This repo shows a simple demonstration of using [ytt](https://carvel.dev/ytt/) to generate docker compose file for multiple services and for multiple target environments.

### What is [ytt](https://carvel.dev/ytt/)?

It is a simple tool helps in shaping and templating YAML content, including, Kubernetes configuration, Concourse pipelines, Docker Compose files, etc. This is maintained by [CNCF](https://www.cncf.io/sandbox-projects/)

For documentation, refer [here](https://carvel.dev/ytt/docs/v0.46.x/), there are lot of samples with playground, which can be found [here](https://carvel.dev/ytt/#playground)

> Note: This demonstration just uses very few of the features of [ytt](https://carvel.dev/ytt/), like data values, overlay, if, etc. There are lot of powerful feature that this tool contributes, to make use of. 

### Getting Started

- Install [ytt](https://carvel.dev/ytt/) command line tool from [here](https://carvel.dev/ytt/docs/v0.46.x/install/)

- The `template` folder here contains the template for the docker-compose file and the defaults. In this case, there is a default file for docker-compose itself. The defaults include each services too, here as an example, there is redis and db for demonstration purpose.

- The other folders here `dev`, `prod`, etc. represents the target environments where it contains overlays as needed.

- To demonstrate the example, to create `docker-compose.yml` for `dev` environment, execute below code from the root.

    ```sh
    ytt -f ./template/ -f ./dev/
    ```

- To demonstrate the example, to create `docker-compose.yml` for `prod` environment, execute below code from the root.

    ```sh
    ytt -f ./template/ -f ./prod/
    ```

- You can pipe the output to create a file or use it on the fly as below.

    ```sh
    ytt -f ./template/ -f ./dev/ > docker-compose.yml
    ```
    OR
    
    ```sh
    ytt -f ./template/ -f ./dev/ | docker compose -f- up
    ```