# Helm quickstart

Navigate to the helm folder
```
cd helm
```

## Inspecting the sample app
The sample app in the bakery-app/ folder contains a simple go http server that serves cakes.

It's a small program, and a small Dockerfile. The built image can be pulled from `europe-docker.pkg.dev/stready-demo/stready/bakery-app`

## Adding a helm chart

A common strategy for managing service-specific charts is to keep the chart inside the service repo, such as inside a `deployment` folder. Let's pretend the `bakery-app` folder is the service repo and add a helm chart to it. It's common to give service-specific charts the same name as the service.

Create the deployment folder and a helm chart inside it:
```
mkdir bakery-app/deployment
cd bakery-app/deployment
helm create bakery-app 
```

Your result should be something like this
```
$ tree ..
..
├── deployment
│   └── bakery-app
│       ├── charts
│       ├── Chart.yaml
│       ├── templates
│       │   ├── deployment.yaml
│       │   ├── _helpers.tpl
│       │   ├── hpa.yaml
│       │   ├── ingress.yaml
│       │   ├── NOTES.txt
│       │   ├── serviceaccount.yaml
│       │   ├── service.yaml
│       │   └── tests
│       │       └── test-connection.yaml
│       └── values.yaml
├── Dockerfile
├── go.mod
├── main.go
└── README.md
```

## Inspecting the created helm chart
The main files you should pay attention to for this quickstart are:
* `deployment.yaml`
* `service.yaml`
* `values.yaml`
* `Chart.yaml`

In this quickstart we will only be changing the `values.yaml` and a tiny change in the `deployment.yaml`

## Inspecting the generated kubernetes mainfests

Helm is both a templating tool for kubernetes manifests and a deployment management tool.
To see what the templates for the default chart outputs you can run:
```
helm template bakery-app ./bakery-app           # add "> debug.yaml" at the end to write to a file you can inspect
```

## Deploying the default helm chart

The default helm chart is ready to be deployed, it's just not configured to deploy the `bakery-app`. Instead it deploys `nginx`, which is nice to test that everything works as it should.

Deploy the default helm chart (nginx) by running
```
helm install bakery-app ./bakery-app
```

In helm terms you have now created a "release". Run `helm ls` to list your releases.

You can see pods running or being created by running:
```
kubectl get pods
```

## Configuring the helm chart for the bakery-app

The bakery app is fairly similar to `nginx`, except that it runs on port 8080 and that it's pulled from a different container repository

To configure the helm chart bakery-app make the following changes:
* In `bakery-app/values.yaml` under `image:` change `repository: nginx` to `repository: europe-docker.pkg.dev/stready-demo/stready/bakery-app`
* In `bakery-app/Chart.yaml` change the `appVersion` to `"0.1.0"`
* In `bakery-app/templates/deployment.yaml` search for `containerPort` (ctrl+F in Cloud Shell Editor) and change it from `80` to `8080`

Real-world considerations:
* The chart version and the app version in `Chart.yaml` track separate things. Every time you update the chart (any change inside the chart folder) you should bump the chart version (semver style), while the app version should follow the version for your deployed app, which in this case is the version for `bakery-app`. Keep in mind that bumping the app version is a change to the chart, and warrants a chart version bump.
* Managing configurations or differences between environments are done by having yaml files with environment specific values outside of the chart folder. A good place to put them is inside the `deployment/` folder next to the chart. You would then run helm with `--values=prod-values.yaml`, which would overwrite the defaults set in `values.yaml` inside the chart.

## Deploying the sample app
You can apply your changes to the helm release by upgrading it using this command:
```
helm upgrade bakery-app ./bakery-app
```

To access the web server port-forward traffic to the cluster with this command
```
kubectl port-forward service/bakery-app 8080:80
```

If you are in the Cloud Shell Editor you can hit "Web Preview" and you'll be able to access ports on your Cloud Shell instance.

Hit Ctrl+C to stop port-forwarding and return to your shell


Real-world considerations:
* When deploying applications to a production cluster deploying with `helm upgrade --install` (upgrade, or install if not there) and `--atomic` (succeed or rollback on any failure) is useful for making your commands idempotent. This is especially useful when ran from CI/CD.

You now have a deployed helm release of the bakery-app deployed to a kubernetes cluster with all infrastructure managed by terraform. The main structure is in place but there are a few more things you need to do for this to be ready for production, such as autoscaling, ingress configuration, and your app probably needs a few more changes to the helm chart.

You can now proceed to the cleanup section

# Contents
* [1. Getting started with Terraform](/terraform/)
* [2. Getting started with Helm](/helm/)
* [3. Cleanup](/cleanup/)
