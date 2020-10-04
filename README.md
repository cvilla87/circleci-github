# CircleCI

**Pipelines**
Pipelines are a full set of configuration you run when you trigger work on your projects in CircleCI. Pipelines are the envelope around your workflows, which coordinate jobs.

**Parallel execution (jobs)**
Parallel execution only applies to jobs, and the way to do it is simply to list them inside the `workflow` section, not using the `requires` functionality, so they will be executed at the same time.

**Serial execution**
For steps: They are automatically run in serial; the order of the steps inside the job is taking into account.
For jobs: You have to use `requires` functionality in order to have them connected one after another.

```yml
    jobs:
      - checkout_code
      - install_app:
          requires:
            - checkout_code
```

**Filtering branch**
We can do that a specific job is run only under certain branch conditions, using either `only` or `ignore`.

```yml
    jobs:
      - build:
          filters:
            branches:
              ignore:
                - develop
                - /feature-.*/

      - deploy-stage:
          filters:
            branches:
              only: staging
```

**Workspaces - Sharing files between jobs**
In order to do this, there are 2 special actions related to workspaces, and they must be added inside `steps`. 
Every commit has its own workspace.

The first step is about persisting the workspace:
```yml
  - persist_to_workspace:
      root: ~/  # directory that we want to persist
      paths:
        - '*'
```

The second one is about using the previously created workspace:
```yml
  - attach_workspace:
      at: ~/  # where persisted workspace will be mounted
```

**Naming step**
```yml
  - run:
      command: ./manage.py test
      name: Test
```

**Ignoring build**
If commits messages have `[skip ci]`, the build in CircleCI will be skipped.

**Require 'passing status' checks**
To do this, you have to go to your Github repo, go to `Settings/Branches` and add a branch protection rule, the one with the name `Require status checks to pass before merging`, and finally select the rule from CircleCI.
