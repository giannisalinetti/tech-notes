# Running Infispan on OpenShift Origin

Create a new test project

```
oc new-project infinispan
```

Create a new ImageStream in the project

```
oc import-image infinispan-server --from=docker.io/jboss/infinispan-server --confirm
```

Create the app by using the ImageStream reference

```
oc new-app --image-stream=infinispan-server -e MGMT_USER=admin -e MGMT_PASS=redhat -e APP_USER=developer -e APP_PASS=openshift
```

So far, image will be pulled and the pod will be created in a short time.

## Exposing the route to the admin console.

The oc new-app command creates a DeploymentConfig and a Service. The Service exposes all the needed ports.

To expose the route to the admin console, we need to exposes the port 9990, the same port used by the WildFly admin console (yep, Infinispan is deployed in WildFly).

```
oc expose svc/infinispan-server --port=9990
```

Now we can inspect the created route:

```
oc get routes
```

And expect an output like this:

```
NAME                HOST/PORT                                             PATH      SERVICES            PORT      TERMINATION   WILDCARD
infinispan-server   infinispan-server-infinispan.192.168.122.172.xip.io             infinispan-server   9990                    None
```

For this lab I used the public wildcard DNS services from xip.io and my route url is infinispan-server-infinispan.192.168.122.172.xip.io.

