# Welcome

So, you've decided to try Codefresh? Welcome on board!

Using this repository we'll help you get up to speed with basic functionality such as: *compiling*, *testing* and *building Docker images*.

This project uses `Node Js, Postgres` to build an application which will eventually become a distributable Docker image.

## Looking around

In the root of this repository you'll find a file named `codefresh.yml`, this is our [build descriptor](https://docs.codefresh.io/docs/what-is-the-codefresh-yaml) and it describes the different steps that comprise our process.
Let's quickly review the contents of this file:

### Compiling and testing

To compile and test our code we use Codefresh's [Freestyle step](https://docs.codefresh.io/docs/freestyle).

The Freestyle step basically let's you say "Hey, Codefresh! Here's a Docker image. Create a new container and run these commands for me, will ya?"

```yml
  unit_test:
    type: composition
    working_directory: ${{main_clone}}
    composition:
      version: '2'
      services:
        postgres:
          image: postgres:latest
          environment:
            - POSTGRES_USER=$POSTGRES_USER
            - POSTGRES_PASSWORD=$POSTGRES_PASSWORD
            - POSTGRES_DB=$POSTGRES_DB
    composition_candidates:
      test:
        image: ${{build_step}}
        links:
          - postgres
        command: bash -c '/usr/src/app/test-script.sh'
        environment:
          - POSTGRES_USER=$POSTGRES_USER
          - POSTGRES_PASSWORD=$POSTGRES_PASSWORD
          - POSTGRES_DB=$POSTGRES_DB
    composition_variables:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=admin
      - POSTGRES_DB=todo
```

The `image` field states which image should be used when creating the container (Similar to Travis CI's `language` or circleci`s `machine`).

The `commands` field is how you specify all the commands that you'd like to execute

### Building

To bake our application into a Docker image we use Codefresh's [Build step](https://docs.codefresh.io/docs/steps#section-build).

The Build is a simplified abstraction over the Docker build command.

```yml
  build_step:
    type: build
    image_name: codefreshio/example-nodejs-postgress
    dockerfile: Dockerfile
    tag: ${{CF_BRANCH}}
```

Use the `image_name` field to declare the name of the resulting image (don't forget to change the image owner name from `codefreshdemo` to your own!).

## Using This Example

To use this example:

* Fork this repository to your own [INSERT_SCM_SYSTEM (git, bitbucket)] account.
* Log in to Codefresh using your [INSERT_SCM_SYSTEM (git, bitbucket)] account.
* Click the `Add Service` button.
* Select the forked repository.
* Select the `I have a Codefresh.yml file` option.
* Complete the wizard.
* Rejoice!
