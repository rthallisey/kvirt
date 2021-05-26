# kvirt

kVirt is a scale and performance test framework for KubeVirt.
The purpose of kVirt is to avoid the hardware burden with mock workloads in
order to stress the control plane.

### Extending the Kubernetes API for Tests
Pods and VirtualMachineInstances are Kubernetes API extentions, so in order to do
mock testing, we'll create a new API extention for 'fake pods', kPods, that will
act like pods, but without the runtime.

### Controller Runtime
The kvirt project leverages the controller runtime project to create controllers
for the mock KubeVirt components and mock Pods.

*kvirt-handler* - controller that fakes the virt-hander by reconciling a VMI object's status
*kPod* - controller that fakes the kube-scheduler by reconciling a Pod's status

Controller Runtime provides a test framework for integration testing the Reconcile
function.  It consists of a real API server and ETCd.
https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/envtest

### What this Project Does and Does Not
It's important to clarify that this project isn't a perfect measurement for scale
and perf testing.  There's no substitute for the real thing.  The focus of this
project is to create an estimate of performance and scale and provide an achievable
way to measure code across KubeVirt releases with a small hardware footprint.

Does:
- Make real API calls
- Move real YAML
- Test virt-controller code paths
- Use a real Kubernetes API server and ETCd

Does not:
- Have a runtime
- Test virt-handler code paths
- Test virt-launcher code paths
- Test Libvirt code paths
