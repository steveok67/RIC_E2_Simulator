# RIC_E2_Simulator
# Youtube video: https://www.youtube.com/watch?v=XCbYJ5TVAiM&ab_channel=SteveKonish

Demo Assumptions:  
a) assuming you have external access to Quay.io
   pre-built simulator container located here: quay.io/skonish/ran_sim
b) The files listed below are already on the server
   e2sim_server.yaml
   ricsim_client.yaml
c) SCTP is enabled on the cluster   

Demo Preparation:
1) oc project ricsim (on both windows)
2) delete the apps if they are already running
   oc get pods
   oc delete pod e2sim
   oc delete pod ricsim

Demo:

1) start the pods:
  $ oc create -f ./e2sim_server.yaml  (in terminal window 1)
  $ oc create -f ./ricsim_client.yaml (in terminal window 2)  
   
2) Verify the pods are running:

$ oc get pods
NAME     READY   STATUS    RESTARTS   AGE
e2sim    1/1     Running   0          62s
ricsim   1/1     Running   0          13s   
   
3) rsh to each pod:

$ oc rsh e2sim //terminal window 1
$ oc rsh ricsim //terminal window 2

4) Set the env variable so the apps find the xml message files:

# export E2SIM_DIR=/playpen/previous //terminal window 1
# export E2SIM_DIR=/playpen/previous //terminal window 2

5) In the e2sim window, determine the IP address for eth0
   ifconfig eth0
   10.129.0.12

6) Run the applications:

   e2sim rsh session:  (use the IP address from above) //terminal 1
     # ./previous/build/e2sim 10.129.0.12
     Start E2 Agent (E2 Simulator)
     [SCTP] Server started on 10.129.0.12:36421
     [SCTP] Waiting for new connection...
     
     
   ricsim rsh session:  //terminal 2
     # ./previous/build/ricsim 10.129.0.12
     
Note: if you get a segfault, it's from one of 2 reasons:
  1) the E2SIM_DIR didn't get set, so set it again in each window.
  2) the OCP cluster doesn't support SCTP (check the logs)    
