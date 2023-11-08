# yunikorn-preemption-example
A simple example to demo preemption.

## Steps:
   ```shell script
   cd example
   kubectl apply -f yunikorn-configs.yaml
   kubectl apply -f low-priority-job-fenced.yaml
   kubectl apply -f low-priority-job.yaml
   kubectl get pods | grep Running
   # Should have 10 pods in Running. (5 fenced, 5 low-priority)

   kubectl apply -f normal-priority-job.yaml
   kubectl get pods | grep Running
   # Wait 1~2 minutes for preemption to occur.
   # (5 fenced, 2 low-priority, 3 normal-priority)

   kubectl delete -f low-priority-job.yaml
   kubectl get pods | grep Running
   # (5 fenced, 5 normal-priority)

   kubectl apply -f high-priority-job.yaml
   kubectl get pods | grep Running
   # Wait 1~2 minutes for preemption to occur.
   # (5 fenced, 3 normal-priority, 2 high-priority)

   kubectl delete -f normal-priority-job.yaml
   kubectl get pods | grep Running
   # (5 fenced, 5 high-priority)

   kubectl apply -f normal-priority-job-with-9999-priority-class.yaml
   kubectl get pods | grep Running

   # Wait 1~2 minutes for preemption to occur.
   # (5 fenced, 3 high-priority, 2 normal-priority-job-with-9999-priority-class)

   #cleanup
   kubectl delete -f high-priority-job.yaml 
   kubectl delete -f normal-priority-job-with-9999-priority-class.yaml
   kubectl delete -f low-priority-job-fenced.yaml
   ```

