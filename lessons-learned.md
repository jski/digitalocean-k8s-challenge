# Learned from this experiment
In working with this experiment, I became more acquainted with the various kubernetes local installations available, and will definitely use kind more in the future, as it's very lightweight and simple to use.

The biggest takeaway is how easy it is to stand up a MongoDB instance using provider charts from Bitnami. This will definitely be useful to use as I'm also hoping to move away more from Docker local work, and knowing that k8s can be provisioned quickly for a test environment is great. As I've had some issues with local Python projects in the past using sqlite, I now have the opportunity to migrate some projects into k8s to run alongside a quick and easy MongoDB deployment, or to have that deployment standalone and reachable outside the cluster as well.