# yunikorn-preemption-example
A simple example to demo preemption.

## Steps:

   ```shell script
   kubectl apply -f yunikorn-configs.yaml
   kubectl apply -f low-prio-job.yaml
   kubectl get pods | grep Running
   kubectl apply -f normal-prio-job.yaml
   # Wait for preemption happened. (1~2 minutes, 5 low-prio pods will be opt-out.)
   kubectl get pods | grep Running
   kubectl delete -f low-prio-job.yaml
   kubectl get pods | grep Running
   kubectl apply -f high-prio-job.yaml 
   # Wait for preemption happened. (1~2 minutes, 5 normal-prio pods will be opt-out.)
   kubectl get pods | grep Running
   kubectl delete -f normal-prio-job.yaml
   kubectl get pods | grep Running
   kubectl apply -f prime-normal-prio-job-with-priority-class.yaml
   # Wait for preemption happened. (1~2 minutes, 5 high-prio pods will be opt-out.)
   kubectl get pods | grep Running

   #cleanup
   kubectl delete -f high-prio-job.yaml 
   kubectl delete -f prime-normal-prio-job-with-priority-class.yaml
   ```

