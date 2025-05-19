# Ex.No: 9  Implementation of Simple Reinforcement Learning 
### DATE:                                                                            
### REGISTER NUMBER : 
### AIM: 
To write a program to implement  Reinforcement learning  in Unity 
### Algorithm:
```
1.Create a new 3D Unity project
2.Create a plane → Right-click Hierarchy > 3D Object > Plane
3.Create an Agent (Cube)
4.3D Object → Cube → Rename to Agent
5. Add Rigidbody (disable gravity if needed)
6. Create a Target (Sphere)
7.3D Object → Sphere → Rename to Target
8.Create an empty GameObject → Area (to reset Agent and Target positions)
9.Add the Behavior Parameters component to your Agent
10.Behavior Name: MoveToTarget
11. Vector Observation: 7 (3 for agent pos + 3 for target pos + 1 for velocity), 
Action Space: Continuous (2)
12. run the command 
mlagents-learn config.yaml --run-id=move-to-target --train
```  
### Program:
```
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;
public class RollerAgent : Agent
{
    private Rigidbody rBody;
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            // If the Agent fell, zero its momentum
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        // Move the target to a new spot
        Target.localPosition = new Vector3(Random.value * 8 - 4,
                                           0.5f,
                                           Random.value * 8 - 4);
    }

    public override void CollectObservations(VectorSensor sensor)
    {
        // Target and Agent positions
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);

        // Agent velocity
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }

    public float speed = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * speed);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if (distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }

        if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }

    public override void Heuristic(in ActionBuffers actionsOut)
    {
        actionsOut.ContinuousActions.Array[0] = Input.GetAxis("Horizontal");
        actionsOut.ContinuousActions.Array[1] = Input.GetAxis("Vertical");
    }
}
```
### Output:

![WhatsApp Image 2025-05-19 at 13 25 54_70897c03](https://github.com/user-attachments/assets/a6155c9e-56c0-46cd-8ebb-1d08860e0968)








### Result:
Thus the AI character was trained using reinforcement learning.
