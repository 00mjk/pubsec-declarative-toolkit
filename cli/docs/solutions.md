# Solutions #

There are 2 aspects to way solutions are managed in arete. First there is a local `solutions.yaml` file that tracks all solutions that have been added to arete from either the global GitHub solutions library or by calling `arete solution get`.

 ## Listing available solutions ##

 If you wish to see which solutions have been added and / or are available to arete, simple run the command `arete solution list`
 
 ## Adding a solution ##

 If you wish to add a solution that is not listed in the global GitHub solution library you can create your own GitHub repo but a couple of required elements must exist:

 1. There must be a `solutions.yaml` file stored at the root of the GitHub path provided
 2. There must be a `Kptfile` in the root of the GitHub path provided. (arete uses [kpt](https://kpt.dev) to render and apply solutions)

 Once your solution is structured correctly you can simple call: `arete solution get https:/github.com/<GIT-USER>/<REPO-NAME>(/<GIT-SUBFOLDERS>)`

 - NOTE: If your solution is stored in a sub-folder of your repo, then just provide the complete path to the that folder to the get command.

## Solutions.yaml ##

The solutions.yaml file has the following structure:
```
solutions:
  - solution: <SOLUTION-NAME>
    description: <SOLUTION-DESCRIPTION>
    url: <SOLUTIONS-GITHUB-URL>
```

example

```
solutions:
 - solution: my-awesome-demo
   description: This is my awesome GCP Demo. You should use it
   url: https://github.com/milhouseisgreat/declarative-demo.git
```

 ### Removing a solution ###

 If you wish to remove a solution from the arete list then you will need to modify the locally cached `solutions.yaml` file. Find the file (defaults to your local home directory in a folder called `.arete`) and remove the solution from the yaml list

 ## Prompting the user ##

Since arete uses kpt to render the final deployments before applying the solutions we rely on the `Kptfile` pipeline process to determine if there is any information needed to be updated by user input. The `Kptfile` pipeline allows for mutators that can use a ConfigPath to a YAML file for setting values through out the solution.

Arete will search the YAML file listed in the ConfigPath for any single line comment pattern. This process looks simliar to the kpt setters function pattern.

If you want to prompt a user to update the value of templated value then add the comment `# arete-prompt` to the end of the key/value pair in any of your kpt mutators.